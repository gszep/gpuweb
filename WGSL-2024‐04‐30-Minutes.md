# WGSL 2024-04-30 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial++-label%3Acopyediting+no%3Amilestone+), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+0%22+), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+1%22+)

**Todos doc:** [WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2024-04-16 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1OIfYvXO67-d4I5ISIjWajtVn8Y-cm5bqLeL_LIgvaiQ) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send **dneto** a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Mike Wyrzykowski **
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * **Alan Baker**
    * **Antonio Maiorano**
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * **Kai Ninomiya**
    * **James Price**
    * Rahul Garg
    * **Ryan Harrison**
    * **Stephen White**
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
    * Rafael Cintron
    * Tex Riddell
* Mozilla
    * Erich Gubler
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * Teodor Tanasoaia
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* Unity
    * Brendan Duncan
* Dominic Cerisano
* Dzmitry Malyshau
* Eduardo H.P. Souza
* Jeremy Sachs
* Joshua Groves
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson
* Jason Erb
* **Myles C. Maxfield**


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


### CG -> WG Transition



* See Corentin’s email about this group becoming a working group
* Contributors need to join the WG to continue contributions


---


# ⏳ Timeboxes (until XX:25)


### [Numeric conversion rules wording is inconsistent · Issue #4538](https://github.com/gpuweb/gpuweb/issues/4538)



* KG: f32’s specification is different from the other ones. Is this on purpose?
* DN: This is kind of the same as the next issue. The difference is floating-point to integer-conversions versus integer-to-integer conversions. We introduced a distinction between these two because we wanted to introduce clamping on the floating-point side.
* KG: let’s discuss the next two points.
* In light of #4569 being closed:
* DN: Let’s think about this one a bit more.


### [u32(abstractint) errors on out of bounds value, u32(abstractfloat) and u32(f32) clamps out of bounds value · Issue #4569 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/4569)



* KG:  There’s a collision between the desire to lossily-cast something at run time and the desire to strictly cast things at compile time when we have abstract types.
* KG: Mozilla is probably most in favor of saying, this is weird, but we have conflicting desires that can’t be squared in a way that yields perfectly consistent behavior. So, we’d recommend no change. And this is in an edge case - we are probably not going to get memed for ridiculous behavior over this.
* KG: The u32 built-in operator is a five-piece case analysis, which shows how many roles it’s trying to play, as it guesses what you want. Indicates presence of a hazard.
* KG: Mozilla’s feeling: no change is best
* AB: We’d be okay with no change. Suggested introducing a diagnostic, might be useful. The fact that we have both value-preserving and bitcast-style conversion operations makes it okay for these to be different.
* KG: We do have bitcast. The code is asking for a bitcast when there is a more explicit way to get that.
* KG: If folks want more time to consider this, that’s fine, but we did arrive at this behavior for a reason. Close for now?
* **RESOLUTION: CLOSE**


### [[Feature Request] Allow readonly usage of atomics · Issue #4574](https://github.com/gpuweb/gpuweb/issues/4574)



* KG: A struct has an atomic in it. We can put that in read-write storage, but you’re not allowed to put it in read-only storage, because our rules don’t permit it.
* AB: This came out of a discussion where the author wanted to re-use structures, but wanted to remove the atomicness in one case, wanted to reuse the atomics in read-only storage.
* AB: This would take some work in our backends, so we would like this not to be a priority.
* JB: That’s Mozilla’s opinion too.  Do it later if we want it later.


### [Should @diagnostic be allowed in the middle of lines? · Issue #4595](https://github.com/gpuweb/gpuweb/issues/4595)



* [issue was closed]


### [smoothstep: is it an error when `low == high` and/or `low > high` (when constant/pipline-evaluated) · Issue #4561](https://github.com/gpuweb/gpuweb/issues/4561) 



* KG: In underlying APIs this is UB. We promise portable runtime behavior, the question here is how constant evaluation should proceed: polyfill, or report an error, or a warning
* AB: evaluation errors would be consistent with other builtins. This seems like a spec bugfix.
* DN: GLSL goes out of its way to say you shouldn’t do this
* **RESOLVED: MODULE-CREATION-TIME ERROR**


---


# ⚖️ Discussions


### [Compat: WGSL validation at createShaderModule or createPipleine time? · Issue #4568](https://github.com/gpuweb/gpuweb/issues/4568)



* (DS: Generally errors are generated as soon as possible. But currently Dawn is validating compat rules only at pipeline creation time. But these oppose each other.  If all the compat info is available at shader-creation time, then should those be shader-creation errors.)
* (MW: Lean checking more things at shader-creation time, but will take a look.)
* JB: Mozilla tended to want to make it a module-creation -time error. It’s more consistent, and easier to plumb the diagnostics through (if you want good diagnostics). It’s an advantage to have alternate code paths for the two cases and select compat vs. non-compat at pipeline-creation time. But that’s a lean.
* KN: I’m in favour of them being consistent, but I’m concerned we’ll want to solve the problem in general, so it will be easier to pick up optional features. 
* KG: If we want to delay the error to pipeline time, we can add that to the shading language spec. There’s some backward compat risk if we later move these validations to pipeline creation time.
* KN: If we want to move errors to pipeline creation time, then all syntax for all the features need to be parseable by all browsers. So you can read entry points but ignore as needed.
* JB: We’re in an unusual situation; we’re starting it with a complete thing but export it to implement a subset. So having to parse everything is not a problem; the implementation already does.
* KN: Right, it’s an issue for future optional features. But it would be nice to be consistent.
* DS: If we add int64, then you have to parse some new suffix for int64 literals. How do you handle that.
* KN: Generalize the grammar, e.g. allowing any letter suffix to numbers.
* JB: The more we talk about applying this to other features the more uncomfortable I become with delaying errors to pipeline creation. The more we talk about this as a general solution I want to make it error at shader-creation time.
* MW: I have similar concerns.
* KN: We can revisit this later. As far as implementation, try to make it easier to change in the future.
* KG: Sound like consensus that the cautious path is to make it an error at shader creation time. We can revisit as needed.


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Pacific-timed**) Tuesday May 14 2023, **4-5p** (America/Los_Angeles)
* 