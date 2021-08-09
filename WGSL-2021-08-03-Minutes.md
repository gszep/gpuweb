# WGSL 2021-08-03 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.



---



# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting** (probably incomplete this week, I couldn’t catch everyone - JB)**:**



* Apple
    * **Myles C. Maxfield**
    * **Robin Morisset**
* Google
    * **Alan Baker**
    * **Antonio Maiorano**
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * David Neto
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * **James Price**
    * Rahul Garg
    * **Ryan Harrison**
    * Sarah Mashayekhi
* Intel
    * Narifumi Iwamoto
    * Yunchao He
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * **Dzmitry Malyshau**
    * **Jeff Gilbert**
    * **Jim Blandy**
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* **Dominic Cerisano**
* Eduardo H.P. Souza
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Pelle Johnsen
* Timo de Kort
* Tyler Larson



---



# 📢 Announcements


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)
        * 


## 

---



# ⏳ Timeboxes


### [wgsl: detailed semantics of integer division and remainder #1830](https://github.com/gpuweb/gpuweb/pull/1830)



* JG: Let’s just pick x/0=x for signed or unsigned for now, until someone has a better idea.


### [Matrix initialization #1964](https://github.com/gpuweb/gpuweb/issues/1964)



* rowMajorMatMxN?
* JG: Maybe it’s useful to have the mat4 that’s implicitly col-major, and then an explicit builtin mat4x4_row_major()?
* DM: Do we actually want that? If we had row-major functions, people would ask for column-major functions. So, 2 constructors for each matrix.
* DM: We can construct a matrix from elements or vectors. Each matrix would need 2 additional functions, one for vectors and one for elements. That’s a lot of functions. Maybe we don’t need them.
* AB: We don’t need one for elements at all? Or just row major?
* DM: We could get away without row major, for MVP
* AB: I thought you wanted row major
* JG: We’re debugging people coming from HLSL and getting it wrong.
* DM: Wasn’t the argument that they have to make a bunch of fixes anyway? If they have matrix[1], they will have to fix that as well.
* AB: we’re not supporting row major matrix, in terms of access operations
* JG: Let’s add the scalar constructor. it will be column major
* RESOLVED: Add column-major element constructor for matrices


### [[wgsl]: Add fmod(), frem() builtins #1913](https://github.com/gpuweb/gpuweb/pull/1913)



* BC: The modulo operator for f32. We kept it open because David wanted builtins as well. He’s on vacation, though. Maybe we just punt
* JG: I was told we have someone who speaks for david.
* AB: I remember that not everyone wanted these builtins
* DM: I was against them. One of the builtins was going to have to be implemented/polyfilled everywhere except spir-v. It has a very limited use. One is not useful. The other one is represented by an operator. Maybe we don’t need either.
* DM: We can punt if we want
* AB: We think there is value in having them. They’re used for different things.
* AB: It’s not a difficult polyfill
* DM: It would help if the SPIR-V didn’t end up being a polyfill anyway, in the driver.
* MM: Yes. If it ends up being slower, that decreases the case for having them.
* AB: Let’s defer for now
* RESOLVED: Defer until post-MVP


### [wgsl: array size may be module-scope constant (possibly overridable) #1792](https://github.com/gpuweb/gpuweb/pull/1792)



* (Rebased, and updated to incorporate #1135 (allow array element count to be unsigned), and a bunch of cleanups)
* Further discussion: allow this only the case where:
    * The array type is the store type for a workgroup variable. (only the outer dimension. (Then can be implemented in MSL as a pointer-to-local entry point param.)
*  [Deferred until next week. Myles didn’t do his homework]


### [Issue 1534's array types differ on stride #1943](https://github.com/gpuweb/gpuweb/pull/1943)



* RM: I’ve been trying to understand this. Is it saying that arrays with different strides are the same types? Or the same types? What about runtime sized arrays?
* DM: This is saying they are different types. Runtime sized arrays cannot be assigned or passed as arguments.
* AB: We’re still discussing internally.
* AB: We are discussing whether or not an attribute should have the power to be part of the identity of a type.
* DM: You’ve been on a crusade against attributes on types
* AB: There are only 2 left! Maybe we can get rid of [[block]]!!
* MM: YES PLEASE
* BC: That would be great but it’s hard
* AB: THere might be ways of working around it, but describing it would exceed the time sandbox
* BC: What does naga do? Are they different types?
* DM: They are different types
* BC: That matches us
* AB: If we pull the attribute into the type, that’s a larger change.
* [Deferred until next week. Google needs more internal discussion]


### [wgsl: can hex floats explicitly represent infinity and NaN as float literals? #1769](https://github.com/gpuweb/gpuweb/issues/1769#issuecomment-879990010)



*  RM: Sorry for commenting so late. The shorter version is: If the programmer goes through the trouble of using the hex notation, they know what they’re doing, they want to control every bit on the float. Ignoring half those bits is not what they want. We’d recommend that if the type doesn’t match the number of bits, it should be an error
* JG: for hex floats, is 0.A, is that 1/10th?
* JB: 0.A is 10/16ths
* JG: We can reliably tell how many bits of precision are being handed to us.
* JB: you’re writing out the bits that will appear in the mantissa
* JG: What if you supply 24 bits instead of 24 bits in the mantissa?
* JB: Robin is proposing that’s illegal
* AB: Our floating point rules say denormals may be flushed. Is that converted? It’s not a bigger problem if too much precision is thrown out as a static error
* JG: It sounds tempting to do this for extra precision, because it’s possible.
* JG: I’m not convinced you can do this validation. We’re writing out the number that will be decoded into the mantissa
* MM: What if someone just happens to write the value for a NaN?
* [general discussion]
* Everyone: It ends up being an error
* E.g. `0x1ffp10, 0X0p-1, 0x1.p0, 0xf.p-1, 0x0.123p-1, 0xa.bp10l`
* JG: Errors are a good value add because people are reaching from floats
* AB: In the future we can discuss making the as_bits() function constexpr, so it can be used at module scope
* RESOLVED: When writing hex float literals, it is impossible to spell infinity or nan, or values larger than the max float, or values between representable floats [and doing so would be a compile error]
* BC: fxc is weird around inf and nan. We’re seeing broken behavior, which some flags can partly work around it. We see things like optimization assuming finite values.
* AB: You can’t use infinity as a sentinel value, because the compiler might just delete it.
* JG: That’s bad
* AB: you can load it, but once you start doing math, the implementation can assume it doesn’t exist
* JG: Can we recategorize things as “respects” vs “doesn’t respect”?
* AB: I’d have to check
* AB: isinf() and isnan() might not work how you think they’re going to work. FYI. These may not work.
* JG: If they may not work, we should just not have them.
* AB: That is open to debate.
* MM: They work on metal
* JG: We don’t want to be in a situation where a class of algorithm just doesn’t work on a class of devices.
* AB: You’d have to use precision in HLSL on everything, and on Vulkan you’d have to have a special execution mode. I don’t know how well that’s tested.


## 

---



# **⚖️ **Discussions


### [Out of bounds behaviour and atomics #1868](https://github.com/gpuweb/gpuweb/issues/1868) 



*  JG: I’m not convinced by what I wrote
* AB: The spec has a problem. It only talks about loads and stores. Maybe it at least needs something saying atomic operations are atomic. The way it describes stores, the store may not occur… it would be bad if half the atomic occurs. It should say the atomic operates on a single address and is atomic
* JG: We should aim for that
* JG: If you access out of bounds, fundamentally you’re getting the address of something, and you do the operation on that address. 
* AB: That would be a good conception
* AB: You might have to clamp atomics, so that you get an address
* AB: The spec only describes what happens with OOB loads and stores. Which load/store behavior occurs for atomics? Do they occur independently?
* AB: Vulkan, without robust buffer acces 2 says atomics are undefined.
* AB: We’d want to make sure that an atomic will hit some address in range.
* MM: What does this disallow?
* AB: I wouldn’t want you to be able to get a 0 out of it, but write a real value into it. If the old value wasn’t 0, just returning 0 makes a lot of sense, if you’re going to write what’s at the atomic
* AB: Right now, the spec says atomics do a load and a store. If it’s OOB, I could load to some random address that’s in bounds, and could store to some other random address that’s in bounds. Something is missing.
* AB: Depending on how you would describe +=, it may work like a load/store
* JG: In terms of conceptually, a += would have the same, it’s a load and a store, and the spec allows them to come from different places
* JG: This should be true on all operations that involve a load and a store. Not just atomics
* AB: That’s fine
* MM: += is kind of different
* JG: We’ve postponed +=
* MM: What if the atomic straddles the end of the buffer?
* JG: Loads/stores that straddle the end of a buffer are considered fully OOB.


### [Data races and security #1788](https://github.com/gpuweb/gpuweb/issues/1788)



* AB: We discussed this internally. We took it to our security teams. They said “you have way bigger problems than this. So don’t sweat this.” We’re not planning to do anything extra special here any more
* AB: Using data races is not the most reliable way to attack the GPU.
* JB: We want to know what kind of attacks we want to protect against
* BC: They were trying to say that exploitation of GPU code is significantly rarer than CPU-side attacks. The first set of things that people look for are vulns in CPU code. The number of successful breaking-out of the sandbox by using the GPUs is rare. We should be focusing on the API and compiler stack
* AB: Their feedback is “this is not a practical way to [missed]”
* JG: Please say some stuff in the issue about all this, and close it.


## 

---



# 📆 Next Meeting Agenda



* Next meeting: 2021-08-10 (like normal)
* 