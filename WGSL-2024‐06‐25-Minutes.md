# WGSL 2024-06-25 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial++-label%3Acopyediting+no%3Amilestone+), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+0%22+), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+1%22+)

**Todos doc:** [WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2024-06-04 WGSL - Agenda / Minutes](https://docs.google.com/document/d/17j6DwuU6tkigIhZ7wbflYeZptc3sUAAJz1YB51t8oaE) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send **dneto** a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * Mike Wyrzykowski 
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
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * **James Price**
    * Kai Ninomiya
    * **Natalie Chouinard**
    * Rahul Garg
    * **Ryan Harrison**
    * Stephen White
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
* Myles C. Maxfield


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

Progress to CR?



* DN: Not really much effort yet, and we haven’t triaged.
* DN: Suggest having a “CR” label to indicate things that should block candidate recommendation.


### FYIs and Notable Offline Merges



* 
* 


---


# ⏳ Timeboxes (until XX:25)


### [Mistake in recursive decent parser definition? · Issue #4713](https://github.com/gpuweb/gpuweb/issues/4713)



* DN: Probably right.  This is likely a bug in the grammar analyzer code. **That’s mine**.
* 


### [WGSL API proposal: shaderLog · Issue #4704](https://github.com/gpuweb/gpuweb/issues/4704)



* (milestone?)
* AB: What kind of reach are you interested in for this?  For us it’s very useful, but if it’s not on Windows then it’s not all that impactful.  Would require a significant effort to implement what the SPIR-V layer does for Vulkan, but for D3D.
* KG: Expect that.
* JB: So D3D doesn’t have a vulkan-link printf extension?
* AB: The linked-to thing only does it in reference implementation but no real drivers.
* AB: SPIR-V layer instruments your code to add writes to an additional buffer. Then using a layer to extract that.
* KG: it’s a ease-of-use feature.
* DS: Any talk on the API side to add layers to WebGPU?
* JB: Would web content themselves inject the layers?
* KG: Would consider it just a feature. No plan to have a layer the way Vulkan implements them (where it’s easy to patch them together).
* AB: My gut says M3 because of the implementation effort.
* KG: Agree.  If someone is desperate for this they can do it themselves.
* KG: Biggest WGSL aspect is strings in WGSL which have none of.
* DS: don’t have to follow C fmt string.
* JB: Agree if we control then we can do nice things.
* **_Move to M3+_**


### [Nested struct and recursive structs · Issue #4700](https://github.com/gpuweb/gpuweb/issues/4700)



* (milestone?)
* DS: Nested structs is already in the spec.  Recursive structs…. No.
* DN: Recursive structs is syntactic sugar for storing pointers.
* KG: Have a need for a struct self-reference, like `struct Foo { ptr&lt;Foo>`
* DN: Order independence at module-scope means it’s already there in terms of identifier resolution.
* JB: Implies malloc?
* AB: No: can have a structure built on the CPU side, and represented in a buffer. (Vulkan buffer reference feature)
* JB: No plans for this.
* AB: This needs a full deliberate design, and it wouldn’t start from this language feature.
* **_Resolved to close as “no current plans”_**


### [Static Samplers Proposal · Issue #4687](https://github.com/gpuweb/gpuweb/issues/4687)



* (milestone?)
* KG: Summary of the post: perf is not significantly faster. Converted to an ergonomics suggestion.
* KG: For milestone, can wait to M3+. Native APIs do have something for this.  The Metal design sounds useful.
* AB: The Metal design feels nice.  Work in Vulkan to make it work seems unappealing.  They are experimenting in Dawn to gather information.
* **_Move to M3+_**


---


# ⚖️ Discussions


### [Do pipeline-creation errors encompass the whole module or just the GPUProgrammableStage? #4648](https://github.com/gpuweb/gpuweb/issues/4648)



* JB:  The approach Alan is advocating for in [https://github.com/gpuweb/gpuweb/issues/4648#issuecomment-2145774057](https://github.com/gpuweb/gpuweb/issues/4648#issuecomment-2145774057) is taking a view centered on the stage being compiled into the pipeline.  Think JS devs will like that which is more permissive.  Uncomfortable that even if B provides a value that can’t be used as presented, it seems we ought to tell them about that, even if it’s not statically used. It seems an indication of some kind of mistake.  Are providing good service if we ignore things that are mistakes.
* AB: We talked about that. We think the common use case is that the overrides are treated as a bundle. They haven’t worked through all permutations. It’s not clear the service/disservice is pointed in the right direction if we are more strict with the error in this case.  It’s easier to understand/explain if you strip down.
* JB: So the mental model is “i have a big bundle and use it for a lot of things”, and you say certain portions of that bundle haven’t been populated or they were set up for a different scenario, and we don’t want those parts to interfere with the current use.
* AB: We think authors are only going to be confident of the ones that _are _statically used.
* JB: I find that persuasive.  But this is more permissive which means we can’t change our minds if we get it wrong.  It does seem right to pare the module down to what you’re using in the pipeline. And seems not hard conceptually to use.
* AB: We already went more permissive where you can set a value that isn’t used.   That was an ergonomics choice. 
* JB: That enabled the bundles, and people wanted that.  And now this question is it will only count if statically used by the entry point.  I’m ok with that.
* JB: Are these clarifications?
* AB: Think the spec currently doesn’t describe the rules well.  Think needs clarification in a few spots. That pipeline creation error only induced by things related to the stage being compiled, and clarify when/how the substitutions occur. And add examples.
* JB: Position makes sense to me.  Not immediately obvious how this impacts our implementation.  If it’s not urgent, **I’d like to talk it over with Teo** to discuss how this impacts our override implementation.
* AB: Sounds good.  Post a comment when you’re done. Consider this as “tacitly approved”.
* JB: SGTM.


### [Guarantee that OOB writes to storage textures will be ignored #4194](https://github.com/gpuweb/gpuweb/pull/4194)



* AB: We’ve landed the change in Tint.   I’ve written CTS tests in draft form.  We had several folks internally review those tests.  And the tests pass on many devices. Think this is ready to land.  Implementations seem to disregard the stores that are out of bounds.  Want David and Oguz to review editorially.  Think all ducks are in a row now.
* KG: Apples’ feedback?
* AB: Mike confirmed for Apple Silicon.
* JP: We ran the CTS tests on AMD and Intel Macs, and they passed the tests.
* **_Approved_**


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Pacific-timed**) Tuesday July 09 2023, **4-5pm** (America/Los_Angeles)
* 
