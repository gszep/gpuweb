# WGSL 2024-08-27 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN, KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial++-label%3Acopyediting+no%3Amilestone+), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+0%22+), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+1%22+)

**Todos doc:** [WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2024-07-30 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1j_m02lBNTIpnz4dD3xci1lGjcCjtuPm8JN_Ok7ewOdg) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send **dneto** a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Mike Wyrzykowski **
    * **Myles C. Maxfield**
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
    * **James Price**
    * Kai Ninomiya
    * Natalie Chouinard
    * Rahul Garg
    * Ryan Harrison
    * Stephen White
    * **Peter McNeely**
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
    * Ashley Hale
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



* There will be a presentation from UCSC in the API meeting tomorrow on memory isolation
* [Add inclusive add and mul to subgroups](https://github.com/gpuweb/gpuweb/pull/4822)
* [Restrict subgroupBroadcast id to const-expression](https://github.com/gpuweb/gpuweb/pull/4820)
* Landed [PR4825](https://github.com/gpuweb/gpuweb/pull/4825) wsgl: clarify fp edge cases
    * Fixed: [#2765](https://github.com/gpuweb/gpuweb/issues/2765), [#2884](https://github.com/gpuweb/gpuweb/issues/2884), [#3220](https://github.com/gpuweb/gpuweb/issues/3220), [#4219](https://github.com/gpuweb/gpuweb/issues/4219)
* WIP: Updating the treesitter version.
    * Lots of things changed.
    * Oguz: May cause spurious/weird CI notifications, sorry!


---


# ⏳ Timeboxes (until XX:20)


### [Clamp overload for scalar min and max with vector value · Issue #4815](https://github.com/gpuweb/gpuweb/issues/4815) 



* JB: Mozilla’s thinking:these sound like good changes, with real improvements. Have a lot of places in the spec where it makes sense. Going through them piecemeal seems not optimal.  Should review systematically. At that point it looks like a bigger project. The language is usable as-is. We feel it makes sense to **treat this as M2**.  For now focus on shipping what we have right now. When we come to M2 and address all these issue they’ll be very happy with the improvement.
* AB: We basically agree. We did do one like this, for `mix`.  Do we want splatting in general, or in special cases. Look at it in a batch.
* JB: For this and the next two items, generally.


### [Support for mixed type `>>=` operation · Issue #4816](https://github.com/gpuweb/gpuweb/issues/4816)



* **As discussed more generally in #4815, M2 for now, but a nice little quality-of-life feature to add down the road.**


### [Support for `mat / scalar` · Issue #4817](https://github.com/gpuweb/gpuweb/issues/4817)



* DN: Additionally for this, 1. Our sense is linear algebra has add and mul, but less so division.  2. Which division will it be: teh full precision / or the more approximate reciprocal. If we choose one then the other community will soon call for the other, which ends up being ugly.
* JB: Kinda surprised you view division here as different enough conceptually. Might as well “scale down” as “scale up”.
* DN: Fine to drop my point 1. Not worth arguing.
* JP: Our sense is M3+;  same with the clamp and shift issues above.
* KG: **We can pick M3+ here and change this or other around later as needed.**


### [bgra8unorm storage textures without bgra8unorm-storage, should generate WGSL error? #4711](https://github.com/gpuweb/gpuweb/issues/4711)



* Corentin: Please confirm the resolution is that this is a pipeline-time error, similar to [Should invalid combinations of storage texture formats and access generate a WGSL error #4728](https://github.com/gpuweb/gpuweb/issues/4728)
* KG: Yes, agree.d
* DN: Yes, pipeline error.
* AB: Yes. We discussed it in this meeting, and agreed it was pipeline error.
* **Confirmed**


---


# ⚖️ Discussions


### [`textureSampleLevel` arguably doesn't work on Intel Mac in compute shaders · Issue #4818](https://github.com/gpuweb/gpuweb/issues/4818)



* Moz: what a well-filed issue - thanks to Greg for the comparisons, numbers, and plots
* JB: It’s so strange that it’s only in compute shaders and not graphics shaders; only certain platforms. It falls within our responsibility as implementors to provide portability.  Agree with suggestion to implement workaround on those platforms.
* KG: In the spec we commit to be better than our implementations are, and better than the CTS tests make us be.  CTS should match the spec.
* MM: Difficulty in characterizing.  If spec is going to describe some are ok and some are not, that’s going to be tricky.
* JB: Well the polyfill just samples on integer boundaries and lerps.
* MM: Your proposal is the AMD mac behaviour is also not acceptable?
* JB: Yes.
* MW: Gregg says WebKit is getting better results on AMD.  Let’s get to root cause here before we move.
* JB: I’m concerned that it does work on graphics pipeline, but not in compute pipeline. If everyone is getting good results on graphics pipeline we should give good service.
* JB: These are the operations that _don’t _do derivatives: you supply the level specifically.
* MM: Since the author has to invoke these special functions that give explicit levels/graidents, these are going to be common in compute shaders.
* MM: Seems the AMD case should good enough. Rather than paying twice the cost to do two fetches.
* KG: There’s a sliding scale of acceptability.  The AMD result is strictly better than the Intel result.
* JB: _put the decision in the hands of the implementor how far out of spec you’re comfortable with._
* MW: Apple’s implementation on AMD precisely matches M1 mac.  So would not require a polyfill.  Maybe some difference in implementations.  The AMD issue is solvable without a polyfill. The Intel UHD 630 issue; should see if that reproduces outside the 630.
* KG: If the spec says it’s linear, CTS should verify that (within some tolerance). 
* AB: Gregg was creating the test when this came up.
* MM: Also the function should be monotonic.
* **DN: 1. Get to root cause difference between Chrome and WebKit on AMD.  2. Find out if other intel drivers/devices are affected.**
* KG: Amusingly, in practice there appears to be a consensus constraint, but it is quite loose: e.g. 0->0, 0.5->0.5, and 1->1!


### [The value of the fragment builtin @position #4777](https://github.com/gpuweb/gpuweb/issues/4777)



* Corentin: Please confirm the milestone. If it is M0 then we would need some resolution.
* KG: We specify the standard positions.  The bug is to specify it more clearly. If someone wants to propose more clear language.
* DN: Gregg’s concern was that OpenGL might not follow what was specified.
* JB: It would only be a problem for compat.
* MM: Seems the cost of a polyfill would be cheap:  observe the deviation, then adjust shaders like that 
* KG: It actually occurred with a particular ANGLE configuration. We could either: don’t ship compat on that config; relax compat for that reason, or the implementation may decide to deviate from the spec.
* **_Resolution: no update needed to the spec. (but if you disagree, please just make a PR to add the language that satisfies you!)_**


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday September 17 2024, **11a-noon** (America/Los_Angeles)
    * KG: Maybe Sep24?
    * MW: That’s during TPAC.
    * KG: **Sep 17 then**
* But email Kelsey or the list if you want to meet sooner!
* 