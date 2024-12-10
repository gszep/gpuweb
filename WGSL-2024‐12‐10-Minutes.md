# WGSL 2024-12-10 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN, KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial++-label%3Acopyediting+no%3Amilestone+), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+0%22+), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+1%22+)

**Todos doc:** [WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previously:** [2024-12-03 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1WsjRo52QNYvzEU5fYOjIQ8X16PCxym8LtQnFT3EtZ6o) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send **dneto** a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Mike Wyrzykowski**
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
    * James Price
    * Kai Ninomiya
    * Natalie Chouinard
    * Rahul Garg
    * **Ryan Harrison**
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



* 


---


# ⏳ Timeboxes (until XX:10)


### [Clarify that `alias` names can be used as constructor function names · Issue #5014](https://github.com/gpuweb/gpuweb/issues/5014)



* KG: Two issues. 1. Clarify you can do that. 2. Feature request for more general aliases to include function declarations. Want to split 2 into its own. What about the 2.
* JB: I don’t mind aliases for functions; seems you can write your own wrapper.  I don’t like aliases for writable things like variables. Very uncomfortable.
* DS: WESL folks want something for function aliases. E.g. let you define variations in a library, then use it with a particular name in your main program.  
* JB: Sounds like a using declaration. 
* DS: Yes.   
* JB: Suggest we wait for implementation feedback
* JB: I speculate that using aliases for that forces you to repeat the name.  The nice thing about import declarations is you don’t repeat yourself.  Think the feedback will be it doesn’t work as well as you’d hope.
* **KG: Will split it.**
* **DN: Milestone 2 or 3+ would be fine with me.**


---


# ⚖️ Discussions


### [WIP: Add subgroups feature #4963](https://github.com/gpuweb/gpuweb/pull/4963)



* (touch-base, status updates)
* JB: Some review.  Some question of properties into their own interface.  Min size and max size are not limits; don’t work as limits.  Comment on another issue “separate out properties as their own thing when we need them”.
* JB: Is there a concept of a subgroup index, as a subset of a workgroup.
* AB: There is but not in every API.
* AB: There is no reliable mapping between linear invocation Index in a compute shader, and subgroup index.
* JB: So no reliable chunking of local IDs into subgroups.
* JB: When someone uses this API they have to write as if an invocation’s subgroup members might be anywhere in the workgroup.
* AB: There’s nothing you can rely on in all cases. If you have a linear workgroup you do tend to see linear subgroup in an obvious way. Difficulty comes when you have multidimensional workgroups.
* AB: D3D doesn’t have a subgroup ID.  You have to write code to avoid assuming the association.
* JB: I can’t figure out how to use this. I can use ballot or sum. But what elements am I even operating on.  There are no guarantees.
* AB: A lot of operations are reductions, and you don’t need to know the relative order of things because you reduce all the way down. The CTS tests go out of their way to avoid the assumptions. It often generates its own subgroup ID specific to the execution.  E.g. use a broadcast of the invocation index of the first element in the subgroup.  So everyone in the subgroup shares some arbitrary value. 
* JB: And you use the index that you place the subgroup results.  And you accept the possibility it’s not 100% dense.
* AB: Most of the time you want to resolve results from the whole workgroup, and so it ends up dense. You can reconstruct where each invocation was placed.
* JB: CTS is one thing, but doesn’t have to produce a result an ordinary program might want to do.
* AB: Think of [atomic?] decimated compaction.  Use two passes.  First pass decides who needs to write, then you use atomics and reductions to calculate the index into which subgroup leads write.
* JB: I wonder if there are techniques like you describe that are ubiquitous that are so valuable such that we supply them as recommended primitives to give people a foothold.
* AB: Maybe. It came up in internal discussion to add language features that add polyfills over the enable-extension baseline feature.
* JB: I put other comments on the PR,  don’t need to talk about it here.
* MW: do we need some kind of barrier functions for subgroups?  OR do implementations insert the barriers.
* AB: There are no new barriers because there no “subgroup memory” memory locations to protect. So you reuse the existing barriers to order memory accesses.   In Vulkan the subgroup-execution-scope barriers are non-uniform, which is tricky ot think about.  So you end up trying to ensure subgroup reconvergence, then use a workgroup barrier.
* MW: Sounds good.
* **Reviews and iteration will continue.**


### [Proposal: "auto" layout algorithm should be able to generate any layout · Issue #4956](https://github.com/gpuweb/gpuweb/issues/4956)



* **KG: **Do we think we should wait for a proposal, like the similar other one?
* DN: I think there’s going to be a trend of having less strict types in the shader, and this kind of goes the other way instead. E.g. types of textures like rgba8.
* Ds: But this just gives auto the ability to work correctly for cases where our current more naive model wouldn’t be able to.
* DN: This looks like it should be type information, but we’re adding it as an attribute instead.
* DN: I’d rather have the convenience pay for itself on its own.
* Ds: An annotation feels cheaper to add, but we could actually make types for this as well if we wanted to.
* AB: So is auto like, the main feature that we build things around, or should we accept that it’s incomplete? It sounds like we’re saying that auto is so key that we want to ensure that we can always use it.
* Ds: Unclear, kind of a question for the API-side people?
* JB: [...] The only reason you would need to not use auto, is that you can share them. What I’m hoping to get from this is to never had to write types in JS again, given that I’ve already written the types in WGSL.
* KG: I think what you said stands on this issue. We’ve heard from people the idea of “why do i have to write these types; the types are already in WGSL”. There is a desire to allow people to use that mode of operation without ever having to do explicit bind group typing. They are really nice-to-have things that don’t seem terribly difficult to add support; it’s a matter of figuring out how we’re going to do it, hence type vs. annotation.
* KG: Think **next step is to have a proposal to discuss**.  Don’t think I heard we shouldn’t support the use case.


---


# 📆 Next Meeting Agenda Requests



* Next meeting?: (**Atlantic-timed**) Tuesday January 07, 2025, **11a-noon** (America/Los_Angeles)
    * (Can we talk about:)
    * 
* But email Kelsey or the list if you want to meet sooner!
* 