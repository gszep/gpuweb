From [Minutes for the 2017-07-26 meeting](https://lists.w3.org/Archives/Public/public-gpu/2017Jul/0004.html).

> DJ: what are next steps?
> 
> Should Corentin or someone define an API? Or move on with the assumption that we have an agreement of the overall shape of this part? 
> 
> KR / MM: seems too early to define an API
> 
> DM: agree, also tied in to memory barriers
> 
> CW: maybe we can make a comprehensive list of things we want to talk about before looking at the shape of the API
> 
> DJ: Could do this on the wiki/github
> 
> MM: an issue per issue, then close them?
> 
> DM: Github milestones?
> 
> KR: spreadsheet?
> 
> DJ: volunteers?
> 
> MM: I can start!

("MVP" means "Minimum Viable Product" and refers to a version 0 of the API.)

- Consensus: Don't include every single feature from WebGL
- Consensus: We're willing to break source compatibility with the MVP in a subsequent version (but we'll try not to).
- Consensus: MVP will include facilities to have multiple extensions, but only a single noop extension will be present
- Consensus: WebGPU aspires to be compatible with WebVR
- API Shape:
  - Consensus: API will be in Javascript
  - Open Question: Will we also include a WebAssembly API?
- Hardware and software targets
  - Consensus: The vast majority of device with any version of Vulkan (that functions correctly) should be able to run WebGPU. (meaning some feature flags might be required if they are widespread)
    - Open Question: Which Vulkan extensions / SPIR-V capabilities should be required? Which do we not want to require?
  - Consensus: Any device with any version of Metal should be able to run WebGPU, with any feature level
  - Consensus: Any device with Direct3D 12 or later should be able to run WebGPU, with any shader model and any feature level
- High level object model (Relationship between buffers, queues, renderpasses, etc.)
  - Consensus: Root API object is the Device (or Context or whatever we end up calling it)
    - Consensus: It's possible to create one of these with no arguments. You get the default one.
    - Consensus: You connect a WebGPU device to one or more canvases. You don't use the canvas's context to do drawing.
    - Consensus: It should be possible to do compute work without a canvas at all
  - Consensus: There should be at least one type of queue: a queue that can perform rendering, compute, and memory operations
  - Open question: MVP may only allow one instance of a queue object
  - Open Question: We may want additional types/capabilities of queues
  - Consensus: Queues will need to be created at device creation time
- Render Passes
  - Consensus: Metal’s render encoders and Vulkan’s render sub-passes will be encapsulated in the same API object (casually referred to as a “render pass”)
  - Consensus: All rendering must be done while a render pass is “open”. No compute may be done while a render pass is “open.” Only one pass may be open at a time.
  - Consensus: MVP will include compute facilities
    - Consensus: Compute passes need to be opened and closed too
  - Consensus: MVP will include copy/blit facilities
    - Open Question:  Do Copy/blit passes need to be opened and closed too?
  - Open Question: Should consecutive render passes inherit state?
  - Consensus: The destination set of textures you’re drawing into can only change at a render pass boundary
  - Open Question: Should render passes include synchronization dependencies?
  - Consensus: Don't include pipeline caching (derivative pipelines) in the MVP
- Rendering features
  - Consensus: MVP includes facilities to rendering to multiple render targets in a single draw call
  - Consensus: Draw commands should support instancing.
  - Consensus: MVP does not include the ability for the draw-call arguments (like the number of vertices, etc.) to come from a buffer, but the v1 will.
  - Open Question: Should the MVP include a way to create mipmaps for an existing texture
  - Consensus: Don't include a way to update the contents of a buffer from immediate operands in the command stream
  - Consensus: Include multisampling in the MVP
    - Open Question: What is the model for how multisampling works?
  - Consensus: MVP doesn't let two distinct resources be backed by the same memory
- Binding model
  - Consensus: Generally, use Vulkan’s binding model: A 3-layered hierarchy: Descriptors, a set of descriptors, and a (single) collection of sets.
    - Open Question: D3D has to keep samplers separately from all other resources
    - Open Question: How does allocation of descriptor sets and pooling of descriptors work in D3D?
  - Open Question: Should the MVP support resource heaps
  - Consensus: The GPU won't be able to change the set of visible resources to a shader
  - Consensus: Resources cannot hold references to other resources.
- Pipeline States
  - [Pipeline design doc](https://github.com/gpuweb/gpuweb/blob/master/design/Pipelines.md).
  - Consensus: In general, include the union of all 3 platform API’s pipeline state
  - Consensus: Includes depth/stencil state
  - Consensus: Does not include sample mask
  - Open Question: Should Renderpass information be included? (Vulkan backends would need to create dummy renderpasses)
  - Consensus: Separate blend state is included (thereby requiring a Vulkan extension)
  - Consensus: Does not include per-face stencil state
  - Consensus: MVP does not include depth bounds test
  - Consensus: Render targets and rasterizer need to have the same sample count. This sample count is specified in the pipeline state.
  - Consensus: MVP does not include an explicit pipeline caching API
  - Consensus: MVP does not include derivative pipelines
  - Consensus: Pipeline state object will have to include the format of the index buffer (int16 or int32) to make primitive restart sentinel value work
  - Open question: Should the API make a distinction between having a depth test which always passes and disabling the depth test?
  - Open question: Should there be an extra bool to disable independent blending, or should it be implicit from the blend attachments?
- Synchronization
  - Consensus: Fences will have 1 bit of information in them
  - Consensus: It should be difficult to have undefined behavior
  - Consensus: It is impossible to perform synchronization within a single WebGPU subpass. All synchronization happens at subpass boundaries. (Therefore, synchronization is explicit from the author)
    - Open Question: How much information does the programmer need to specify at a subpass boundary in order to help out the implementation?
  - Open Question: How far should we go to eliminate non-portable behavior
    - Consensus: It is impossible to guarantee portable behavior in all situations (because of UAVs)
- Resources
  - Consensus: GPU-visible buffers will have to be aligned
    - Open Question: Aligned to what?
  - Consensus: You can't synchronously upload encoded content (image or video) to a WebGPU content. (Decodes into an ImageBitmap need to be done ahead-of-time).
  - Consensus: There is a way to upload data to a buffer such that you are guaranteed that any subsequent draw calls see the contents of this buffer (for WebVR)
- Shading Language
  - Consensus: MVP's shading language should be the shading language for all subsequent versions
  - Open Question: Should the API accept human authorable source? Should it accept bytecode? Both?
  - Consensus: Some text form of the shading language would need to be specified (not just a binary form)
  - Consensus: The shading language must be no more expressive then SPIR-V in Logical Addressing Mode
  - Consensus: Language must abide by the browser's Same Origin policy.
  - Consensus: Implementations are free to kill shaders at an implementation-dependent time for running too long
  - Consensus: Shading language should be based off of either SPIR-V, WSL, or HLSL
- Resource uploads/downloads to/from device
  - Consensus: Uploads/downloads will be submitted to the queue between adjacent passes
    - Open Question: Should uploads/downloads actually be its own type of pass? (like a render pass or compute pass)
  - Consensus: Uploads/downloads will have the same synchronization type of mechanisms as the rest of the API
  - Open Question: Can data for an upload be supplied directly, or does it first need to be owned by another API object before it can be used?
  - Open Question: What mechanism should be used to let the app know that a download has completed? Promises? An API object that can be polled?
  - Open Question: Should a WebGPU resource object be able to represent a collection of platform-API resources? Or, does a WebGPU resource object have a set of capabilities that the author needs to react to? (like "able to be sampled from" or "able to be read from the CPU")
  - Open Question: Should a WebGPU application need to know how many backbuffers there are in the canvas framebuffer?
- Threading model
- Consensus: Don't include bundles / secondary command buffers in the MVP
- Consensus: Don't include stream-out / transform feedback in the MVP
- Consensus: Don't include predicated rendering in the MVP
- Consensus: Don't include tessellation in the MVP
- Consensus: Don't include sparse resources in the MVP
- Consensus: Don't include queries (Performance, or fragment pass count & similar info) in the MVP