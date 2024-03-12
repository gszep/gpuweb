# WGSL 2024-02-20 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+is%3Aopen+milestone%3A%22Milestone+0%22), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+is%3Aopen+milestone%3A%22Milestone+1%22)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2024-01-23 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1aSdSJoG0cZWSiOLa3YQrIZU5kM8eGA3tU2pUn0ErzBs) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Mike Wyrzykowski **
    * Myles C. Maxfield
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
    * Kai Ninomiya
    * **James Price**
    * Rahul Garg
    * Ryan Harrison
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



* 
* 


---


# ⏳ Timeboxes (until XX:15)


## Untriaged:


### [Texture types with f16, e.g. `texture_2d&lt;f16>` · Issue #4483](https://github.com/gpuweb/gpuweb/issues/4483) 



* KG: Moz tentatively agrees that this would be nice 
* AB: Original reason: Can’t have a 16bit type in Images in d3d12/vk. Back when we were discussing it, we thought performance for dealing with packed values was [...].
* AB: 
* JB: So texture loads in VK will always be 32bit?
* AB: Yes
* AB: Heard from some vendors that if you had RelaxedPrecision on the load, and followed that with a downcast, they could optimize.
    * e.g: [https://github.com/KhronosGroup/Vulkan-Docs/issues/1140#issuecomment-1017222900](https://github.com/KhronosGroup/Vulkan-Docs/issues/1140#issuecomment-1017222900)
* KG: 
* AB: Haven’t looked into what formats are allowed. Probably wouldn’t get these in rw-storage? Would maybe be sampling-only? If we want to go down this road, we need to research
* JB: KN asked in bug whether this is a bug or deliberate, and it sounds like it was deliberate/intentional
* AB: The register pressure thing has been showing up as important for perf, so some of us care a ton.
* JB: So the polyfill would add register pressure?
* AB: Not compared to what we do now. But on Metal, it could reduce reg pressure.
* MW: Apple is in favor of allowing this.
* JP: I think D3D allows this, or at least DXC accepts it.
* AB: Do either of you know if we need new feature/sample-type flags?
* MW: I think it operates the same way as float[32] on Metal.
* JP: I recently prototyped this in our Metal backend, and I’m indeed getting some perf improvements.
* KG: Milestone? 3? 2?
* AB: Slight pref for 2
* MW: 2 sounds good.
* **-> M2**
* 


### [[wgsl] Assume bool is 4 bytes for array limits #4487](https://github.com/gpuweb/gpuweb/pull/4487)



* AB: I think originally it was supposed to be 4, but I guess we put it in as 1. I see this as a spec bug fix.
* **Approved.**

 


## M1:


### [relax absolute error bound for asin f32: too tight for Intel f32 with DXC · Issue #4477](https://github.com/gpuweb/gpuweb/issues/4477)



* JB: 6.77e-5 to 6.81e-5? “Rubber-stamped with extreme prejudice”
* 


### [Add PI and TAU builtins · Issue #4100](https://github.com/gpuweb/gpuweb/issues/4100)



* Oguz: PR is ready for review, TYVM! [https://github.com/gpuweb/gpuweb/pull/4473](https://github.com/gpuweb/gpuweb/pull/4473)
* BC: 3 things:
    * Not so sure on the benefit of `TAU`, since it’s only one more char compared to `PI*2`.
    * Not sure about whether or not to additionally have unicode symbols for these. Would like to defer until DN is back.
    * Not sure if we can implement this in any near-term timeframe. Would like to push back additions to the spec until we can catch up.
* MW: +1 to everything BC said
* JB: It felt like a tiny thing to Moz. Is is notably more difficult for other implementations?
* BC: A little bit. It’s *probably* easy, but there is opportunity for engineering risk here.
* AB: I kinda don’t want to spec things that no one is implementing (yet)
* KG: But if e.g. if Moz implemented this, then more amenable to add?
* AB: Yes
* KG: Since we’re postponing this, let’s move this to M2 or M3
* -> M2
* KG: I do suspect we’ll be landing this PR as-is, we just want to catch up in implementation a bit.


---


# ⚖️ Discussions


### [Shader extensions are redundant · Issue #3961](https://github.com/gpuweb/gpuweb/issues/3961) (M0)



* **CLOSED**


### Maximal reconvergence



* (Requested by AB)
* [Explainer blog](https://www.khronos.org/blog/khronos-releases-maximal-reconvergence-and-quad-control-extensions-for-vulkan-and-spir-v)
* [Extension text](https://htmlpreview.github.io/?https://github.com/KhronosGroup/SPIRV-Registry/blob/main/extensions/KHR/SPV_KHR_maximal_reconvergence.html)
* Testing and development challenges
* AB: When we talk about subgroups etc, at Khronos we’ve been working for a really long time on this. Specs are very underspecd for reconvergeance, and we’ve been working on this for 5 years. It takes a long time because it’s very difficult. For “where can we go with subgroup portability”, I think it’s really hard. We had a lot of data that we couldn’t release for a while (standard khronos stuff), but now we can. We did add public cts tests in the ‘experimental tests’ category.
    * We think it’s ready. [but with some minor gaps?] With something like this, we can start to have uniform behavior for these things, tightens up rules, e.g. “implementation must reconverge here”.
    * Some things are complicated, like helper-invocations.
    * The other is `switch` statements. Hard to deal with lowerings, so all that happens is a bounded range of convergence. If the guarantees we give you for switches aren’t enough, you can go back to `if`s, which are robust here.
    * We had a ton of help from IHVs, and we’re starting to see implementations integrating this.
    * This is the level of formality that we think is necessary for this stuff to be portable.
    * I wrote a blog post on this, and there’s also the (harder to read) spec itself. If you’re curious, read the blog first, since that does a better job of high-level explanations.
* KG: Do we have big questionmark still from d3d and Metal?
* AB: Metal doesn’t have much strictness here. I think we asked a while ago. Some of our bugs are fixed. Unsure how much Apple wants Metal to specify here.
* AB: For HLSL, Microsoft says “it should just work”, and the easy stuff probably does, but the hard stuff (helpers, switches) probably don’t. Lots of this was probably hammered out through devrel.
* AB: You can run the reconvergeance tests if you want, and most of them do just work, though not all. 
* AB: Because my main dev machine is Apple, I spent a lot of time hammering on the behavior on Metal. In [one example], I found behavior which seemed wrong, but couldn’t figure out how it would be wrong per-spec.
* AB: Most of our focus was on the devices we happened to have for Mac and Linux.
* KG: So prereq for subgroups? Add reconv tests to webgpu cts?
* AB: For subgroups no, but for portable subgroups, yes. There’s lots of benefit to subgroups even if they are not robustly-portable. 
* AB: Would love if webgpu could be influential here, to Apple and Microsoft. :) 
* RC: I think you’ve been already talking to the right people over here!
* AB: I know you’re working on vkon12, so maybe it’s possible to test it there.
* RC: Yeah talking to Greg (Roth?) is the right choice, and it’s good that this is mostly a testing thing. 
* KG: MW: Anything we can do to push this over to the Metal team?
* MW: If I had the testcases, I could totally look at them, and pass them along to the Metal team. Can you give them to me?
* AB: Yeah, I can include a link to the tests. [https://github.com/gpuweb/cts/pull/2916](https://github.com/gpuweb/cts/pull/2916)
* KG: So AB, do you see this as two features: subgroups and portable-subgroups? Or just land subgroups first in a less-portable form, and iterate on making them stricter over time.
* AB: Depends on timeframes I guess. If we can make everything portable before releasing subgroups initially, it would be ideal if they could be gotten right the first time. We want to push out experimentation/origin-trial for subgroups later this year. (even if they aren’t as probable as we’d like)
* KG: JB- What about in Naga?
* JB: We had an outside contributor contribute a big addition that we’re iterating with.
* AB: I’ll reiterate, when we were doing uniformity analysis, it was tricky. If everyone was stricter, we could also tighten up uniformity analysis, either more accurate or more precise, vs what we felt like we could spec/implement previously.


---


# 📆 Next Meeting Agenda Requests



* Next meeting?: (**Pacific-timed**) Tuesday March 05, 2023, **4-5pm** (America/Los_Angeles)
* 