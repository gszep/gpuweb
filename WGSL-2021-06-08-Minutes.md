# **WGSL 2021-06-08 Minutes**

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
    *   **Ben Clayton**
    *   Brandon Jones
    *   Corentin Wallez
    *   **David Neto**
    *   Ekaterina Ignasheva
    *   **Kai Ninomiya**
    *   James Darpinian
    *   **James Price**
    *   **Rahul Garg**
    *   **Ryan Harrison**
    *   Sarah Mashayekhi
*   Intel
    *   Narifumi Iwamoto
    *   Yunchao He
*   Microsoft
    *   Damyan Pepper
    *   **Greg Roth**
    *   Michael Dougherty
    *   Rafael Cintron
    *   **Tex Riddell**
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


### [Remove workgroup_size builtin #1771](https://github.com/gpuweb/gpuweb/pull/1771)



*   MM: Fine to remove. If we need it, we can add it back.
*   (approved)


### [signed integer division: unspecified rounding mode for the quotient; and overflow · Issue #1774](https://github.com/gpuweb/gpuweb/issues/1774)



*   (Polyfill of signed integer division turned out not too bad. [https://github.com/gpuweb/gpuweb/issues/1774#issuecomment-853488578](https://github.com/gpuweb/gpuweb/issues/1774#issuecomment-853488578))
*   DN: Polyfill more straightforward than I expected. It’s more instructions now, but people probably don’t care?
*   MM: Probably best to be portable first. Maybe less portable a la fastmath later
*   KN: Devs probably won’t love that they get 3 instructions for one “instruction”. So maybe we can allow operator% only for unsigned, since that’s “perfect”, but mod(int,int) that’s portable.
*   DM: Do people care wrt how expensive division is?
*   MM: Ints uncommon in shaders, too. And division on ints are doubly uncommon. Perhaps even so uncommon that it’s fine for it to be 3 instructions.
*   JG: Ints will be more common in compute shaders, though.
*   Consensus:
    *   Require portability even if it emits extra instructions
    *   Allow operator syntax for % and / with ints


### [Remove % for signed integral #1763](https://github.com/gpuweb/gpuweb/pull/1763)



*   (resolved above - same resolution as on #1774)


### [2nd, 3rd, and 4th components of sampled depth/stencil textures are undefined in Metal · Issue #1198](https://github.com/gpuweb/gpuweb/issues/1198) 



*   JG:  Other APIs fill in 0,0,1.  What’s the direction for Metal, if any.
*   MM: Metal has different types for depth text. Vs. non-depth.  And the sample function returns a scalar, not a vector.  The fact that an author can attach a depth texture with a regular sampled texture, then it’s not-intended usage.  Swizzling might not be a thing.
*   MM: How important is it to create a depth texture, but bind it to a thing in the shader that is not a depth texture type in the shader.  Perhaps disallow that case?
*   DM: Concern is reusability of shaders.  You’re saying restricting that and only create a depth binding for those.
*   MM: Yes.
*   DM: Another thing is how to treat the stencil type.  The plan of record was to use a sampled texture in the shader to bind a stencil part of a depth/stencil resource.  So need to solve that. So why is depth special when stencil has to be solved anyway.
*   DM: Shader distinguishes between depth vs. non-depth (sampled) texture resource type. But you still need to read a stencil, which falls into the non-depth sampled resource type.
*   JG: Sounds like Myles is suggesting a “stencil” sampled type in GPUTextureSampleType just like there’s a special “depth” enum value.
*   MM: May need to learn more.  Each bit in a stencil is significant. So why do “sampling” on it.
*   DM: It’s not about filtering though. WebGPU already has a confusion of “sampled” for how the read path works, vs actually filtering (etc.).
*   DM: Plan was, to read stencil, you bind the texture to a regular sampled texture type with uint sampled type, and you get a vec4&lt;u32> in the shader.
*   MM: Maybe treat stencils with the same wgsl type as depth.  (?)  So you’d have builtins to get the stencil value. But treat as stencil view into the depth/stencil texture from the API side. (pls check this.)


### [Uniformity annotations for global variables #1791](https://github.com/gpuweb/gpuweb/issues/1791)



*   DN: Can we have the compiler just figure out uniform/non-uniform for the vars, rather than needing the author to get it right?
*   RM: The alg can only validate yes/no, so it would need to try yes/no for each var, so it’d be exponential in runtime.
*   MM: Why not model the vars as inputs (and outputs?) to functions?
*   RM: Same sort of runtime problem, where complexity is higher that way.
*   RM: Not sure what syntax to propose here.
*   DN: Suggest making a new PR for this thing alone. Also, “uniform” at what scope? Especially once we have subgroups.
*   MM: Sounds like we want this between double-square-brackets.


### [Error with literal indexing in a shader #1803](https://github.com/gpuweb/gpuweb/issues/1803)



*   MM: Motivation: It would be bad if one browser had different choices of what to reject.
*   DN: Yes, we just need to pick one and standardize. My pref is to tread array case as dynamic, even for literals. Would be fine having static error for vectors.
*   MM: Aren’t arrays not harder than vectors, expect dynamic arrays.
*   DM: Maybe we should punt on that?
*   MM: Also, from a compiler laziness standpoint, nice to treat everything as dynamic.
*   DN: Nice to have single universal rules.
*   JG: Isn’t vector subscripting have the same limits as matrix subscripting?
*   JB: There are more useful operators for vectors here than for matrices.
*   (consensus: Treat it as dynamic)
*   


## 

---



## Discuss


### [Buffer indices should be unsigned · Issue #1135 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1135) 



*   (Previously: MM to talk with Metal team about proposed solution (i64 all the time))
*   MM: Discussions with the Metal team are actually quite large, and still ongoing. Recommend postponing.


### [Clarify out of bounds behavior #1722](https://github.com/gpuweb/gpuweb/pull/1722) 



*   (people needed time to review)
*   
*   people need time to review


### Vulkan Uniformity Extension



*   AB: There’s a new VK ext: VK_KHR_shader_subgroup_uniform_control_flow
    *   adding uniformity rules for subgroup scope
    *   [https://github.com/KhronosGroup/Vulkan-Guide/pull/118](https://github.com/KhronosGroup/Vulkan-Guide/pull/118)
    *   It’s a new optional execution mode for the shader.  It doesn’t work out of the box when you try it.  The driver has to modify its compilation stack to make the behaviour work.
    *   When you add the mode to the shader, you get more guarantees about the behaviour.
    *   You get workgroup uniformity (reconvergence) by default, but this extension enables it for subgroup level.
*   DM: Do you expect existing hardware to get updated drivers?
*   AB: This can be handled in existing devices, I expect.  For a KHR extension, Vulkan requires 3 passing implementations, and this extension has met that condition.


### [2nd, 3rd, and 4th components of sampled depth/stencil textures are undefined in Metal · Issue #1198](https://github.com/gpuweb/gpuweb/issues/1198) 



*   DN: IIRC, you might want to do a depth and stencil ops at the same time. That is, it would be nice if we can preserve the fastpath in the underlying API, and not require extra ops. 
*   MM: Not sure if this falls on the “not everyone can do it, so drop it” or “not everyone can do it, so require polyfill”.
*   KN: I know that MTL can only bind as depth or stencil, can’t bind both into the same view.
*   (DN and MM will take a closer look at our APIs)


## [https://github.com/gpuweb/gpuweb/issues/1816](https://github.com/gpuweb/gpuweb/issues/1816) null chars in WGSL source.



*   KN: If null-char can exist inside the source, we need to hold (data,size), and not just (null-term-data,).
*   MM: And this comes up because of a C-api?
*   KN: Yes, for Dawn.
*   MM: Not that sympathetic to constrain ourselves because of this.
*   BC: Does unicode allow this?
*   MM: Null bytes are valid but not null code points.
*   KN: Pretty sure zero is a code point?
*   MM: No.
*   KN: So utf-8 has a non-zero alternative to allow zeros without null-term problems.
*   JG: USVStrings allow zero scalars, so we do accept that today at least.
*   MM: Well, I think our grammar forbids zero chars.
*   KN: Maybe not in comments though.


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-06-15 (like normal)
*   