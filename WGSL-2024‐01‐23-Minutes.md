# WGSL 2024-01-23 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **4-5pm **Americas/Los_Angeles (**Pacific-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+is%3Aopen+milestone%3A%22Milestone+0%22), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+is%3Aopen+milestone%3A%22Milestone+1%22)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2024-01-09 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1qm1ZsrmJVbwzCos_ZbsateXKW1wSLE2Vi8-1taUWae4) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


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
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * dan sinclair
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Price
    * Rahul Garg
    * Ryan Harrison
* Intel
    * **Hao Li**
    * Jia A Chen
    * Jiajia Qin
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
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



* KG: Looking at requirements for advancing specs to next level.


---


# ⏳ Timeboxes (until XX:15)


## M1:


### [Feature request: textureQueryLod equivalent · Issue #4180](https://github.com/gpuweb/gpuweb/issues/4180)



* Oguz: I made a simple inquiry into the built-in behavior across APIs and drivers. In combination with ecosystem info, I suggest two functions. My comment in the issue: [https://github.com/gpuweb/gpuweb/issues/4180#issuecomment-1907044827](https://github.com/gpuweb/gpuweb/issues/4180#issuecomment-1907044827)
* KG: Why two functions?
* MOD: HLSL and MSL have that.
* DN: I don’t understand the two, the wording is different.  Vulkan’s reads likes truncated and untruncated.
* MOD: Wrote the app to check.  It’s really two different values.
* DN: I’ll need time to absorb the example and what it means.
* DN: from SPIR-V spec
    * The first component of the result contains the mipmap array layer.
    * The second component of the result contains the implicit level of detail relative to the base level.
* DN: Seems like it can be affected by sampler LoD offset?
* KG: willing to be a third set of eyes.
* DN: I need more time.
* MOD: Easy to hack the project, linked in the issue.


### [Add PI and TAU builtins · Issue #4100](https://github.com/gpuweb/gpuweb/issues/4100)



* Oguz: Please allow me a week as I’m almost done with the investigation but had to delay a bit due to recovery, kind regards.
* KG: What’s the investigation needed?
* MOD: In the past, I had some trouble stemming from the need to specialize to PI_2, etc. in context of equations for integration ops, etc. Thought of validating in those cases and reporting data AoT
* DN: Can write text saying mathematically perfect value; infinitely precise. (maybe rounded up or down maybe)
* KG: We have tacit agreement to add these values. We don’t have agreement to add pi /2.  We’d have to reopen it.  Could do that in another issue.
* KG: It’s about spelling, then write the text.
* MOD: Ok, thank you


---


# ⚖️ Discussions


### [HW Clip Planes support · Issue #390](https://github.com/gpuweb/gpuweb/issues/390) (M2)



* JS: Investigating the limit of number of clip planes allowed. It seems Metal has an undocumented limit of 8.
* KG: How to bridge the gap between the desire and what can be done.
* JS: We’re still investigating support level.
* DN: Primer on this feature?
* KG: See the D3D11 functional spec;  describes it well.
* DN: ok.
* KG: There’s a similar investigation for an extension for WebGL.


### [Shader extensions are redundant · Issue #3961](https://github.com/gpuweb/gpuweb/issues/3961) (M0)



* DN: Google’s stance is the redundancy is fine, and do not want a change in the spec.


---


### [Avoid indeterminate value result for tanh with large input · Issue #4458](https://github.com/gpuweb/gpuweb/issues/4458) (M2)



* ZJ: [https://github.com/gpuweb/gpuweb/issues/4458](https://github.com/gpuweb/gpuweb/issues/4458)   tanh is defined over the whole Real number line, but defined as inherited from Sinh and cosh, which overflow very easily.
* AB: Are you proposing hardware returns this, or we would have to polyfill to get this result.
* ZJ: Currently D3D12 is returning NaN for large inputs. Spec currently allows this.  I propose to make the spec strict to prohibit such indeterminate value.
* JS: In ANGLE, extreme values return 1. To work around D3D11 limitations. But it’s not in the WebGL spec. Want to spec it for WebGPU.
* DN: One way out is to define a second function, `tanhComplete` that is defined over the whole real line. It’s polyfilled and slow, for the people who need this behavior.
* KG: Seems in the weeds. Resist adding overload for this purpose. What’s the use case?  My inclination would make it slow but accurate in the common case. Then wait for reports of slowness.
* AB: I don’t think you’d need a branch, and given the inherent complexity of the core function, it’s probably fine to throw in selects. Probably used for activations for ML layers.
* JS: (thumbs up about the ML use case)
* AB: Question is where the boundary is, and how to test it. 
* ZJ: Currently the proposed case is based on getting within 0.5ulp for the 1.0 result.  I need to do an on-device investigation.
* AB: As long as we get confident on the transition, it’s probably fine to put the restriction in.
* KG: I wonder if we should let the implementation choose the threshold. The main issue is to ensure completeness. If that’s the goal, we have to assert that’s the case.
* DN: Concern is we would need _some_ rule for the gray area.  In the test suite we would spray values in the region and check.
* KG: Milestone?  
* DN: M2
* DN: I’m not super-concerned about urgency because a developer can always put the selects in themselves, at the cost of a little extra complexity.


---


# 📆 Next Meeting Agenda Requests



* Next meeting?: (**Atlantic-timed**) Tuesday February 13, 2023, **11am-noon** (America/Los_Angeles)
* DN: [#4477](https://github.com/gpuweb/gpuweb/issues/4477) relax absolute error bound for asin f32: too tight for Intel f32 with DXC
* AB: Next time may want to talk about ecosystem stuff. (maximal reconvergence)