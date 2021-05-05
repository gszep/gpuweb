# **WGSL 2021-05-04 Minutes**

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
    *   Dean Jackson
    *   Fil Pizlo
    *   **Myles C. Maxfield**
    *   **Robin Morisset**
*   Google
    *   Corentin Wallez
    *   **David Neto**
    *   Kai Ninomiya
    *   James Darpinian
    *   Brandon Jones
    *   Ryan Harrison
    *   Sarah Mashayekhi
    *   **Ben Clayton**
    *   **Alan Baker**
    *   **James Price**
    *   Rahul Garg
    *   Ekaterina Ignasheva
*   Intel
    *   Yunchao He
    *   Narifumi Iwamoto
*   Microsoft
    *   Damyan Pepper
    *   **Greg Roth**
    *   Michael Dougherty
    *   **Rafael Cintron**
    *   **Tex Riddell**
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
    *   **Jim Blandy**
*   Kings Distributed Systems
    *   Daniel Desjardins
    *   **Hamada Gasmallah**
    *   Wes Garland
*   Dominic Cerisano
*   Joshua Groves
*   Kris & Paul Leathers
*   Lukasz Pasek
*   Matijs Toonen
*   **Mehmet Oguz Derin**
*   Pelle Johnsen
*   **Timo de Kort**
*   Tyler Larson
*   Eduardo H.P. Souza



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


### VF2F



*   Survey soon!
*   


## 

---



## Timebox


### [FMod vs FRem semantics #1696](https://github.com/gpuweb/gpuweb/issues/1696) 



*   (To specify is human. To test is divine)
*   We will do an investigation into builtins in MSL and HLSL, and come back to this
*   DN: Because they are sort of named backwards in SPIR-V, I wish I could go back in time to swap them
*   AB: We sometimes choose to polyfill, and these would be easy
*   JG: But is the polyfill that useful?
*   MM: If we only have one of these supported, seems like we should just pick that one
*   TR: HLSL has mod(s) but DXIL doesn’t have builtins for it
*   MM: Proposal: Both `%` and the builtin, which allows us to add both later maybe
*   DN: And that it would be C99 fmod, spir-v OpFRem
*   DM: Why operator? (taste)
*   DN: Weak against float%float because e.g. C doesn’t support that anyway, but not strong feeling.
*   MM: So fmod(floats), and uint/sint %-operator
*   MM to write PR


### [Add invariant attribute #1673](https://github.com/gpuweb/gpuweb/pull/1673)



*   Ship it


### [add atomic type, texture type, store type to storable; adjust rules for var and let. (issue #1686) #1693](https://github.com/gpuweb/gpuweb/pull/1693) 



*   (issue: [https://github.com/gpuweb/gpuweb/issues/1686](https://github.com/gpuweb/gpuweb/issues/1686))
*   DN: Pulling atomic types into plain types for variables, and then a bunch of fallout from that. Also textures and samplers can be vars e.g. at the module scope but not everywhere.
*   DM: Are textures really “storable”? (well it’s in-storage?)
*   Editors/DN to land this


## 

---



## Discuss


### [Relax the out-of-bounds specification #1649](https://github.com/gpuweb/gpuweb/issues/1649)



*   MM: Can we talk about OOB access derailing the whole draw call? (yes, but next week)
*   MM: If you have shader with array, and index off the end, something happens, spec offers some options. There’s also a different situation where you have a draw call that asks to read off the end of the vertex buffer (e.g. too-large index), and that has options too. This is particularly an issue for indirect draw calls. You can’t stop an OOB indirect draw call, but we can use a compute shader to write vert_count=0 before the draw call.
*   AB: Can we just say “as if the draw call was discarded”?
*   MM: Can say “if you OOB off the end of anything, impl can &lt;list of options>”. But there are cases where you can’t actually fully discard the draw call, because you may have already done work. 
*   DM: Maybe we don’t need to mention this in WGSL, but rather the API spec. We would only need the RBAB defined.
*   GR: What does the API say today?
*   DM: Clamped I think, but lots of TODOs
*   Sending this to API group
*   DM: Worried about OOB being able to return (safe) garbage for OOBs. [Someone] had an idea to enforce stronger D3D behavior with shader preambles.
*   AB: At a cost though.
*   DM: We would at least expect desktops to have this for free.
*   RM: Do we have numbers? 1% very different from 20%
*   DM: We need to test on mobile
*   RM: Can we agree to revisit this after investigation?
*   AB: For storage, does it let us store anywhere in an array? Discarding the store would be control flow. Additionally, would bias performance balancing towards mobile, vs usually overpowered desktops
*   MM: We think it’s worth seeing if the perf tradeoff is worth it. Portability concern makes it worth considering. DM: Can you look?
*   DM: We will look
*   JB: Does hardware have conditional stores? (yes probably)
*   DM: Can test with shaderplayground, which does tell us e.g. RDNA
*   TR: Don’t they need this? (yes)


### [Should we be able to pass textures and samplers as function parameters? #1695](https://github.com/gpuweb/gpuweb/issues/1695) 



*   AB: Do we forbid it, or require a conservative uniformity-analysis-like pass?
*   MM: Leaning towards doing the analysis.
*   MM: Tangent: What would this look like with Modules? Having two shaders that use the same helper function, and have that code only appear in the binary/memory once.
*   JB: Another benefit of analysis is that it’s nice from a user perspective. There may be good reasons to make modules even if it’s just for users
*   MM: We can talk about modules more in the future.
*   DN: Wondered, what the runtime complexity would be here. I think the analysis complexity is feasible given what the limits on resources here are.
*   MM: Like what limits?
*   DN: E.g. 64 texture bind points. If we deal with arrays of these later, could treat each array conservatively as having the same constraints/usages.
*   JG: If we allow conservative analysis, we allow some people (maybe) to footgun, but if we didn’t allow analysis they would have to rewrite their program to make it work. This rewrite would still work with conservative analysis.
*   MM: What would the pessimal program look like? I think it’s not that bad
*   Let’s try for it: Resolved needs spec


### [consider pulling 'access' into the type of a pointer/reference #1604](https://github.com/gpuweb/gpuweb/issues/1604)



*   (David’s proposal slide deck:  [WGSL access attribute](https://docs.google.com/presentation/d/1-XIH9gEjnqQJRSNnhicso7VBRd_ZuZYGwfmc4HKzM-s/edit?usp=sharing), also posted to the last comment on the issue)
*   [DN presents slides]
*   MM: 1. Should state why you want read-onlyness. E.g. fewer pipeline API barriers required, gives you better perf.
*   MM: 2. Difference between a thing not being mutable, vs. the shader can’t mutate it via that reference.  Want read-only-ness of the pointer to match the same as the read-only-ness underlying object.
*   MM: 3. Forbid casting-away const-ness.  Make the type system more sound.
*   MM: 4. Want read-onlyness to follow the pointer.  Becomes impossible to write a helper function that is generic over read-only-ness.


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-05-11 (like normal)
*   #1604 again
*   RM: Uniformity analysis update will be ready definitely in two weeks, but maybe not by next week.