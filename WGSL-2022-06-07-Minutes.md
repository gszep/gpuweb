# WGSL 2022-06-07 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **5-6pm** Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+is%3Aissue+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-05-31 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1wu4YFqbKGr-KupbstECDGM3hmMEv7VsO49Uy-JRcNxQ) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Daniel Glastonbury**
    * **Myles C. Maxfield**
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * Dan Sinclair
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Price
    * Rahul Garg
    * Ryan Harrison
* Intel
    * **Hao Li**
    * Jiajia Qin
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * Jim Blandy
    * **Kelsey Gilbert**
* Connecting Matrix
    * Muhammad Abeer
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* Dominic Cerisano
* Dzmitry Malyshau
* Eduardo H.P. Souza
* Jeremy Sachs
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson


---


# 📢 Announcements


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


### FYIs and Notable Offline Merges



* [https://github.com/gpuweb/gpuweb/pull/2995](https://github.com/gpuweb/gpuweb/pull/2995) ldexp: all operands are concrete or all operands are abstract.  See[ #2993](https://github.com/gpuweb/gpuweb/issues/2993) for general rationaleM
* [https://github.com/gpuweb/gpuweb/pull/2906](https://github.com/gpuweb/gpuweb/pull/2906) Use table formatting for texture builtins.
* [https://github.com/gpuweb/gpuweb/pull/3008](https://github.com/gpuweb/gpuweb/pull/3008) Reformat builtin tables


---


# ⏳ Timeboxes (until 5:30)


### [wgsl: Allow phony assignment of abstract numeric type #2968](https://github.com/gpuweb/gpuweb/pull/2968)



* DN: No reason to disallow it.  Was a weird corner/ irregularity, when Ben was implementing it.
* AB: Any reason to keep any type restrictions at all?  Maybe runtime arrays might be impossible.
* KG: Should be able to phony assign anything that would be assigned to something else
* AB: Well, it’s more like allowing ‘const’ decl.
* KG: Phony should be able to accept anything that you would be able to feed into a ‘const’ decl.
* MM: Would be unfortunate to have to do the out of bounds check in this case, and force a failure.
* _Approved._


### [Add saturate() or clamp01() #3012](https://github.com/gpuweb/gpuweb/issues/3012)



* DN: Someone noticed that it’s missing from WGSL, and that it exists in e.g. hlsl and MSL and that e.g. AMD has hardware support for this, so I think it’s worth adding.
* MM: *The* reason for accepting this isn’t performance, because we can recognize this anyways. The difference is that this should be helping authors, about being nice to authors. I think it should be called saturate and not clamp.
* KG: Also feel should be nice to authors.  Clamp01 makes more sense to me.  ‘Saturate’ sounds like a self-modifying verb.  Comes down to where we come from in our programming culture.  I’d have to look up what ‘saturate’ means. That’s my vote, but not strongly held.
* KG: Unity have clamp01.: [https://docs.unity3d.com/ScriptReference/Mathf.Clamp01.html](https://docs.unity3d.com/ScriptReference/Mathf.Clamp01.html) (but this isn’t a shader language)
* GR: Can we have both?
* MOD: Clamp01 reads to me as a different version of ‘clamp’.  Looks like a version not a function.
* KG: I’m resigned to it being saturate instead of clamp.  I still would have to go lookup.
* KG: Compiler should translate the idiom   clamp(x,0,1) and translate it to saturate call. 
* _Agreed to ‘saturate’_


### [Specify a minimum SizeOf for a runtime array #2997](https://github.com/gpuweb/gpuweb/issues/2997) 



* DN: Editorial.  Think already exists in totality of text of the specs.


### [Specify that structures compare by name, not by structure. #2994](https://github.com/gpuweb/gpuweb/issues/2994)



* KG: Structs are not “duck” typed.  It’s by-name match instead of structural.
* Agree this is the way it is. Make sure spec says it.


### [wgsl: comparison operator result is scalar bool when "is scalar" should expand to include abstract numerics. #2988](https://github.com/gpuweb/gpuweb/issues/2988)



* editorial


### [wgsl: Other language types in reserved word list #2983](https://github.com/gpuweb/gpuweb/issues/2983)



* DN:  Bunch of easy cases (like texture types in other languages), an remove them.  But there are some like i16 and i64 that  are on likely path to being in WGSL.
* KG: Agree to reserve those.
* MM: Why. We have a forward path that allows us to add those and have user defined things override WGSL-predeclared-things.  We have to have a solution to that problem.
* AB: That’s not what the spec says right now.
* KG: Don’t want to redefine ‘true’ ‘false’.
* AB: Agree we need a plan for expansion in that way.
* KG: Need an issue
* DN: [https://github.com/gpuweb/gpuweb/issues/2941](https://github.com/gpuweb/gpuweb/issues/2941) , and make that non-editorial.


---


# ⚖️ Discussions


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* MM: Internal discussion raised: 
    * If compiler can prove that two pointers don’t alias, then in some programs the compiler can generate better code around those pointers.
    * If compiler knows two pointers do alias, then compiler can emit better code in some situations.
    * For WGSL we know the whole program. (e.g. could inline the world).  So reason for restrict keyword in C doesn’t really apply for WGSL.  (There are not multiple compilation units).
    * For that reason, there’s no good reason to say that a program that has aliasing pointers ever has a reason to behave nonportably. Should treat aliasing pointers that just arises naturally.  Don’t think there’s a large benefit from making that assumption.
    * If there is a read-write-read sandwich then …
* DN: So the compiler audits far enough that the compiler handles it. Can this ever lead to combinatorial explosion?
* MM: It’s a trade-off, so the compiler would have to decide. It wouldn’t *have* to.
* DN: Spir-v has the assumption that there’s no aliasing, so the compiler would need to protect against aliasing, and emit special aliasable versions.  Could lead to compilation in the browser to explode.
* MM: We were thinking, if you were representing in a language that doesn’t allow aliasing, you’d essentially have to de-dupe pointers. Because of logical addressing restrictions, this is probably ok. E.g. &myArray[foo()] vs &myArray[bar()] is tricky but should be possible.
* MM: We’re not super-sure that it’s game over, there may be another solution. I understand the point you’re making, not sure it’s a deal-breaker?
* AB: I think it’s also not good in copy-in-copy-out like HLSL. When there *is* aliasing, I don’t think it’s safe anymore, at least not trivially. I think it’s scary that we’d require whole-program analysis for this already. E.g. if we add libraries, we’d need to pessimize. 
* MM: The sandwich is, a read via one pointer, write via a different pointer, and then read via orig pointer.
* AB: For languages with copy-in copy-out languages, you’d never see the proper update in that sandwich case.
* MM: What are the current path in folks targeting D3D?
* AB: We emit HLSL, and currently compile FXC.  Too many machines with Windows to ignore.
* MM: Firefox?
* KG: Outside my scope.  Expect similar to Chrome’s experience/constraints/targets.
* GR: Would be curious what other telemetry indicates. Alan said DX11 is pervasive.  Or, DX12 that does not support DXIL is the interesting case.  Would be interested in comparing notes.  Would be willing to share it.
* AB: Not acceptable to us to limit us to requiring a DXIL backend.  
* GR: As I recall, Google told us that 40% of the DX12 platforms didn’t support DXIL.  That doesn’t ring right
* AB: Let’s talk offline privately.
* KG: In sum, for v1 launch we have languages using in/out semantics, and SPIR-V that assumes no- aliasing.  So would have to do whole program analysis to correctly support the aliased cases.
* MM: **Would like some more time.**
* AB: Think we could pessimize SPIR-V to support, using “Aliased” decoration on all memory object declarations (and maybe intermediate pointers?). My concern is more about languages with inout params.  ‘Alias’ means ‘may alias’ with other things. There’s a whole table describing the interactions.  Additionally, things could get weird if you allowed type punning.	


### [wgsl: document how much of an alignment request is checked by the API #2999](https://github.com/gpuweb/gpuweb/issues/2999)



* MM: Can we get a better explanation?
* DN: Vector of 4 floats requires a certain alignment requirement. Additionally, author can add @align annotations, and that bubbles out to the outside. 
* _Direction: it’s about packing data, not about divisors on machine addresses (when that goes beyond the HW alignments on primitive types).  **Take option 3** in [https://github.com/gpuweb/gpuweb/issues/2999#issuecomment-1148913857](https://github.com/gpuweb/gpuweb/issues/2999#issuecomment-1148913857) _


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, June 14, **11am-noon** (America/Los_Angeles)
* [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)