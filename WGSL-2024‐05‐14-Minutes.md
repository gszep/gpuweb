# WGSL 2024-05-14 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN, KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **4-5pm **Americas/Los_Angeles (**Pacific-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial++-label%3Acopyediting+no%3Amilestone+), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+0%22+), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+1%22+)

**Todos doc:** [WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2024-04-30 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1vaICnZrSK_3xexJhJsMGkj_vtie2WHnav1gADmEP-88) 

 

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
    * Stephen White
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
* Mehmet Oguz Derin
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



* This is a WG meeting, and we will generally have WG meetings moving forward, instead of CG meetings. Please contact one of the Chairs if you have any questions or concerns about this!’


---


# ⏳ Timeboxes (until XX:25)


### [Numeric conversion rules wording is inconsistent · Issue #4538](https://github.com/gpuweb/gpuweb/issues/4538)



* DN:  Alan and I verified the key issue is overflow behavior for u32(x) and i32(x), and that was resolved in [#4569](https://github.com/gpuweb/gpuweb/issues/4569) in the last meeting.  Also I verified that the f16(x) cases do the correct thing across all scalar argument types.  I **closed the issue** as no-change-to-spec.
* JB: Mozilla’s main concern now after 4569 was that f32 was different from f16, as the most surprising thing here
* DN: We’re happy to resolve to clarify (though they are behaviorally the same) that f16 behaves the same as f32.


### [Should bitcast tests allow FTZ on inputs/outputs? · Issue #4623](https://github.com/gpuweb/gpuweb/issues/4623)



* Landed [https://github.com/gpuweb/gpuweb/pull/4640](https://github.com/gpuweb/gpuweb/pull/4640) as fix
* DN/AB: This relaxation describes current behavior on Windows drivers.  


### [Compat: Disallow `textureSampleLevel`/`textureSampleCompareLevel` with `texture_depth_2d_array` · Issue #4519](https://github.com/gpuweb/gpuweb/issues/4519)



* JB: TT isn’t here for this one, but these are pretty uncontroversial we think.
* DN: Fine by us.
* MW: Thumbs up
* Resolved to validate out


### [Compat: Disallow texture*() of a texture_depth_2d_array with an offset #4513](https://github.com/gpuweb/gpuweb/issues/4513)



* Resolved to validate out


### [Compat: Disallow textureLoad() of depth textures in WGSL via validation. · Issue #4512](https://github.com/gpuweb/gpuweb/issues/4512)



* (Was resolved in the API call, so fyi and/or here’s your opportunity to touch on it in these wgsl meetings!)


### [Difference between alignment of struct with size 8 vs vec2&lt;f32> in uniform buffers. #4497](https://github.com/gpuweb/gpuweb/issues/4497)



* DN: Let’s mark this as for copy-editing to make it more clear why (even if it’s “historical reasons”)
* 


---


# ⚖️ Discussions


### [Does an override need to be statically used by the entry point to be allowed? · Issue #4624](https://github.com/gpuweb/gpuweb/issues/4624)



* KG: Example an override ‘banana’ is in the shader, not used by the entry points, and question 1 is it valid to set ‘banana’ on the pipeline.  Question 2. If the override is statically used in the entry point and does not have a default, it must be set in the pipeline.  Kai points out that WGSL spec defines “interface of a shader”, that’s not referenced in the API spec.  But the specs do work together as intended. There’s a potential for misunderstanding.
* JB: Like that we have “typo detection”. Another question is whether there are useful diagnostics for saying you’re setting an override that’s not used.  And default it to off.
* DN: These pipeline diagnostics would have to be requested at pipeline creation time. That’s (probably?) something that’s not a compiler diagnostic, but rather an api-level diagnostic that’s needed.
* JB: So the diagnostics system that we have only applies to shader compilation for now, but not yet pipeline compilation?
* DN: That’s my intuition yeah
* JB: So maybe we would need a feature for this? It sounds like something we might want to use. “Good service would include diagnostics at pipeline creation”. 
* DN: The spec does anticipate this, and says that diagnostic issues at pipeline compilation time causes validation failure.
* AB: We could add such a feature, we just need text in the override section, since that’s where we discuss/validate overrides.
* KG: So M3 to add something like this?
* AB: Nothing before M2 IMO
* JB: M2 sounds good to me
* **-> M2**
* DN: Also though, where do we want to add this? Module-scope attribute? Elsewhere? TBD, but we’ll need to figure that out.


### [Add optional feature dual_source_blending - PR #4621](https://github.com/gpuweb/gpuweb/pull/4621)



* (PR is still in-progress)
* AB: Regarding naming, naming in WGSL doesn’t use short forms like ‘src’. Was that taken into consideration when designed?
* JB: don’t think we took _that_ into consideration, but we did bikeshed it somewhat. Personally I like full words like  ‘source’, but i don’t want to reopen if others don’t.
* KG: This has long history attached to this, prefer to not reopen.  If you want to relitigate it, need a volunteer to champion that.
* AB: That’s fine.  Let’s leave this, remember the WGSL style in the future.


### [Support "uniform texel buffer" and "storage texel buffer" ? · Issue #162 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/162)



* KG: This is showing up now, why?  Kai was going through old issues and adding tags.  This touches on WGSL. We last talked about it in WGSL in Dec 2023.  At the time we put it in M1.  As we talked in Mozilla, it would be nice to know how widely supported it is; is it optional, and understand the API side implications. On the WGSL side is there another address space.
* AB: It’s widely supported.  We’re ok moving this to M2. It would be a new texture type, with storage in the ‘handle’ space.
* KG: Seems an API proposal stage.
* AB: We have an internal doc we can expose for the investigation.
* DN: This *is* requested by partners, has real demand. But, this is lower in priority than e.g. subgroups, and we don’t expect to implement these new until much later this year.
* MM: is this implementable on metal?
* AB: i think it's just texture_buffer in metal
* MW: All Metal devices should be able to support this.
* DN: I updated to Milestone 2.


## Candidate recommendation



* AB: We have a list of editorial items that should be fixed before going for CR. If you have a list, please say so so we can address them too.


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday May 28 2023, **11a-noon** (America/Los_Angeles)
* 