# WGSL 2022-10-11 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DS

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** **(non-APAC!)** Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-10-04 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1cRSiOceeZ6u32XQDipA2mo-KsTC1_nRhCS9IVN1gqeE) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Myles C. Maxfield**
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Price
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
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * **Tex Riddell**
* Mozilla
    * **Erich Gubler**
    * **Jim Blandy**
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



* 


### CG Charter



* **WGSL Only: [https://github.com/gpuweb/admin/pull/19](https://github.com/gpuweb/admin/pull/19)**
* 


### More APAC meetings during Nov and Dec?



* Previously:
    * MM will be out Nov and Dec, but DG can make APAC timeslots. Shall we have more APAC timeslots for these two months?
    * Generally: yes, more APAC is fine, though we generally won’t have BC for APAC timeslots. (Current APAC slot is 1am for BC)
    * DN: I think more-APAC-meetings will slow down BC’s great feedback for Nov+Dec, but that’s alright for the short term.
    * DG: We might be able to move the slot an hour earlier (which I can still make) and make things slightly more reasonable for people further east. Though China is two hours off from me I think, so we should check with Intel Shanghai. 
    * KG: I’ll talk with CW and we’ll check with our non-APAC members (who aren’t all here today) for more feedback.
* 


---


# ⏳ Timeboxes (until 11:20)


### [Validating maxComputeWorkgroupStorageSize #3485](https://github.com/gpuweb/gpuweb/issues/3485)



* DN: way of counting up sizes of workgroup size and saying if beyond we'll fail pipeline compile. Round up size of each var to 16 bytes. There was one case I thought about that had to check for. Think we can ignore those cases (or lump into uncategorized error). Think proposed change is reasonable.
* MM: There is a packing algorithm in Metal, it isn't controllable by author, get packing you get. In metal compile shader, after compile ask how much space used. Thought, probably not worth that kind of dance, willing to lean on the uncategorized error. As long as webgpu counting has low false positive rate then we're ok in rate situation where packing is slightly off and we can't compile to say uncategorized error. As long as it's rare.
* AB: Think uncategorized is good, not going to do any better.
* MM: So rounding up variables to 4 bytes is pessimistic enough and would be fine.
* KG: Proposal is make this change and we can uncategorized error when we need too
* DN: On table is round up to 16 bytes
* MM: Even better. If authors complain …
* KG: **Make it so**.


### [invalid expressions in const-expr and override-expr should be error (no later than pipeline creation time) #3253](https://github.com/gpuweb/gpuweb/issues/3253) 



* DN: At some time, split into runtime vs compile time. All changes to pin down compile time have landed so think this is candidate for closing. Would like to close.
* KG: Sounds right to me.
* DN: **Cool, closed**.


### [wgsl: bit shift width is always u32 or vector of u32 #3516](https://github.com/gpuweb/gpuweb/pull/3516#top)



* (Fixes: wgsl: (1 &lt;< 2u) should be 4 (an abstract int)[ #3511](https://github.com/gpuweb/gpuweb/issues/3511))
* DN: Weird issue BC noticed. When doing shift the type of result should be type of LHS (number shifting). The way abstract int case had both concrete or both abstract. So, could have cases like in the title where it's just weird. Making change makes it more nature. Seemed like a nicer way to keep extra behaviour without forcing shifted number to be concrete
* MM: Was confused 2u isn't' abstract. Literally said u, but was surprising 1 &lt;< 2u that we haven't had this problem. Is there a way to have unsigned abstract?
* DN: You use no suffix, if the value is non-negative then it's non-negative. Can only have an abstract when you know the value.
* KG: So, instead of 1 &lt;< 2 would that work?
* DN: That would work, you get abstract 4.
* MM: Before this patch if either of the 2 params were concrete then the spec was written such that both had to be concrete. The suffix list value would get concretized right there. This patch says they can be distinct so LHS abstract and RHS concrete which is good, just surprised it was a problem in the first place.
* KG: Title was confusing, but **sgtm**.


### [wgsl: Define "indeterminate value" #3508](https://github.com/gpuweb/gpuweb/pull/3508)



* (to fix [#2888](https://github.com/gpuweb/gpuweb/issues/2888))
* DN: About runtime expressions when using nonsense values (or unsupported values) this is an undefined value except for each time you observe it it's the same value. It's a particular value at the execution. In LLVM terms. a frozen undef. No metastable aspect. Think that's what we're trying to capture for a value we don't want you to rely on but won't cause UB if branched as it has a specific answer
* KG: About uniformity?
* DN: No
* MM :Image if (x > 3) {} else if (x &lt; 3) {} do something else. Or hardware is capable of handling Nan. In this program if we made a nan (the frozen value is nan) then the program about is confusing. The program that would be have "correctly" is if x > 3 else {} that would behave correct in that one branch would happen. Were thinking about nan case and would like to be able to make nan as the frozen value (not necessarily all the time)
* DN: Yes, should be able to operate correctly however the translation isn't if x > 3 do something x &lt; 3 it's the negation of …
    * When x is float, this is how you should split up the “x&lt;3” into false and true cases.

		If (x &lt; 3.0)  { … }

 		If ( !(x&lt;3.0) ) { … }



* AB: Be careful about how you think about complete comparisons when doing ordered comparisons.
* MM: Official request is we would like frozen values to hold nans. Not proscribed but capable.
* DN: By my understanding the answer is yes. Nan is a valid bit values in metal and that is a valid bit pattern of the type and if it makes a nan it would be carried forward.
* KG: Maybe related, for nans is indeterminate value useful ot talk about? For implementations which don't support nan we said you get a different value back. Is this a useful framework to describe that? Say implementation does 0/0 and don't support nan, do we have any promises about that nan or can we say it's a frozen value? Or do we not have that.
* MM: The original calc is allowed to not produce nan, even if ieee says it should. If calculated nan and feed into another computation, that second computation may accept the nan, but not return nan, which is also against ieee. And the compiler is allowed to reorder computations, which means the “second” computation may actually appear earlier in the program source, or be hoisted up from a branch, or may not ever appear in the program source at all.
* JB: Looking at PR on line 4595 it has value extract which is the example of indeterminate value and says if extract == extract and has comment says always executed. If extract is nan, the reason this example is ok is because it's i32. If the example was f32 it would not be the case.
* DN: Correct, should expand to have float example with nans discussed.
* JB: 2 reasons compare could fail, 1- UB and 2- Nans involved. Don't want the first but want the second. Would be nice to have explicit.
* KG: So, want **PR changes**?
* JB: Yes, would like example expanded with note.
* DN: **Put note into PR**.


### [Uniformity analysis is far too strict #3479](https://github.com/gpuweb/gpuweb/issues/3479)



* JB: Want to extract as much information from folks now about what they're hitting because as soon as we have an escape hatch we won't get the feedback. Would be good to organize issue to have a list of issues being encountered instead of just discussions.


---


# ⚖️ Discussions


### [wgsl: pointer-to-workgroup in user-defined functions · Issue #3276](https://github.com/gpuweb/gpuweb/issues/3276) 



* AKA “function-out-param language functionality”
* See also:
    * (pr) [wgsl: ban ptr-to-workgroup params to user-defined functions #3422](https://github.com/gpuweb/gpuweb/pull/3422)
    * (issue) [Tuple-like-returns proposal for function-out-params functionality #3501](https://github.com/gpuweb/gpuweb/issues/3501) 
* KG: What I would like is consensus, direction that satisfies various desires and we're trying to find it.
* MM: Had a discussion since last call and we think that the proposal by BC is great and we should do it and it's wonderful.
* JB: Mozilla is less enthusiastic about the proposal. Agree it addresses problem but looks like heroics. Even granting on FXC everything is inlineed anyway we feel  that on other platforms doing aggressive transforms like the one shown reduces predictability of the platform. That is people write something and suddenly get multiple copies of function and no indication in what they wrote to see that would happen. So, if worried about register pressure it makes that harder. Much bigger difference between what was written and what is executed. This seems undesirable. Also a large amount of implementation complexity. One way we talked about towards consensus, reading between the lines it feels like Apple is prioritizing having clean answers for folks writing WGSL, how do I write this. Apple wants an answer that reads well and that folks make sense, Google especially and also Mozilla we are thinking about clients converting large code bases mechanically to WGSL. An application less concerned with output or comprehensibility. We want to support folks with non-sophisticated transformations. Not doing full AST but doing something local and really want local transformations for language they're translating into WGSL.  We were wondering if it would be an accessible compromise to stop talking and release v1 if we had both the tuple like returns which is something Apple could teach/show folks as the nice way but also restrict pointers to what hte platforms can reliably do. That is take out ptr-workgroup to functions and not do the heroics of BC's transform. Restrict pointers to what they can do well. Then Apple has something folks like and folks doing translations can use ptr for inout and we could move on with this.
* BC: So, transforming SPIR-V to something with multiple return values is going to require heroics, substantial heroics. Probably more then the transform written. Ignoring the conversion from spir-v multiple returns requires transforming function calls and there would be heavy duty transformation that would be required to support multi-return. At least for Tint, it's substantially more code for multiple-return values.
* JB: Are you talking about generating SPIR-V from WGSL with multiple return values
* BC: For ingestion of SPIR-V would have to do heroics
* JB: That's a misunderstanding, WGSL would have both, you'd have ptrs as they are now and it would have MRV. If you ingest SPIR-V you convert to ptr. If you're writing WGSL you can have multiple return values. Only worry about spir-v as output. Code that uses ptr in spirv continues to work. 
* BC: So, where you use MRV in user written WGSL we'd have restrictions they have to be statements or something. That's more implementatable. Not speaking for Google but not my preference
* JB: Not my preference either but want to reach consensus. My pref is spec as is. Ptrs as are, no workgroup and no MRV. But realizing that isn't satisfying for Apple and hoping if there is a way to write code they want to then we can write what we want too
* GR: The only discussion isn't just technical effort but political effort to get everyone on the same page.
* MM: Both option isn't very satisfying as most of objections to ptr were about the addition of a language feature that has so many carve outs in it. So, saying we'll add a language feature with carve outs and a second language features doesn't fix situation. Second thing, a week ago made post and said 2 reasons why initial version is distinct, 1. it could be true that future versions of wgsl could be opt-in. Please give me version2. 2- because I was under the impression when I wrote this that a less carved out language feature for pointers was hardware dependant and so then therefore even if the language moves on some authors will be stuck on the old version. The reason we like BCs proposal so much is it deals with 2nd issue so well. Having ptrs isn't hardware dependant and can be use anywhere and makes future more comfortable and if that future isn't opt-in it's really helpful. EVen if we say ptrs are bad no and the future isn't' opt-in and everyone can be upgraded that would be sufficient to agree to the less powerful pointers with the carve out.
* KG: Do think it's mostly y'all who think the current state of points is so bad and don't think it's consensus that it's true. But understand that's the position.
* BC: Leaning more towards Apples stance (no GOogle position) If I was to try using WGSL in aggression would get angary.
* DS: want clarification from Myles: would be satisfied if  future would have more fully featured pointers as core required and not dependent on hardware feature.
    * MM: Yes.
* KG: No the implementation cost up front is what we're worried about. Main concern is the ongoing cost which we aren't sure of. THe single biggest thing is folks with non-trivial shaders with non-obvious changes and get compile time blowups on some backends. Think that's closer to the part which is most concerning, not the technical implementation.
* JB: It's that it's a surprising transform. Wouldn't have imagined it until BC pointed it out. It's similar to JS where if you know what you're doing it's fast but if you get it wrong it isn't fast. Similar here where perf could be affected.
* DN: To rephrase get what you're saying and to amplify, you get great behaviour on Mac as no transform but ship somewhere else and Boom. Or I made this change in my code and it goes Boom. When talking to BC, yes you can have more code gen but if there is a saving grace middle ground if when you change that code you get immediate feedback that it got worse. Not sure how controlled that is but an aspect. If wrote WGSL pretending GLSL and don't use feature you don't pay for it. If everything pass by param then will pay for it. In between state has mixed cost and complexity. Want tight connection between what you write and what you get.
* BC: 2 backends we care about for transform. SPIR-V, definitely jumping through hoops in spir-v base. Fairly confident vulkan is moving towards variable pointer support rapidly. So this is a short term transform for vulkan. Other is hlsl where you have less heroics. Other pointer types you don't deal with. Hlsl doesn't have a solution. If emitting directly have to do transforms for pointers, no getting around that. Uniform buffers you have to do something. Storage buffers are slightly different and you need to do something. If you want pointers we _have_ to do something for HLSL. But, in terms of surprising transforms FXC forcefully inlines and don't think there is a substantial difference if you do or don't in what the compile will emit. DXIL is a different thing where there could be a substantial difference. think the cases where this happens are more limited then vulkan side. That's my counter, but don't think it's as bad as I initially thought.
* MM: Little support to that on gpuinfo 75% of windows vulkans support variable pointers. On android 85% support variable points
* AB: That's not what our internal numbers say.
* JB: Can't use GPUinfo. It isn't statistically significant. Just folks who ran program. Need to use telemetry like Chrome is collecting. Wanted to say, yes, what FXC is doing is way more of an aggressive transform that what we're proposing and that weakens argument. Was I correct in that Apple would be willing to take pointers as now if there was a good faith understanding in the group that in the future either hardware gets feature without opt-in and Mozilla and others agree for other platforms that don't support ptr then we would use the transform. So underpowered would use transform and majority would use pointers. Is that a way forward?
* MM: Yes. Some time concerns, can't come 20 years in the future but operating in good faith then that's reasonable. 
* BC: So that would be v1.0 have partial pointers and 1.1 having full ptr.
* JB: As long as we have a chance to not do this to most people and we can try to put off the complexity of the implementation then maybe everything will be fine and we do nothing but it turns out we do for some cases think we could settle on that. Speaking on my own feelings, not Mozilla position.
* KG: Curious about wanting it to be not opt-in. 
* JB: That was part of the stipulations from MM. Smooth upgrades. I like that too.
* KG: The concern traditionally is that it gets into the talk about how we go about expanding the language which we always said was opt-in for portability.
* MM: Yes it does. Two thoughts, the reason we don't want opt-in, it's all tied up in having a minor feature that has carve outs is unfortunate for language if in the future there is nothing an author can do to get that version that's fine. The future is larger then the past. As long as the time is fintie for all browsers to get then that's fine as the carveouts just won't exist. For portability, at the risk of re-opening large and complicated topic, at least in JS they add functions to STD they add syntax and classes as long as new things don't cause previously correct code to operate differently it's fine. In this particular case, without getting into language extension argument, think it's reasonable to just add new power to ptrs in the future as programs which would have failed to compile would fail to compile. It would fail to compile on every browser and authors would not write them.
* DN: This is a good concrete case of one of the arguments in that discussion. A desirable outcome is a future which is infinite with powerful pointers and we should aim towards it
* MM: That makes sense from  a Google perspective that if variable pointers are coming everywhere then WGSL should just get variable ptrs.
* KG: Given long tail of devices it's hard to get behind.
* JB: BC are slides somewhere
* KG: In last weeks notes and in bug issue 
* BC: Need consensus on how we scale the language forwarded. In terms of now, v1 spec as is. v1.1 without explicit enable we get ptrs. Does that mean wg has no started to spec 1.1? Is this the first 1.1 spec meeting?
* DN: Yes.
* JB: I think so.
* EG: That's a healthy space when we need to start compromising
* JB: This is slide 28. Keep spec as is, lift restrictions in 1.1 Nothing to change in 1.0 use transform in 1.1 to lift without full hardware support. Maybe this is the way forward.
* KG: One more week to think about this?
* DN: The rest of spelling, and naming. I'm happy with this direction.
* KG: Think we're tentatively Ok, but want to talk internally, so want one more week. Excited about getting consensus here.
* RC: For leave it as is, what does that mean?
* JB: No ptr-to-workgroup args to user defined functions. No uniform pointers, no storage buffer pointers. No pointer to subobjects.
* RC: So, what can you make a pointer too? Nothing?
* AB: Restrictions are mostly around function-params. In function can make pointer to anything. Need to distinguish use cases. Passing between functions has restrictions. Address space issues, workgroup issue and a whole object restrictions. What the spec calls a root identifier which is either the param or variable name.
* RC: Can pass pointers to things but not into things
* MM: Things have to be local variables in private or function address space.
* RC: Ok. Would be good to put whatever slide number into the spec at some point so we have it there, including all outstanding PRs.
* GR: 21 is slide number
* JB: Have separate doc from KN about how WGSL goes to implementations. That would be the place for this.
* RC: Ok, thanks.
* GR: Don't believe that the fact 1 platform supports all these features is really a slam dunk, as if we are this isn't' the meeting I thought it was
* JB: Strong agree.
* KG: **Mozilla will talk about this, signs point to yes but not finalized. Back next week once more.**


---


# 📆 Next Meeting Agenda Requests



* Next week: (**APAC**!) Tuesday, October 18, **5-6pm** (America/Los_Angeles)


### [wgsl: Define "indeterminate value" #3508](https://github.com/gpuweb/gpuweb/pull/3508) 



* DN: Updated the PR to call out NaN as a possible value in the f32 case. Provided an example.