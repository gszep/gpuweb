WebGPU has multiple major concepts with the word "multi" in them. This explainer attempts to
describe each one and the relationships between them.


## Relationships between things

- **One adapter : many devices.**
    Multiple independent devices can be created on each adapter.
- **One device : many queues.**
    Each queue is a part of a single device.
- **Many threads : many devices.**
    Each thread may use any set of devices.
- **Many threads : many queues.**
    Each thread may issue work to any set of queues.


## Multi-Device

From the spec:

> A **device** is the logical instantiation of an adapter, through which internal objects are
> created. It can be shared across multiple agents (e.g. dedicated workers).

"Multi-Device" refers to the ability for a webpage (or multiple independent widgets/libraries on
a single webpage) to create multiple WebGPU devices. These may or may not all be on the same
adapter (see below), but are completely independent entities with no implicit interactions.

### Why?

A webpage may have multiple independent widgets/libraries running concurrently. Each of these
widgets/libraries should not have to consider the possible actions of others on the same webpage
- they want to assume their behavior has no dependence on separate work on the page.

Today, this is achieved in WebGL by creating separate WebGL contexts (each from a different canvas).

### What's missing?

Nothing.

**Multi-Device Resource Sharing:**
There's a possibility of some kind of sharing of resources between WebGPU devices, but for almost
all use cases the desired result can be achieved by just using a single WebGPU device for the two
subsystems that want to share resources, as WebGPU devices are stateless (unlike WebGL contexts).
The remaining use case is sharing resources between devices on different adapters, which can
probably be achieved by interop with existing Web platform mechanisms (like ImageBitmap).


## Multi-Adapter

From the spec:

> An **adapter** represents an implementation of WebGPU on the system. Each adapter identifies
> both an instance of a hardware accelerator (e.g. GPU or CPU) and an instance of a browser's
> implementation of WebGPU on top of that accelerator.

"Multi-Adapter" refers to the ability of a webpage to issue work to more than one of the system's
hardware adapters at the same time.

### Why?

This is most important to compute applications which want to extract as much compute power out of a
system as possible.

This is not possible today in WebGL, unless a UA gives different adapters for different
`powerPreference` values.

### What's missing?

Today, `GPU.requestAdapter` takes only a single option, `powerPreference`, and that option is a hint
(not a requirement). On a dual GPU system, it's likely but not guaranteed that `"low-power"` and
`"high-performance"` will give you two different hardware adapters.

An API will be proposed for enumerating a list of adapters. This would be the best way for a webpage
to guarantee that it's seeing all of the adapters available to it. (Note that the user agent can
arbitrarily decide what adapters are actually exposed to the webpage - so having API surface for
adapter enumeration does not reduce the amount of fingerprinting protection the UA can provide.)

**Explicit Linked Display Adapters**:
This refers to the ability to explicitly program for multiple hardware adapters in SLI/Crossfire.
It's an extremely niche feature, and there are currently no plans to investigate its inclusion in WebGPU.


## Multi-Threading (JavaScript)

[Investigation](https://github.com/gpuweb/gpuweb/issues/354#issuecomment-511616037).

We say "Multi-Threading" to refer to the ability to use a single WebGPU device from multiple
JavaScript "threads" (the main thread, dedicated workers, shared workers, etc.) This allows
issuing WebGPU work on a single device from multiple threads in parallel.

The proposed model for this is to make most WebGPU objects be sharable
between threads, just like SharedArrayBuffer. They can be "serialized" and "deserialized" for
`postMessage`, which creates a new handle object (e.g. `GPUBuffer`) on the receiving thread
referring to the same underlying WebGPU `buffer` object.

Most of these objects are immutable, so the fact that they are shallow-cloned is not really
observable. However, some objects have mutable internal state, like the buffer/texture
"destroyed" state and buffer mapping state.

These states are observable only through WebGPU function calls (submit, map, unmap).
**Note:** Only CPU-side state is synchronized. Resource contents exist purely on the GPU
device itself, and accessed from queues, so they are unaffected by any CPU threading.
Competing usage of GPU memory would be possible with multi-queue (below).

### Why?

Multi-threading allows CPU-side issuing of WebGPU work to be parallelized across CPU cores.
It also provides thread-level concurrency.
(Note that JavaScript natively provides single-thread concurrency).

### What's missing?

- Specification of shared state.
- Implementations. For ease of implementation, some early implementations may push WebGPU
    operations through a single message channel, resulting in some synchronization overhead
    that slightly reduces the parallelism of work issuance from multiple threads. However,
    there is still a significant gain because other JavaScript work still runs in parallel.

## Multi-Threading (Synchronous / WebAssembly)

An additional desired multi-threading capability is the ability for a thread to use a new object
without having to request and asynchronously wait for a currently-owning thread to send the
object (via `postMessage`).

### Why?

JavaScript threads are used to implement WebAssembly threads, so multi-threading abilities are
necessary for multi-threaded WASM apps to use WebGPU from multiple WASM threads. However, the
programming model of WASM is also different from JavaScript: Where JavaScript requires a thread
to `postMessage` an object to a receiving thread in order for the receiving thread to see the
object, WebAssembly applications are written in native languages that assume all objects are "raw
data" which can be used from any thread at any time (or at least synchronously shared or moved
between threads). WebAssembly achieves this for raw data by sharing a memory space between
JavaScript threads via SharedArrayBuffer. However, WebGPU objects are not representable as raw
data, as they refer to objects outside the WebAssembly virtual machine.

Many JavaScript applications with synchronous architectures (especially rendering engines) would
also benefit significantly from these facilities.

### What's missing?

A way for objects to be shared synchronously in bulk between many threads of a multi-threaded application.

<details>
<summary>
    Many possibilities have been considered, but the best approach hasn't been figured out yet.
</summary>

- A "shared object table": The sending thread puts an object in the table, synchronize with the
    receiving thread via shared memory (SharedArrayBuffer atomics), at which point the receiving
    thread can take it out of the table immediately.

- A new synchronous "receiveMessage" on MessagePort which can be called as soon as a message
    has been sent by the sending thread. Such messages could be identified by an integer that can
    be shared via SharedArrayBuffer.

- "Export integer handle" and "import integer handle". The sending thread would call a method on
    a WebGPU object that creates a strong reference to the object and returns an integer. The
    receiving thread would call a static method or GPUDevice method that conjures up a local
    reference to that object. This has issues with garbage collection - strong references can
    easily leak; if the references are made weak, then garbage collection becomes observable.

In all of the above cases, the sending thread could make objects available to other threads upon
creation, allowing the receiving threads to find them at any time (without requesting code to be
run on the sending thread).
</details>


## Multi-Queue

[Investigation](https://github.com/gpuweb/gpuweb/issues/1065).

WebGPU Queues are queues of work to be executed on the actual GPU hardware.

In native APIs, queues can be of several types:

- render/compute/copy
- compute/copy
- copy only

Queues represent several aspects of GPU hardware/driver architecture:

- Each queue has independent ordering (except when synchronization is added).
This allows the software or hardware scheduler to pick from the front of any of the available queues
at any time. For example, rendering workloads typically leave some "holes" in the occupancy of GPU
cores, which a scheduler can fill with other rendering or compute work. Most typically, this is
applied via "async compute", in which compute work is issued on a separate "compute-only" queue to
fill any unused occupancy.

- Some hardware has specific subsystems for non-programmable memory operations, like memory copies
between textures and buffers (B2B/B2T/T2B/T2T).
By enqueuing these operations separately, an application allows the system to
schedule these operations on separate hardware subsystems without creating synchronization
bottlenecks/overhead with other subsystems.

WebGPU already exposes one queue object per device (`GPUDevice.defaultQueue`).

**Note:** Queues do not directly provide GPU parallelism or concurrency.
All programmable GPU work is structured to be parallelizable across GPU cores, and it is already
possible for independent work on a single queue to be interleaved or run out-of-order at the
discretion of the hardware/driver schedulers, within the constraints of ordering guarantees
(which, in native APIs, are implicit or provided by the application via barriers).
Multi-queue only improves the occupancy of available hardware on the GPU when the two tasks at
the front of two queues can better occupy hardware resources than just one of the tasks would.

### What's missing?

See the investigation and open proposals for details.

- Investigation and design.
- A way to create more WebGPU queue objects (of different types).
- (Maybe) a way to query the geometry of the adapter.
