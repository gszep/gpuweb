# **WGSL 2021-06-15 Minutes**

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
    *   **Sarah Mashayekhi**
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
    *   **Daniel Desjardins**
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
*   **Timo de Kort**
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


### Note



*   I’ve renamed “Needs Approval” to “Needs Decision”
*   Put things here if we should have enough info to decide during a meeting.
*   I think this slight broadening makes it more useful, and it should help relieve the overworked “Needs Discussion” column.


## 

---



## Timebox


### [wgsl: remove unsigned case for frexp 2nd arg #1823](https://github.com/gpuweb/gpuweb/pull/1823)



*   Issue: [wgsl: frexp: what happens if the exponent is negative but the second operand is ptr-to-u32 #1820](https://github.com/gpuweb/gpuweb/issues/1820)
*   (Propose dropping the unsigned case for second arg to frexp and ldexp)
*   MM: Why outvars?
*   DN: We don’t have tuples and don’t have named structs yet
*   JG: I think proposing struct names was on me
*   (merge this though)


### [wgsl: Remove level from 1D texture load intrinsic #1814](https://github.com/gpuweb/gpuweb/pull/1814) 



*   (Not all targets support mipmaps for 1D textures)
*   JP: Also adds overloads for other texture types, that removes the level parameter, out of consistency. And when level is not specified, semantic is to use level = 0.
*   MM: Intentional to do for load and not store?
*   JP: Stores don’t have level yet
*   GR: Levels would have to be zero anyway
*   JP:In spec now, textureLoad on storage texture does not have level param anyway.  So this change only adds overloads to non-storage textures.
*   JG: Does this reverse a decision we made before, to require level=0 to be specified?
*   AB:We made the opposite decision for external textures.  It has textureSampleLevel but it doesn’t have a level param.
*   JP: This PR makes textureLoad more like textureDimensions.
*   JG: Generally don’t like making more overloads just for the purpose of dropping a param.  So I’m less in favour of the second iteration of this to generalize it to the other non-storage textures.
*   DM: Do you want to require users to pass 0 for those cases?
*   GR: I’m remembering that was the decision as well.  Don’t feel too strongly.
*   JG: Perhaps we should find that previous decision.  Maybe different folks in the forum right now.


### [wgsl: detailed semantics of integer division and remainder #1830](https://github.com/gpuweb/gpuweb/pull/1830)



*   (PR for signed integer division)
*   (For INT_MIN / -1 makes it a defined result (yielding INT_MIN), assuming the polyfill was used.  Essentially forces the use of the polyfill)
*   MM: If backend APIs treat this as UB, then implementations will need extra care here. I guess if 2 of 3 of the backends need to detect this UB, then WGSL should define it both for consistency and because the implementation will have to guard against this anyway.
*   DM: ??
*   MM: This is related to a later discussion (#1135) because if the compiler identifies UB, it can do bad things.
*   MM: I see this as two steps:
    *   1) Our implementations must ensure that we don’t translate WGSL into an underlying shading language in a way that invokes UB troubles
    *   2) If we’re paving over the UB concerns in implementations, we should just standardize on a single result
*    ...
*   DN: In the two step arg, step 2 requires betting that our choice of definition will always be what we want. That is, maybe a different result becomes more efficient in the future.
*   DN will file a separate issue about the details of this general discussion


### [Expand allowable type for ignore() #1836](https://github.com/gpuweb/gpuweb/pull/1836)



*    Allows for ignore(sampler), ignore(texture), ignore(&buffer).
*   JG: Why do you want to do that?
*   BC: Useful for testing, or partial development of a shader where you haven’t written the guts of the shader, but still want to declare that certain bindings have to hang around.   Everything that’s being touched is being reported by the reflection code.
*   JG: It’s a bit weird you call ‘ignore’ to *not* ignore something.  (void) in C is kind of nicer.
*   BC: Would be useful someday if we raise warnings for unused variables.  Alternate is something like ignore(textureDimensions(s)).
*   JG: Maybe revisit wording (?)  The name of the intrinsic.
*   _Agree to merge._


### [wgsl: array size may be module-scope constant (possibly overridable) #1792](https://github.com/gpuweb/gpuweb/pull/1792)



*   (Dzmitry pointed out interaction with layout, so that needs clarification)
*   


###  [Assignment operators (e.g. +=) in wgsl #1518](https://github.com/gpuweb/gpuweb/issues/1518)



*    MM: Defer to post-MVP
*   JG: Agree
*   DN: Agree


### [WGSL: Standardized error codes #1813](https://github.com/gpuweb/gpuweb/issues/1813)



*    


## 

---



## Discuss


### [Buffer indices should be unsigned · Issue #1135 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1135) 



*   We will think more about this


### [2nd, 3rd, and 4th components of sampled depth/stencil textures are undefined in Metal · Issue #1198](https://github.com/gpuweb/gpuweb/issues/1198) 



*   (DN and MM to take a closer look at our APIs)
*   DN: I didn’t do my homework. I suspect there’s no way to sample both depth and texture simultaneously (aside from a comparison sampler).
*   MM: Metal latest spec says depth aspect can only be read via a depth texture.  Stencil is different.  If you have stencil 8 (no depth aspect), can be read via integer (noncomparison) sampling.  … Problem: you have stencil-only texture that you want to read in shader; treat it like a texture2d attach, then sample.  Works, except the return type is a 4-elem vector, but the stencil texture only has 1 component.  When you run the sample function on AMD and NV, the one channel is broadcast out to all 4component. But Apple and Intel silicon get the 1 channel, then 0,0,1 for the other components.
*   JG: Catalina gives us the ability to swizzle in the pipeline state?
*   MM: Yes.
*   JG: I think stencil textures are worth having.
*   MM: Can modify 5 to say you can have stencil textures, but not for sampling in the shader.
*   JG: Think of difference between render target (can’t read back) vs. stencil texture (which can be read from).  Maybe have a different texture types from it in the language, then you can make the overload return just one component, or force the other components to have specific values.
*   MM: That would be novel.  For compilers that produce WGSL, you need to synthesize this new type.   3,4,5 options are all ok with me.  Lean 5.
*   


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-06-22 (like normal)
*   