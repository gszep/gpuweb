# **WGSL 2021-03-30 Minutes**

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
    *   Rafael Cintron
    *   Tex Riddell
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
    *   Jim Blandy
*   Kings Distributed Systems
    *   Daniel Desjardins
    *   Hamada Gasmallah
    *   Wes Garland
*   Dominic Cerisano
*   Joshua Groves
*   Kris & Paul Leathers
*   Lukasz Pasek
*   Matijs Toonen
*   **Mehmet Oguz Derin**
*   Pelle Johnsen
*   Timo de Kort
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
        *   [rmorisset@apple.com](mailto:rmorisset@apple.com)
    *   Invited!


## 

---



## Timebox


### [Support for vector splats (scalar operations on vectors) #1520](https://github.com/gpuweb/gpuweb/issues/1520)



*   E.g. `vec(x,y) * 2.0`
*   Previously:
    *   JG: Ok, I will write up proposal based on intersections
*   PR: [https://github.com/gpuweb/gpuweb/pull/1575](https://github.com/gpuweb/gpuweb/pull/1575) 
*   DN: should be a table instead of a list, will do that editorial change and submit a new PR. Otherwise PR is fine.
*   Followup
*   GR: Matrix divide by scalar [or was that the other way around]  Is it in the intersection, and is it an oversight.
*   JG: MSL spec doesn’t say it allows it.  MSL spec might be incomplete.
*   _Discussion_
*   DM: Should reconsider matrix + scalar. Think it’s not useful, and is ambiguous.  Might have wanted a diagonal matrix.
*   JG: Generally, it’s always component-wise. Exception is matrix * vector (and vector * matrix).
*   DM: When discussing matrix constructor of scalar, we decided that was ambiguous/confusing/not-clear. Should not reintroduce it implicitly here.
*   JG: Think we want multiply matrix by scalar. By analogy would want to allow adding.
*   MM: MSL doesn’t allow matrix + scalar or scalar + matrix. 
*   JG: Nevermind then! I’ll update the proposed list of supported operations.


### [Replace const with let. #1574](https://github.com/gpuweb/gpuweb/pull/1574) 



*   AB: Keep calling them “constants”.  A ‘let’ is how you declare a constant.
*   JG: DM wondered about module-scope ‘const’.
*   JG: Also, specialization constant.  Still ok to call them ‘constants’
*   MM: +1 to using the same token ‘let’.
*   _Consensus._


### [Remove the void type #1460](https://github.com/gpuweb/gpuweb/pull/1460)



*   Merged


### [Add /* */ block comments. #1470](https://github.com/gpuweb/gpuweb/pull/1470)



*   Previously:
    *   MM: Should we just try writing down the state machine for line-and-block comment nesting? I can do this
*   MM: Research: [https://github.com/gpuweb/gpuweb/pull/1470#issuecomment-809788878](https://github.com/gpuweb/gpuweb/pull/1470#issuecomment-809788878)
*   MM: State machine proposal: [https://github.com/gpuweb/gpuweb/pull/1470#issuecomment-809818216](https://github.com/gpuweb/gpuweb/pull/1470#issuecomment-809818216)
*   _Discussion of the graph, relation to C++._
*   _Proceed to PR._


## 

---



## Discuss


### [WGSL: describe ref ptr #1569](https://github.com/gpuweb/gpuweb/pull/1569)



*   Describes the consensus from [https://github.com/gpuweb/gpuweb/issues/1456](https://github.com/gpuweb/gpuweb/issues/1456) 
*   _Consensus that the content is ok.  For editors to merge._
*   AB: point about access qualifier on type, for the assignment.
*   


### [Add the uniformity analysis to the WGSL spec #1571](https://github.com/gpuweb/gpuweb/pull/1571)



*   AB: Misunderstanding of the proposal. Expected “there are some things that originate non-uniform values or behaviors” but don’t see that addressed.  E.g. some builtins are definitely uniform and others definitely non-uniform. Eg. group ID value, invocation ID value.  Is this more conservative than I thought?
*   RM: Have TODOs to fill in for different builtin functions.  The analysis is about propagating the facts through a view of the program.   But can change the inputs / leaf facts as needed.
*   AB: Looking at behaviours of a function body.  If neither Nothing or Return, then reject the program.  But each loop that terminates must have a Break; so anything with a loop is rejected.
*   RM: When you exit a loop, you remove the “Break” from the set of behaviours, and replace it with “Nothing”.  Will address by separating the prose.
*   AB: I think I get the intent.  The discard one confused me.  The only behaviour is Discard, or Return. Is a singleton of behaviours allowable, as long as each invocation has the same behaviour.
*   RM: Yes, a singleton would be accepted.  If there is a singleton behaviour for a function, then the behaviour after the call site is the same as the behaviour before the call site: either all the invocations are still there, or none of them are.
*   RM: Idea is if you have a single behaviour for a compound statement (or function body), then the convergence property after the end of that statement is the same as at the beginning.  Will try to add examples, and check against the test suite.
*   MM: Examples would have helped reading through.
*   MM: Bikeshed has a mechanism for marking a section as an example; examples are non-normative.
*   DM: does the PR allow private variables to be uniform?
*   RM: Currently all globals are considered non-uniform. DN pointed out some kinds of module-scope variables are uniform.
*   _Discussion: sampled textures are uniform.  Storage textures are pointers_
*   _Need some things to be clarified editorially; RM to continue edits. Direction is good._
*   AB: Need to define the invocation group. 
*   DM: For vertex shader it doesn’t matter.
*   AB: Yep.  For a fragment shader, it gets complicated. 
*   JG: Is invocation group an entire draw call? That’s what it brings to mind, but maybe it’s the raster group.
*   AB: Will look it up.
*   MM: Does it affect the analysis for today’s WGSL?  Sure, it may when adding features.
*   DM: It affects flat-interpolated floats.
*   GR: Derivatives don’t care about the full group; it only cares about the quad.
*   AB: SPIR-V / Vulkan doesn’t guarantee reconvergence of the quad after a prior divergence.  So it’s more than just the quad.
*   JG: If interested in the content, tag yourself as a reviewer.  Wait on merging until all the reviewers have had a chance to review.  Expect to be permissive: don’t expect it to be perfect.
*   RM: Feel should wait at least one week.  Hope to do the first round of edits by office hour tomorrow; may confirm builtins lists in office hours tomorrow.
*   AB: Checked Vulkan spec:  invocation group for a fragment shader is the whole draw.
    *   DM: Please post the reference.


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-04-06 (like normal)
*   Atomics in a pointer world
*   