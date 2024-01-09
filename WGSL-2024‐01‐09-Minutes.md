# WGSL 2024-01-09 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** JB, DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11a-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+is%3Aopen+milestone%3A%22Milestone+0%22), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+is%3Aopen+milestone%3A%22Milestone+1%22)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-12-05 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1Q_eCQflIiXMl5MYlHQNmg9r-1PIxI-zUWGNWsycYtYw) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


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
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * **James Price**
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
    * Teodor Tanasoaia
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


---


# ⏳ Timeboxes (until XX:35)


## Untriaged:


### [Add diagram with memory model to WGSL spec · Issue #4296](https://github.com/gpuweb/gpuweb/issues/4296)



* KG: Cool illustration, but is the spec the right place for it.
* DN: From a minimal perspective, the words are what matter, but non-normative illustrations are okay. We should also only use original diagrams. Also, this is the memory hierarchy, not the memory model. But the words need to remain normative.
* JB: Use correct terminology, and be clear it’s non-normative. Otherwise not strong feeling if this is in the spec or elsewhere.  **Make it editorial**.
* KG: Suggested timeframe: ~**M2 urgency**.


### [Are "Fine" derivatives guaranteed to actually be fine? · Issue #4325](https://github.com/gpuweb/gpuweb/issues/4325)



* KG: Step 1: clarification of existing spec.  Seems they are fine everywhere except for OpenGL ES. So it affects compat. We don’t have positive confirmation that ES in practice always gives us fine.
* JB: Kai wrote “in practice” but it’s not clear to us if that was an observation, or whether more fact-finding is needed.  If support is very limited in ES, then consider deleting the fine variants in ES, because those who ask for fine really want fine.
* AB: Makes sense. Sounds like a validation rule for compat, potentially. If “fine” is requested in compat, then it’s an error.
* KB: So, **in core spec fine always gives you fine**. Probably testable too. E.g. sign of the derivatives.
* AB: Yes.
* KB: Step 2: **compat can’t actually ask for fine**.
* JB: if we find it’s universally supported in ES, then we can enable it again.


### [Limitation on the precision of hex floats? · Issue #4350](https://github.com/gpuweb/gpuweb/issues/4350)



* **Issue closed**, with explanation on the issue.


### [document ambiguity in tie-breaks for cube map sampling face selection · Issue #4396](https://github.com/gpuweb/gpuweb/issues/4396)



* JB: The polyfill that spirv-cross inserts is a substantial piece of code. You have to recompute a bunch of things. Since the workaround looks expensive and folks using cubemaps are probably not caring that much, it’s probably better to say “should” and recommend the one behaviour but tolerate the other.
* DN: Sounds fine.  **Document the ambiguity**.


### [add swizzles for scalar->vectors conversion too · Issue #4428](https://github.com/gpuweb/gpuweb/issues/4428)



* KG: Jim suggested discussing at same time as the 0-1 swizzling.  Let’s do it later, like those.
* DN: 01 swizzling is M3+. Move it there.
* DS: Also affects parsing, e.g. (blah).xxx now needs to track if the thing in the parens is a scalar.
* **_Move to M3+_**


### [Weird behavior while using switch code block · Issue #4430](https://github.com/gpuweb/gpuweb/issues/4430)



* KG:Not a spec issue.
* BC: I already made a Tint bug for this. Close.
* **_Close_**


### [Add subgroup barrier to subgroups proposal · Issue #4437 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/4437)



* KG: Maybe an **M3+** thing. Add later.
* JB: Raph’s example, says workgroup barrier make it not worth it.  
* AB: Would have to be its own feature flag distinct from the main subgroups feature. Raph’s example also uses assumptions about mapping subgroup IDs with local Id subgroups, that are not enforced by Vulkan.


## M0:


### [Shader extensions are redundant · Issue #3961](https://github.com/gpuweb/gpuweb/issues/3961) 



* (let’s just check in on this)
* KG: Still think not necessary. 
* JB: Find the redundancy valuable.
* DS: Google did not have consensus on the enable_corresponding_wgsl_features.
* KG: Call to action: **think about it again**.


## M1:


### [Uniformity annotations for global variables · Issue #1791](https://github.com/gpuweb/gpuweb/issues/1791)



* AB: We’re confused.  I don’t understand why it was reopened.  Don’t think it’s necessary.
* JB: In terms of making an assertion about the variable, “treat this as uniform”; we didn’t think that was value.  It’s nice for the user to state their assumption and make a request to check; then they can treat as an error or warning. It wouldn’t change the conclusions of the analysis.
* BC: That’s nice, but would be hesitant to add more controls to the uniformity analysis until someone requests it in a real example. So far what I’ve seen is confusion on the part of users. 
* JB: This is not more control. It’s helpful for people who are confused.  You can annotate intermediates and have it checked.  Strictly gives more information about what the uniformity analysis thinks.
* AB: It’s not exactly. It’s an assumption currently that writable global variables are non-uniform.  So now you’re adding more endpoints to the check to verify. It’s not free because it’s not something that’s implemented.
* BC: Most of the folks I’ve been helping aren’t even sure what uniformity means. There’s a certain level of learning to be done before we give more tools.
* JB: Platforms do care what uniformity is. 
* DN: Sounds like what you want is uniform(X) that you can then static-assert on.
* JP: That’s a different use case than what is being requested here.  E.g. tf.js ran into this; we had to change Tint’s output to accommodate.
* DN: Still don’t think we’re in consensus that this is good to add.
* AB: Happy to push to M3+, We’ve had solutions for folks so far. Don’t think there’s a pressing need.
* JP: Clients that run across this in their own code can pass values down as parameters.
* **_Move to M3+_**


### [Add PI and TAU builtins · Issue #4100](https://github.com/gpuweb/gpuweb/issues/4100)



* 


### [Feature request: textureQueryLod equivalent · Issue #4180](https://github.com/gpuweb/gpuweb/issues/4180)



* 


---


# ⚖️ Discussions


### [`AbstractInt` conversion ranking forces unfortunate compromise · Issue #4291](https://github.com/gpuweb/gpuweb/issues/4291)



* Also: [Conversion from AbstractInteger to i32 appears to be underspecified #4415](https://github.com/gpuweb/gpuweb/issues/4415)
* (needs milestone)
* DN:  
* JB: I think this should be M1, since it’s a bugfix of a feature that we already added. 
* MW: Also noticed an issue converting abstractFloat to int32.
    * [https://github.com/gpuweb/cts/issues/3253](https://github.com/gpuweb/cts/issues/3253)  (shader,execution,expression,binary,af_addition:scalar:* has incorrect expectations #3253)
* AB: Think Ryan was looking into this today.
* BC: Part of a bag of features.
* BC: Think this is a bugfix that would land ASAP.
* JB: Agree this is a bugfix.
* **_#4415 Resolved, needs a PR.  _**
* AB: Is #4291 also closeable?
* JB: **Yes**


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Pacific-timed**) Tuesday January 16, 2023, **4-5pm** (America/Los_Angeles)
* _(next pacific-timed, from JS)_ [HW Clip Planes support · Issue #390](https://github.com/gpuweb/gpuweb/issues/390) 
* 