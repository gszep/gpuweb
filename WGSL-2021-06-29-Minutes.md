# **WGSL 2021-06-29 Minutes**

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send Dan Sinclair a Google Apps enabled address and he'll add you.



---



# **📋 Attendance**

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



*   Apple
    *   **Myles C. Maxfield**
    *   **Robin Morisset**
*   Google
    *   **Alan Baker**
    *   **Antonio Maiorano**
    *   **Ben Clayton**
    *   Brandon Jones
    *   Corentin Wallez
    *   **David Neto**
    *   Ekaterina Ignasheva
    *   Kai Ninomiya
    *   James Darpinian
    *   **James Price**
    *   Rahul Garg
    *   **Ryan Harrison**
    *   Sarah Mashayekhi
*   Intel
    *   Narifumi Iwamoto
    *   Yunchao He
*   Microsoft
    *   Damyan Pepper
    *   **Greg Roth**
    *   Michael Dougherty
    *   **Rafael Cintron**
    *   Tex Riddell
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
    *   **Jim Blandy**
*   Kings Distributed Systems
    *   Daniel Desjardins
    *   Hamada Gasmallah
    *   Wes Garland
*   **Dominic Cerisano**
*   Eduardo H.P. Souza
*   Joshua Groves
*   Kris & Paul Leathers
*   Lukasz Pasek
*   Matijs Toonen
*   **Mehmet Oguz Derin**
*   Pelle Johnsen
*   Timo de Kort
*   Tyler Larson



---



# **⚖️ Agenda/Minutes**


## Meta


### Office Hour



*   **Wednesday 10am-10:50am**
*   **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
*   Everyone welcome
*   Mass calendar invite will have been sent out
    *   If you still need an invite, add your email here:
        *   [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)
        *   


## 

---



## Timebox


### [Should the API enforce that writable storage buffer bindings don't alias each other? #1842](https://github.com/gpuweb/gpuweb/issues/1842) 



*   MM: I think we said that we would try the extra validation, and see how bad the perf overhead is.
*   JG: So do we need to spec something?
*   MM: Probably in the API spec
*   DN: WGSL requires no aliasing (variables reference distinct storage)
*   DM: But only within the shader, I think, not necessarily resources.
*   MM: It would be fine if they alias so long as it’s not observable
*   (consensus: back over to API)


### [Requiring integral-valued float literals to have .0 seems gratuitous · Issue #739](https://github.com/gpuweb/gpuweb/issues/739)



*    DN: We now have type inference for let-decl and var-decl, so it interacts.
*   (consensus: post-mvp)


### [wgsl: array size may be module-scope constant (possibly overridable) #1792](https://github.com/gpuweb/gpuweb/pull/1792)



*   DN: We don’t plan to implement this in the next month or so, but we do feel like this is the right direction.
*   MM: I don’t want to stand in this way, but I want to think about whether this causes issues for generating metal shaders. I can reopen the issue if it causes a problem.
*   DM: I think it would be impossible.
*   JG: We can punt for now, no pressure, sounds like
*   MM: Punting ok with me
*   DN: Away next week
*   MM: One other option is that we might choose not to generate shaders early if the author is using these facilities.


## 

---



## Discuss


### [Buffer indices should be unsigned · Issue #1135](https://github.com/gpuweb/gpuweb/issues/1135)



*    MM: I’m still working more on this. Two attacks: 1) Try in a number of shading languages; 2) Experimenting with zero-extending vs perf. I found I didn’t fully understand DN’s proposal. Could you elaborate?
*   DN: I think the problem is for large arrays, and I think I want a large array to require a wide int to index it. I want the max size of the array to be no larger than the max signed integer size.
*   MM: Consider, what if I index with -10. Is that “too big”? How should it work?
*   DN: It would wrap around.
*   MM: If the base of the array lies above 4 billion, then that should still work?
*   DN: That’s an implementation detail. The addressing that happens should be 32-bit friendly/well-defined. The fact that it has byte addresses that are very large is an implementation detail.
*   MM: I’m looking at the cost of a zero extension, and an array implementation involves a multiply and an add. On 32-bit pointer device, this is moot, so ignore this. On a 64-bit pointer device (that has enough memory), the multiply and the add would then be on the wider 64-bit types. So the question is what we do to promote the 32-bit signed indexing to our 64-bit internals? Assuming data lies at addresses, and arrays have a base pointer…
*   DN: That’s an implementation question.
*   JG: Is this sign-extend vs zero-extend?
*   MM: On many processors, the 32-bit operations will automatically zero the top half of the 64bit result register. So if that’s the goal, it’s generally free. For sign-extension, sometimes we’d need an extra instruction to get that behavior.
*   DN: I think I’m saying, if you’re thinking byte addressing has to happen a certain way, I’m saying it’s an implementation choice.
*   DN: For example, OpenCL didn’t start out with byte addressable memory, it had to be an extension. It’s often in hardware now, but not everywhere.
*   MM: If I write a program that use either signed or unsigned for the implementation, it will be the same instructions. Instead, in order to get a perf comparison, I have done all my own pointer math. If I present that, would it be valuable to others?
*   DN: I suspect that the memory access overhead/bandwidth will dominate the pointer math, even with caching. I would start with testing on a CPU first.
*   MM: I have some results, but not ready to share.
*   DN: I also think that given our portability requirements, then we shouldn’t admit this comparison.
*   [missing]
*   DN: I think if authors want >2GB they should author using 64bit types
*   MM: I think the math might still be expensive, which would mean that it would be useful to use a 32bit index
*   MM: We could potentially give the authors both options: u32 could be cheaply zero extended if users wanted it, or they could use i32, or i64 if they want
*   DN: Going back to automatic promotion to the widest signed type.
*   MM: If I have an array, I want to access the 3 billionth element, I would then need a wider type. Devices that support a 3 billion array probably do support i64. The compiler could see an unsigned array index and output i64 math. For the devices that don’t support i64, they would have lower max-size limits, and that would be ok. 
*   DN: Could have a rule, if 32bit index, we’ll use a 64bit index if the device supports it, and you’ve opted into it.
*   MM: I think what you’re saying is: In WGSL, if the author has not enabled 64bit ext, max buffer sizes will be artificially low, but only for spir-v devices. If they opt-in instead, the max size of buffers would be higher
*   DN: Not quite: No special case for spir-v. If you want have a 2B+ element array, you’d need to opt in to the i64 extension.
*   MM: I think that’s over-restrictive. Could make it up to implementations, where some implementations could handle u32 indices properly, even without i64.
*   DN: Right now spec says index can be i32 or u32, so we have that already.
*   JG: I think the proposal is that implementations that don’t want to support 3B arrays without i64, could choose to limit the max size to 2B and not have to deal with it. What’s the portability issue exactly?
*   DN: It would be clear that e.g. a device is a spir-v device because they are limited to 2B if it doesn’t have i64.
*   JG: So this is a problem with non-portable limits? (yes) I think that’s not a philosophical problem/blocker for us.
*   DM: Are we debating whether to put the max array size in the spec?
*   DN: Right now we limit them in WGSL based on the max int size.
*   DM: Is MM unhappy with this? (yes)
*   MM: I just want my u32(3B) to work
*   DN: I want you to have to use i64(3B)
*   MM: I think that’s unreasonable. Is it valuable if other shading language would support this?
*   DN: No, because we still need spir-v to work.
*   JG: I think we’re at the point where we just need to choose qualitatively, and that there’s not more data to be gathered. I think we should take another week to think to ourselves about it before forcing a decision (at worst, voting) next week.
*   GR: As a note, DX doesn’t support 64bit indexing, though there are 64bit capability bits, but not for indexing.
*   GR: We may want to reconsider taking a consequential vote so close after the US holiday


### [2nd, 3rd, and 4th components of sampled depth/stencil textures are undefined in Metal · Issue #1198](https://github.com/gpuweb/gpuweb/issues/1198)



*    


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-07-06 (like normal)
*   