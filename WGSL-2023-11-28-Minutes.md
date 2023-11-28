# WGSL 2023-11-28 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-10-10 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1D7_zrKp2rW-A_MBPkum3uzXLTVaIHC1cNchxe1Nk2JE) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * Mike Wyrzykowski 
    * Myles C. Maxfield
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Price
    * Rahul Garg
    * **Ryan Harrison**
* Intel
    * Hao Li
    * Jia A Chen
    * Jiajia Qin
    * Jiawei Shao
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * Zhaoming Jiang
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Microsoft
    * Damyan Pepper
    * Greg Roth
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * Erich Gubler
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * **Teodor Tanasoaia**
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* Unity
    * Brendan Duncan
* **Dominic Cerisano**
* Dzmitry Malyshau
* Eduardo H.P. Souza
* Jeremy Sachs
* Joshua Groves
* Lukasz Pasek
* Matijs Toonen
* Mehmet Oguz Derin
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson
* Jason Erb


---


# 📢 Announcements/Meta


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


### FYIs and Notable Offline Merges



* Multiple language features have landed since our last meeting
    * [Integer dot product](https://github.com/gpuweb/gpuweb/pull/2738)
    * [RW and RO storage textures](https://github.com/gpuweb/gpuweb/pull/4326)
    * [Unrestricted pointer parameters](https://github.com/gpuweb/gpuweb/pull/4323)
    * [Dereference syntax sugar](https://github.com/gpuweb/gpuweb/pull/4311)


---


# ⏳ Timeboxes (until XX:15)


## Meta topic: What does “M1” vs. “M2” mean.



* Ordering on _discussion._
* Ordering on _landing changes._
* Related to timelines?
* KG: they are our old v1 and v2.  Try to finish M1 before M2. 
* AB: Hope to use to prioritize discussion. But some features will take a long time, and need time to bake and discuss. Want to get the ball rolling on those.
* KG: It’s a way to communicate to the outside when a chunk of work has been done.
* KG: I will make agendas from M1 things. Can add other things, on demand.
* AB: This group didn’t do a WGSL-side triage into milestones. Are we open to pulling things in and pushing things out.
* KG: These are porous. Up to us to update the milestones.
* KG: The things in M2 are not that urgent. We’ve voted with our feet.
* AB: We’ve done an internal triage. Will share next time.
* KG: Happy to look at them.


### [wgsl: clarify what happens for IEEE 754 "invalid operation" cases, e.g. acos(2) · Issue #4219](https://github.com/gpuweb/gpuweb/issues/4219)



* DN: I’m treating this as **editorial already**. I have WIP.
    * Already added text listing the kinds of IEEE fp exceptions. (Text around “IEEE-754 defines five kinds of exceptions”).  Remaining work is applying those terms to cases in various builtins.


### [Add PI and TAU builtins · Issue #4100](https://github.com/gpuweb/gpuweb/issues/4100)



* DN: If we add these, also add the greek letter form.
* BC: We’ve been liberal adding builtins. Fear of collisions if we add too many things.  May have overlap with other things in the future. Pi is limited usage.  It’s nice that any user can add things that shadow builtins.
* KG: ‘pi’ comes up a lot for “packing information”. Silent shadowing makes it fine.
* AB: Make them builtin functions. Don’t want traits or accessors on types. 
* JB: We have global constants that are abstract types, so use them.
* BC: Pi is a universal number.  Other things like min max value of type are different.  
* KG: Like `Pi` and `Tau`
* JB: WGSL borrows a lot from Rust. So does 
* JB: Lay out your arguments, then leave it for the editor.  
* DN: Greek letters? Two votes yes.
* BC: Have to check unicode XID_start or not (update: they are)
* KG: Re capitalization, Math.Pi in JS.  Think capitalization is the right choice.
* **_Yes to the feature. Spelling to be determined._**


### [Missing intrinsic functions like rcp and rsqrt that are useful in other shading languages · Issue #4092](https://github.com/gpuweb/gpuweb/issues/4092)



* JB: Tangled up with our questions about fast math. Do we want blank thing, or specific operations. This is not just about these functions.  Needs a wider discussion. In [https://github.com/gpuweb/gpuweb/pull/2080](https://github.com/gpuweb/gpuweb/pull/2080) Dzmitry proposed an attribute on function calls.
* DN: Also related: [feature request: relaxed ftoi · Issue #4280](https://github.com/gpuweb/gpuweb/issues/4280) 
* AB: Last meeting notes on this issue said needed discussion of accuracy. But it was only one platform.  
* KG: **Moving to M2**


### [wgsl: Sketch design for arrays of buffer-bindings · Issue #2105](https://github.com/gpuweb/gpuweb/issues/2105)



* Do we want this to stay in M1?
* KG: Propose move to M2.
* AB: Regression vs. GL.  But not at the top of our list.
* KG: **Moved to M2**.
* TT: Think texture array is regression from GL, not buffers.
* AB: Want to design both at the same time.
* AB: Arrays of textures is [https://github.com/gpuweb/gpuweb/issues/822](https://github.com/gpuweb/gpuweb/issues/822) (and is currently M1)
* 


---


# ⚖️ Discussions


### [Subgroups Proposal - Pull #4368](https://github.com/gpuweb/gpuweb/pull/4368)



* AB: We wrote an experimental extension with very limited functionality to determine portability.   (Only need size, id, and ballot).  The short of it: There’s not much portability across platforms and devices.
* AB: There’s a draft PR in CTS to run the experiments.
* AB: We have internal use cases that get a lot perf out of this, and really want this. We want to do an origin trial in order to prove out the performance benefits.
* AB: We’re looking for the group to agree on this direction, then hammer out the details. That will let us experiment.
* JB: People get attachment to experimental code.
* DN: The request is to land the proposal doc in the repo, not to change the main spec.
* JB: Expect to need this in the future. Not prepared to offer opinions on naming, or tech details. Think the threshold for landing proposal docs is low.
* KG: Difference between landing as proposal vs. having this in your own repo.
* AB: Signals agreement on direction. Fine points will need discussion.
* JB: Inclined toward this, is taking a very prevalent arch feature and fronting it to applications, because it has massive performance implications.  Don’t know of competing designs we’d consider as alternatives. WebGPU will have to do something here eventually.
* KG: Positive on direction. Details TBD. Do worry about accidentally ensconcing details of the implementation because customers write code and end up relying on it.  Do want to have a stronger way to deprecate, e.g. by a prefix on each builtin name, or a long extension name.
* AB: More prefixing seems fine. We’ll discuss internally.
* JB: Positive signal. Let’s discuss next week.
* KG: If you want an official signal, we have a standards-position process: [https://github.com/mozilla/standards-positions/](https://github.com/mozilla/standards-positions/) 
* AB: Call to action. Please look at it.  Tried to lay out where portability falls down. Sketched some potential guardrails.
* JB: **Want to discuss some details in the office hours**.


### [add a system-defined wsgl namespace for predeclared objects · Issue #4308](https://github.com/gpuweb/gpuweb/issues/4308)



* AB: Spacing this, likely pulls in user-defined namespaces too. ([https://github.com/gpuweb/gpuweb/issues/777](https://github.com/gpuweb/gpuweb/issues/777) )   So it seems like a large feature.  Our impetus was code complexity that we’re now getting away from after internal rewrites.
* KG: So M2? (JB nodding)
* AB: That’s where it ended up for us internally.
* BC: Came from when we were doing all our transforms in the AST. And had lots of complexity to avoid collisions.  Still remains a user-level issue. If people do libraries by concatenating, then you can get a collision accidentally.  It’s hard to access the root “Pi” for example, when some other concatenated codebase that shadows “Pi”. But yes, our internal code complexity challenges are going away.
* KG: Propose moving to M2.  Library authors can adopt a style to not shadow WGSL builtin things.
* **_Move to M2_**


### [Remove the need to hard-code texel format for `texture_storage_2d` in shaders · Issue #4298](https://github.com/gpuweb/gpuweb/issues/4298)



* KG: For write-only storage textures common to not need the format. For read and read-write usages you do need the format, commonly.
* DN: Sad to tie it to access mode.
* TT: Most people will only use the write-only, so might be worthwhile still.
* KG: what about “any”.
* AB: Still need the shader component type in there, like for sampled types. Split it three ways: i32, u32, f32.
* KG: Push to M2?
* AB: Usefulness depends what you’re used to. HLSL never does it. GLSL does. But your engine might not be configured to plumb it through.  Feels like a quality-of-life thing only.
* JB: Kai points out opportunity to reduce # pipeline combinations.
* KG: Think of this as “proposals considered”. 
* AB: Feature could cover the readonly.
* DN: There are aesthetic concerns.
* DN: Fingerprinting concern?
* TT: Read without format is very uncommon. Think we could make write-without-format core. Only lose 3 PowerVR devices.
* KG: Think the fingerprinting aspect: the feature is likely to align with other bundles of features. Are those PowerVR devices popular?
* KG: **Put this aside for now**, **push to M2**.  Group will not actively try to solve it in the M1 timeframe.


### [Early Depth/Stencil test directive or flag in pipeline state creation · Issue #398](https://github.com/gpuweb/gpuweb/issues/398)



* KG: Think there is some value. Came up as GL extension (?).  Anybody want to pick this up?  Or push to M2.
* DN: Is this shader or API?
* AB: Vulkan SPIR-V makes it a shader execution mode.  
* KG: MSL makes it an entry point attribute.
* **Moved to M2 absent anyone picking it up.**


### [Uniformity annotations for global variables · Issue #1791](https://github.com/gpuweb/gpuweb/issues/1791)



* AB: Think we can close this. We added a diagnostic system.  That’s what this was originally proposed for.
* JB: What about compute shaders.
* AB: If it’s unsafe to turn off, then you can’t trust the user to tell you to turn it off anyway.
* JB: The user is making a promise. For private variables, you should trust the user. If it is sound to forbid non-uniform writes, then why can’t we infer that uniformity.
* JB: Don’t know if Robin’s original proposal is correct.
* KG: We can close this for now, then wait for folks to complain. Then we can reopen it.
* **_Close it._**


---


# 📆 Next Meeting Agenda Requests



* Next meeting? (**Pacific-timed**) Tuesday December 05, 2023, **4-5pm **Americas/Los_Angeles
*  

 
