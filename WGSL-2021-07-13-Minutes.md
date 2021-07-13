# **WGSL 2021-07-13 Minutes**

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



* Apple
    * **Myles C. Maxfield**
    * **Robin Morisset**
* Google
    * **Alan Baker**
    * **Antonio Maiorano**
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * **James Price**
    * Rahul Garg
    * Ryan Harrison
    * Sarah Mashayekhi
* Intel
    * Narifumi Iwamoto
    * Yunchao He
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * Rafael Cintron
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
* Mehmet Oguz Derin
* Pelle Johnsen
* Timo de Kort
* Tyler Larson



---



# **⚖️ Agenda/Minutes**


## Meta


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



## Timebox


### [need saturating conversions between signed/unsigned int scalars and vectors #720](https://github.com/gpuweb/gpuweb/issues/720)



* _closed_


### [wgsl: array size may be module-scope constant (possibly overridable) #1792](https://github.com/gpuweb/gpuweb/pull/1792)



* DN: Probably remaining blocker is the goal that it’d be nice to be able to generate MSL shaders at ShaderModule creation time.  Otherwise, the shader author would have to generate the WGSL source at pipeline creation time, which is a materially worse option for them.
* (deferring discussion a week)


### [[wgsl] Improve the Memory Layout sections #1915](https://github.com/gpuweb/gpuweb/pull/1915) 



* Is this editorial? (mostly yes?)
* BC: One new surprising behavior is If you have a structure within a structure, you can’t put anything in the 16byte padding.
* DM: What if uniform buffers were all aligned to 16bytes?
* BC: There’s a difference between size and alignment here.
* MM: I had a related proposal, to add this restriction to the layout rules instead of a validation error.
* BC: Probably don’t want to require 16 bytes for as many cases as that would add.
* _Consensus to merge_


### [WGSL: Support type conversion from bool to int/uint/float #1910](https://github.com/gpuweb/gpuweb/issues/1910)



* DN: What about vector versions? 
* DM: Needs investigation?
* DN: Let’s take scalars for now and look at vectors separately.
* (-> Needs spec, DN to take)


### [WGSL: Allow global let-declarations initialized with constant expressions #1911](https://github.com/gpuweb/gpuweb/issues/1911)



*  MM: We sort of need to spec constant expressions first
* JG: Don’t we have some hacked version of this?
* MM: Something like “just literals”. I think we should postpone after mvp
* JB: What about just allowing constant idents too?
* DM: Constants or spec-constants
* JB: Probably just constants for now?
* DN: Happy to push off to mvp. Worth noting, SPIR-V has a description of const-expr that we can work from. 
* (push to mvp)


### [Consider whether we need a quantize-f32-value-to-f16 #704](https://github.com/gpuweb/gpuweb/issues/704)



* DN: We do have the pack/unpack ops already, so you can more or less do this already.
* DM: How is this done on Metal?
* DN: Just convert twice.
* GR: Support for f16 in hlsl exists.
* DM: So people would use this without 16bit floats, such as knowing what they’ll get when outputting to f16 render targets?
* DN: Yes, authors have use for it, though it’s niche, though it does exist as part of authors’ flows, for validation purposes/testing if “can I convert all this code to f16 safely”. It’s part of existing development processes.
* DM: Would SPIR-V put relaxed precision on the vars then?
* DN: It’s usually a compiler flag.
* 

 


### [[wgsl] Comma operator #581](https://github.com/gpuweb/gpuweb/issues/581)



*  (close? Move to post-mvp?)
* DN: Close!
* RM/MM: Don’t close!
* RM: Let’s postpone past mvp.
* MM: Strongest arg from our perspective is using the comma op to avoid having to use the continuing block. There are other args, but that’s the most important one.
* (postponed)
* DN: It’d rather have something like a lambda block.
* MM: Lambdas are good too.


### [Clarify out of bounds texture operations #1906](https://github.com/gpuweb/gpuweb/pull/1906)



* JB: So this does allow multiple/alternate values/results, which is a portability problem, right?
* MM: There’s both a portability problem and a perf problem too. I did testing on this that I published in the past. [...] Shader authors try pretty hard to avoid accessing OOB, so I’m less worried about the portability here, if there’s performance to be gained.
* JB: If author try to avoid it, it’s probably *not* perf-critical too, so we probably don’t have to worry about perf.
* MM: Portability here *is* fairly constrained. There are four(?) options, so it’s not too wild what you can do here.
* DM: I think it is perf-critical here, because we need to check even the good accesses. Practically we’re going to have D3D on D3D, robustness on VK, or shader injection on VK, and shader injection on MTL.
* BC: Clamping is what Tint does.
* DM: Does tint have an option to do zeroing? (no)
* MM: Would be great if we accidentally end up on the same behavior and we can narrow this more.
* (accepted)


## 

---



## Discuss


### [Buffer indices should be unsigned · Issue #1135](https://github.com/gpuweb/gpuweb/issues/1135)



* DM: What if we have a u32(3B)?
* DN: OOB access behavior
* DM: What about i64(3B)?
* DN: That would be fine
* [what about large runtime sized arrays as a different type]
* DN: I don’t have a proposal for that.
* MM: I think this would be a design mistake
* MM: I think we should accept i32/u32/i64, and if it’s a valid offset, it’ll work regardless of type.
* (let’s let gears turn and come back to this)


### [2nd, 3rd, and 4th components of sampled depth/stencil textures are undefined in Metal · Issue #1198](https://github.com/gpuweb/gpuweb/issues/1198)



* MM: (listed 3 main directions)
    * 1. Allow deviant behaviour for those components.
        * 1b.  Allow two distinct behaviours (the ones that are observable today)
    * 2. Make a stencil texture in shader language.
    * 3. Don’t allow binding stencil textures like this (as a shader resource)
* JG: Could constrain to a handful of behaviours. (like out of bounds)
* AB: How do you know which texture type to do it on.
* MM: Jeff is saying do it without any extra shader code. And running the code on all GPUs out there will result in one of two behaviours.
* JB: Concern that “undefined behaviour” in practice is going to be stable / safe in the future.
* DM: It’s compiler undef behaviour. The compiler just doesn’t know what kind of thing is bound.
* JG: Push the guarantee upstream?
* DM: It’s definable on non-Metal APIs.  And for Metal it’s only a concern for old software versions (??)
* MM: Texture swizzling is not compatible with render targets.  And stencil are usual with render target.   And don’t alias stenciling with other kinds of bindings.
* MM: Depth textures are not a problem here because there’s a different WGSL type for them. It’s only for stencil textures.  And depth texture is a different binding type in the API too.
* JG:  Think modern API should have sampling from stencil texture.
* DN: option 2 is my least favorite.
* DM: Banning (i.e. option 3), is not the end of the world. Can always copy the texture to an R8 texture and sample that.
* (out of time, regroup for decision next week)


## 

---



# 📆 Next Meeting Agenda



* Next meeting: 2021-07-20 (like normal)
* [https://github.com/gpuweb/gpuweb/pull/1913](https://github.com/gpuweb/gpuweb/pull/1913) [wgsl]: Add fmod(), frem() builtins
* [https://github.com/gpuweb/gpuweb/issues/1480](https://github.com/gpuweb/gpuweb/issues/1480) [wgsl] Proposal: Remove pointer out parameters from modf, frexp](https://github.com/gpuweb/gpuweb/issues/1480#)