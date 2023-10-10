

## WGSL 2023-10-10 Minutes

**🪑 Chair:** AB

**⌨️🙏 Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los\_Angeles (**Atlantic-timed**)

**Specification:** https://webgpu.dev/wgsl

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-08-08 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1UCUVHwilb1K71GKtUikB0tnQbhOZ55E_jxeZg8l5yyQ/edit?usp=drive_link)

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.



---



## 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



*   Apple
    *   Dan Glastonbury
    *   Mike Wyrzykowski 
    *   Myles C. Maxfield
*   Cocos
    *   Huabin Ling
    *   Zeqiang Li
    *   Zhenglong Zhou
*   Connecting Matrix
    *   Muhammad Abeer
*   Google
    *   **Alan Baker**
    *   **Antonio Maiorano**
    *   **Ben Clayton**
    *   Brandon Jones
    *   Corentin Wallez
    *   dan sinclair
    *   **David Neto**
    *   Ekaterina Ignasheva
    *   Kai Ninomiya
    *   **James Price**
    *   Rahul Garg
    *   Ryan Harrison
*   Intel
    *   Hao Li
    *   Jia A Chen
    *   Jiajia Qin
    *   Jiawei Shao
    *   Narifumi Iwamoto
    *   Shaobo Yan
    *   Yang Gu
    *   Yunchao He
    *   Zhaoming Jiang
*   Kings Distributed Systems
    *   Daniel Desjardins
    *   Hamada Gasmallah
    *   Wes Garland
*   Microsoft
    *   Damyan Pepper
    *   Greg Roth
    *   Michael Dougherty
    *   **Rafael Cintron**
    *   Tex Riddell
*   Mozilla
    *   Erich Gubler
    *   **Jim Blandy**
    *   Kelsey Gilbert
    *   **Teodor Tanasoaia**
*   UC Santa Cruz
    *   Reese Levine
    *   Tyler Sorensen
*   Unity
    *   Brendan Duncan
*   **Dominic Cerisano**
*   Dzmitry Malyshau
*   Eduardo H.P. Souza
*   Jeremy Sachs
*   Joshua Groves
*   **Aura Munoz**
*   Lukasz Pasek
*   Matijs Toonen
*   **Mehmet Oguz Derin**
*   Michael Shannon
*   Pelle Johnsen
*   Robin Morisset
*   Timo de Kort
*   Tyler Larson
*   Jason Erb



---



## 📢 Announcements/Meta


#### Office Hour



*   **Wednesday 10am-10:50am**
*   **https://meet.google.com/xrp-hpck-vmy**
*   Everyone welcome
*   Mass calendar invite will have been sent out
    *   If you still need an invite, add your email here:
        *   [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        *   


#### FYIs and Notable Offline Merges



*   


#### Milestone 1



*   Are we ready to land changes for milestone 1?
    *   JB: Mozilla still implementing the “v1”. Don’t hold back the spec.  Snapshot would be nice. Fine to add new features.
    *   TT: Adding extensions, for things to core.
    *   AB: We have “language” extensions that can appear anywhere, and “enable” extensions which depend on HW.  There are PRs up for the first kind.
        *   https://github.com/gpuweb/gpuweb/pull/4312  fewer restrictions on pointer parameters. Can discuss later.
    *   TT: Q is mainly if there is optional functionality we’re talking about.
    *   AB: What WGSL describes as “language features”, all implementations are expected to eventually support them.
*   Do we wish to snapshot the “v1” version of either/both specs?



---



## ⏳ Timeboxes (until XX:15)


#### [Triage milestone 1 issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl+milestone%3A%22Milestone+1%22)



*   Any to push?
*   Any issues that should be pulled in? ([untriaged issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl+no%3Amilestone))
*   AM: Want the equivalent of  gl\_PrimitiveID.  Currently in M3.   What’s the process for pulling in? https://github.com/gpuweb/gpuweb/issues/1786 
    *   AB: Post comment saying you want this. We need that feedback. Suggest a first round of your own triage
*   AB: https://github.com/gpuweb/gpuweb/issues/1791  suggest pushing this out since we have the opt-out. 
*   AB: Call to action.  Homework: Please make your own opinion known.


#### [Simplify @builtin() notation](https://github.com/gpuweb/gpuweb/issues/4304)



*   AB: Previously decided against this, happy to leave as is
*   JB: Mozilla favours status quo.
*   **_Agree to leave open, but not revisit soon._**


#### [AbstractInt conversion ranking forces unfortunate compromise](https://github.com/gpuweb/gpuweb/issues/4291)



*   DN: no desire to have values affect type checking
*   JB:   Example shows it.  
*    Fails because that literal doesn’t fit into i32.
*   JB: Still would like to see a change to bitcast.  Think Bitcast should have an overload for AbstractInt to make it work if it the value fits in the target type.
*   AB: I’m a bit on the fence on this.  Maybe wordsmithing can help.  AbstractInt is 64 bits now but could be expanded. Do you feel -1 should work the same way? Once you have all the leading 1s, is it in range or not is a question.
*   JB: 2\*\*32 - 1  is a representable as u32. It’s unfortunate we have all these first class citizen values. I’d be happy to say bitcast to a type should be able to accept all the values targeting that type.
*   AB: Is it bitcast the only one that is problematic?
*   AB: The constructor with u32(-1) feels in the same vein as you’re asking for.
*   JB: -1 isn’t in range for u32.  That’s a step beyond what I might like.
*   AB: We made a special case constructor for abstract int.
*   DN: could probably get behind this because AbstractInt was not meant to have a bit representation, but we says it needs at least 64 bits. Would you extend to f32?
*   JB: probably not. Fewer variations for integers.
*   DN: I’m ok with adding bitcast<T>(AbstractInt), must be value-preserving.
*   _Consider resolved; let’s look at the PR._


#### [#4312](https://github.com/gpuweb/gpuweb/pull/4312) Remove restrictions on user-defined pointer parameters



*   Review WGSL language feature name `unrestricted_pointer_parameters` 
*   AB: David merged this prematurely.  Two questions. 1. Review language feature name,  2. Intentionally didn’t want to pollute the spec with a mass of conditionals and interactions.  In this case it’s a deletion of a bunch of text (which is now lost).
*   JB: As long as it’s sufficiently clear what’s disabled, it’s ok ot generally describe the spec as all features are on. 
*   DN: In this case it’s an entire deletion of sets of conditions.
*   TT: Descriptions is too vague in this case.
*   AB: This case we can reword.  Pull the restrictions into the description of the feature.
*   JB: That sounds better.
*   BC: The name is quite wordy. The language uses ‘ptr’, use it there?
*   **_Leave the name as is._**
*   AB: I’ll revert this, make changes,** tag the spec, and I’ll draft the new changes.**

 



---



## ⚖️ Discussions


#### [DP4A](https://github.com/gpuweb/gpuweb/issues/2677)



*   PR: [#2378](https://github.com/gpuweb/gpuweb/pull/2738)
*   AB: Google wants to ship this.
*   AB: After internal discussion, we asked to add the accumulator back since it will be polyfilled if support is missing; map more directly to HLSL.  Can supply 0 to get the non-accumulating form.
*   SJ: Then we need to consider the overflow issue on the accumulator
*   AB: Language extension name is a suggestion.
*   AB: Assume overflow is like any other integer add (on concrete type) which is wrapping behaviour. In SPIR-V the accum is separate instruction, so assume wrap.  Need to check what HLSL says/ spir-v says dot product overflow yields indeterminate value.
*   JB: In general use this applies to u32 read from a buffer.  If you did the pack/unpack yourself you probably lost the throughput.
*   AB: Pack/unpack is for assembling data, and supplying a constant. Yes, generally think the data comes as u32 from a buffer.
*   JB: No strong opinion on accum/non-accum.
*   AB: Chrome/Dawn has experimental implementation of non-accumulating.
*   JB: Fits directly onto a machine instruction.  Is your peephole optimization not effective?
*   AB: That’s a reasonable expectation. SPIR-V arrived at a different answer than HLSL.  SPIR-V thought addition was only worth combining for the saturation case.
*   JB: ML folks seem to have moved to 4bit values, and so maybe don’t worry too much about this optimizing perfectly. Let’s ship something here ASAP.
*   AB: If we prefer to leave it off, then we’re ok with that.
*   JB: easier to document the feature without accumulation.
*   _Consensus to keep it as the non-accumulating form._


#### [RW and RO Storage Textures](https://github.com/gpuweb/gpuweb/issues/3838)



*   AB: Google would also like to ship this
*   AB: have an experimental implementation with subset of formats available behind flags in chrome
*   AB: partner request
*   —-------
*   AB: Thanks Teo for all the work driving this.  My understanding is we can make this core language feature.  The old apple devices are aged out.
    *   On the API side they have to validate “original tier1” in core, and “original tier 2 + 3” as a separate GPUFeature. But WGSL just adds the enums without distinguishing.
*   TT: yes, think so.
*   JB: Barrier is for visibility between invocations in a workgroup.
*   AB: Self-fence is required, and will be injected by our backends for Metal. 
*   _Consensus.  Get a “language” feature that adds readable storage textures, and read-write storage textures.  API side finalizing elsewhere._

Teo: What about adding write-only storage buffers?  That’s missing from the matrix.

AB: I remember discussing this. We didn’t expect there to be a performance benefit, so it didn’t seem necessary to expose it.

Teo: Can see that storage buffer is different enough from storage textures.


#### [Shader extensions are redundant · Issue #3961](https://github.com/gpuweb/gpuweb/issues/3961)



*   _No discussion_



---



## 📆 Next Meeting Agenda Requests



*   Next meeting?: (**Atlantic-timed**) Tuesday October 17, 2023, **11am-noon **Americas/Los\_Angeles

 

 

 
