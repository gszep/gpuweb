# **WGSL 2021-04-06 Minutes**

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
        *   


## 

---



## Timebox


### [Allow comma at the end of the parameter list #1584](https://github.com/gpuweb/gpuweb/pull/1584#issuecomment-812171222)



*    _Approved the proposal._


### [Add ignore builtin function #1587](https://github.com/gpuweb/gpuweb/pull/1587)



*   MM: `ignore` better than `sink`
*   No strong feelings, so `ignore` it is


### [Add section for "Constant Identifier Expression" #1586](https://github.com/gpuweb/gpuweb/pull/1586)



*   DN: Replied to feedback from DM. 
*   AB: What about undefined case? When you don’t specify something
*   DN: Pipeline creation fails
*   AB: What if it has an initializer, do we need to supply it?
*   DN: That would be ok.
*   MM: This is values in the global scope?
*   DN: First row is pipeline overridable constant, &lt;details>
*   DN: To JB’s point, could say that program is invalid in that case?
*   DN: Will add note for AB, will check with AB before merging


### [[wgsl] Define interpolation decorations/qualifiers · Issue #802 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/802#issuecomment-778411038)  



*   Has some discussion over naming.
*   And question for D3D about interpolation qualifiers on vertex outputs.
*   AB: Was question for microsoft: can interp decorations happen on just VS? GR? Do they have to match, can they be just on the VS?
*   AB: I think Metal is FS input only
*   MM: If Metal needs on VS output, we might need to require them there too.
*   GR: Will get an answer for d3d
*   AB: Will start writing it up
*   GR: vertex `nointerpolation` qualifiers must match between vertex outputs and fragment inputs in D3D


### [Atomics proposal · Issue #1360 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1360#issuecomment-811304155)



*   MM: Probably should be using pointers. 1. Consistency with existing languages (like MSL). 2. Consistency within WGSL. 3. Same logic as desire for explicit ampersand.
*   AB: If we do pointers, we don’t need to change.
*   MM: Should we talk about loads and stores?
*   AB: I wrote up how we could emulate them. I think we discussed whether to do this in the past and agreed.
*   


## 

---



## Discuss


### [WGSL: describe ref ptr #1569](https://github.com/gpuweb/gpuweb/pull/1569)



*   DM: Main thing we’re discussing here is whether to discriminate based on type qualifiers, such as with stride annotations. We could say that this was a ‘reference qualifier’, rather than having it be within the formal type system. I think this would give us a stronger type system.
*   MM: Type qualifier wouldn’t be spellable? (yes) Is the qualifier equivalent to lvalue/rvalue? (not familiar with C enough to say)
*   RM: What’s the difference between type and type qualifier?
*   DM: Can bolt qualifiers on to a stable type system without changing the formal type system.
*   MM: Does this affect what programs can be written? (no) What type of analysis are you expecting to do on the types but not the qualifiers?
*   DM: Can validate more simply that the types on each side match.
*   DN: Can I assign 3 to 5 then?
*   DM: Will we need type qualifiers at all? What about access flags?
*   DN: I don’t think we should use type qualifiers for that.
*   AB: Access is a property of the memory and not the type
*   JG: Can't the property of memory be constrained/satisfied by the type?
*   AB: Not per se(?)
*   AB: Probably don’t want to be nesting types with different access qualifiers, ergonomically
*   DM: Can’t we say then that we have to trace the accessibility to the root variable?
*   DN: Reference itself is the set of mem locs together with how it’s interpreted
*   MM: The only way I could see an alternative working out here is if the sum of the complexity of the qualifier system is less than or equal to the complexity of the type system alone
*   DM: I think it’s more complicated to intertwine this into the type system instead of living outside of it
*   DM: Does Tint have a ‘real’ type for ref?
*   DN: Hasn’t landed yet, but yes, it’s expected to.
*   


### [Add the uniformity analysis to the WGSL spec #1571](https://github.com/gpuweb/gpuweb/pull/1571)



*   RM: Thanks to everyone who commented! I don’t think it’s useful to discuss details in the whole group, but rather it would be good to discuss #669 instead.
    *   [Issue 669](https://github.com/gpuweb/gpuweb/issues/669)
*   MM: I feel like there’s not much to discuss, but rather this is an exercise in identifying the intersection between our host APIs
*   AB: Though we do need to do this statically, which not all hosts may have
*   MM: I volunteered to investigate MSL
*   GR: I can tell you for derivatives in d3d, where we assume the author doesn’t ever ask for them outside uniform control flow
*   DN: Maybe there is a distinction, whether we can statically determine these, vs dynamic. On a particular platform, it might *compile*, since it assumes that there’s some run-time reason why it will be valid, even though it’s not provable
*   TR: Can always expand it later, but can’t restrict it later
*   MM: As a programmer, I want/need to know if what I’m doing is legal
*   AB: I think we would need different uniformity levels, which current proposal doesn’t handle
*   MM: Besides derivatives, is everything else handled? Maybe I need to go through the builtins to check which needs what.
*   MM: Right now, wgsl is pretty simple, can’t address quad/simd groups today, but, we might want to add that later. It could be useful for RM and me to already go adapt the uniformity analysis to handle multiple levels of uniformity. Today, FS has derivative groups, and CS have workgroups, but no shader type has multiple levels today.
*   RM: We may just be able to run the analysis twice.
*   GR: Is this done for each webpage? (yes, as part of shader validation and type checking)
*   DM: One case we have in webrender, where we have different quads that have different values for `flat` interpolators, and this is a use case we should try to handle for webgpu.
*   TR: Like for per-primitive uniforms?
*   TR: Also difference in doing a deriv in non-uniform control if its source at least comes from uniform control flow
*   MM: This is a case where we may reject a technically-allowed thing, for unusual code
*   TR: Not that uncommon, and we probably want to handle it. This was allowed in FXC even when we tried to be very conservative otherwise.
*   


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-04-13 (like normal)
*   Uniformity
*   Timeboxed things?
*   