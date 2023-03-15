# WGSL 2023-03-14 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** ds

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (non-APAC!)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-03-07 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1U78rLoMBQQ5EkueX2FkNnDgEfY2G5ZlLV4wszqxojB4) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * Mike Wyrzykowski 
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
    * **Teodor Tanasoaia**
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



* PR [#3945](https://github.com/gpuweb/gpuweb/pull/3945) Weaken min,max,clamp on denorm inputs
    * Happens on Apple silicon (M1).
    * Issue [#3944](https://github.com/gpuweb/gpuweb/issues/3944) wgsl: max(denorm1, denorm2) may return either value, in MSL/Metal
* Have a date and location for TPAC: Sevilla, Spain, 11-15 September 
* Link:[ https://www.w3.org/wiki/TPAC/2023](https://www.w3.org/wiki/TPAC/2023)
        * In September 11-15, hybrid. In Spain. 
    * Ordinarily the format is focused topic blocks of ~90 minutes per. 
    * Expect we would need a day’s worth to be worthwhile to travel.
    * Demos would be a different topics section. And others would be welcome to attend those demos.
    * Opportunity to meet with other interested W3C internationalization and privacy folks, etc.
    * MOD: Tiny question. Are there any joint sessions focusing on future direction of Bikeshed?
    * KG: Suggest to check the previous years’ sessions, and we can get in touch in any case.
    * MOD: Thank you!


---


# ⏳ Timeboxes (until XX:10)


### [[WGSL] Consider removing limit for maximum workgroup array size · Issue #3917](https://github.com/gpuweb/gpuweb/issues/3917)



* KG: Tabled last week
* JP: In Tint, went through limits in WGSL spec and made hard maxes so folks don't accidentally go over. This limit seemed like the odd one out. Limit already has validation in API spec. Slightly more precise in API spec, total workgroup size over all workgroups. Second, that limit is configurable. If device supports, can support more the 16k workgroup storage size. As dev would expect to work. If device supports and request 32k expect to work. Know it's a min/maximum. But having limit in WGSL seems redundant and misleading
* KG: Does this mirror the limit in API?
* JP: Unclear as currently written. As renders in spec is a link, but text is static. Is it supposed to change as api limit changes?
* AB: Up to implementation. This is a minimum-maximum. Might consider nice to users to match what API has. One thing this provides is offline tooling where no device is available provides something as opposed to only getting error online with GPU. 
* JP: Do we need WGSL limit to do that? Could still do the error even if not called out in spec
* AB: Becomes more like a uncategorized error. Those are less helpful to users, less obvious what's going on. Maybe messaging makes it ok. If it's in spec, it's easy to point at
* KG: One thing that maybe satisfying, maybe just editorial, make a stronger link saying this limit is the same as what's in API. And if that's not true, we should talk about this more. Assuming this is true and is the same as API limit then think we should have it. Can't really query this anyway.
* AB: Section 12.1. Value is then a link to the API limit.

	[https://gpuweb.github.io/gpuweb/wgsl/#limits](https://gpuweb.github.io/gpuweb/wgsl/#limits) 



* KG: **Seems editorial. Just be clear it's an inherited API limit**.
* 


---


# ⚖️ Discussions


### [wgsl: unrecognized diagnostics #3927](https://github.com/gpuweb/gpuweb/issues/3927)



* MM: Did some thinking and warnings are not prescribed by the spec. Implementation can have any warning it wants. Hypothetically imagine a warning that the user uses var when could have used let. New idea that one browser wants to implement. Think it's useful and add warning. It's our experience in WebKit and programming at large that people want to turn off warnings for certain lines of codes. In WebKit in various places we turn off a particular warning for a given line and then turn back on. We think that's pretty useful. totally reasonable for author to write shader and get to point where there are no warnings in a given browser. Seems like a natural thing for a author to do. In that world, because the set of warnings is not standardized, if they take that program and run on a different browser it would be a shame if that browser, seeing a var vs let warning, makes the shader fail to compile. Just because the author was trying to do a good job. We think the unrecognized directives should be ignored.
* JB: Agree with everything laid out above. People want to be able to work with warnings and get code clean. Lots of projects want to do that. People take advantage of locally disabling warnings in rust code all the time. Nice experience. Think, in the same vein, it's a bad experience when folks don't get feedback on their mistakes. When I typo something would like to know it's happened. It's irritating as i know the computer did the validation, it just didn't say anything. We should be designing the system to work for all of these cases. We have solutions that will support both MMs scenario and my concern. Just that they aren't things we'd like to do in v1. So, what I'm concerned about is cutting off the possibility of having the nice solutions that support all of these use cases. That's what we'll do if we promise to ignore. Thinking after the amount of discussion it would be good for me to write a real proposal. Think there is a PR I could write that would really make everyone happy here. It's not stuff we were hoping to deal with before V1 but we brought the question and there was more discussion then expected, so maybe we do have to deal with it in V1. Think given the discussion we should press on for a satisfying solution and think that solution has to involve telling people when they mistype things. Willing to dot he work to propose something and optimistic folks will be happy with the behavior.
* KG: Can you sign up for this?
* **JB: Will sign up to do this work**
* JP Clarifying question, said people don't get feedback on mistakes, as written spec doesn't disallow that, and tint actually already does tell you it doesn't know what it is. You do get some feedback.
* JB: In scenario from MM the idea is warning free coding practice the problem is that it will generate warnings in FF and will have no way to get around that.
* JP: Thanks.
* KG: Personally I think it's tolerable going into v1 with spec as written. Can make warnings if we want, would love to see what JB proposes to make things better. Think being able to turn on and off diagnostic warnings will be important but is something where if we had a spec making best practices warnings in well formed code. Would be embarrassed by this in v1 but could tolerate it. Looking forward to proposal
* DN: Proposal doesn't have to be landable PR, just description of thing
* JB: Just has to be clear and specific and concrete so folks can understand the consequences.
* KG: Have next steps.
* DN: We've continued to discuss, and don't have a unified voice yet, happy to see next steps.
* KG: JB Please self-assign the bug.


### [wgsl: consider disabling out of bounds memory accesses to shared memories #3893](https://github.com/gpuweb/gpuweb/issues/3893)



* KG: Sounded like the three things are:
    * Should probably resolve and look forward to editorial change to make spec scarier.
    * Question to AB if we should wait on more news from Vulkan or if we have all relevant news
* AB: Don't think it will be quick.
* KG: But that it would effect us?
* AB: Of course it might. I can't predict how the resolution will go.
* KG: Can pretend we think it will be fine
* DN: Looking at points from last discussion, thrust is some of these behaviours we say in the spec induce a data race and lets be honest and say it's a data race and will note that a user who has written code like this and will get wrong results and should say that and this also subsumes trapping behaviour as one of the UB behaviours is trapping. Noticed in the spec we say it's dynamic error if it's unchecked at runtime and spec says webgpu specs says what happens and it doen't . So this is an opportunity to fill that in. Opportunity to make internally consistent, call dynamic error and then WGSL spec doesn't say much about what program does, WebGPU spec may say more.
* KG: For the trapping behaviour, where we left off was MM was going to make a proposal
* MM: Yup, not done yet.
* KG: Doesn't sound like an easy proposal.
* MM: Quick note on what DN said, don't think material, but important, spec shouldn't say Undefined Behaviour. That's a trigger word, cal it something else and that's Ok.
* DN: I'ts a Dynamic Error if index  is outside bounds then find a place to say the shader may or not behave according to … might get device lost. Can't remember what it says you can touch. Don't need to put phrase undefined behaviour
* KG: Can always get Context lost.
* MM: Any situation where context gets lost is a situation which might violate the security model. Don't think we can get context lost. The context would be lost if you tried to preform the memory access out of bounds and if you did that there is a chance the security guarantee is violated so claiming any security mitigation all browsers will have to do will require as a side effect that the device won't be lost. Many other things could happen, but dont' think device lost could happen.
* KG: Don't think I agree, but disagree in a non-useful way. Think we're on the same page, but not sure if i agree with that
* MM: Maybe we intentionally choose to loose the device, but that's a different thing
* AB: One other point after looking at more stats on Spirv side. Disagreement in Vulkan, spirv- says scary UB to "form" a UB pointer. So any mitigation has to gate if any pointer is formed.
* MM: Just forming the pointer?
* AB: No agreement on if robustness fixes this.
* JB: Forming pointer restriction is same as in c++. In c++ if you form a pointer beyond bounds it's UB.
* KG: Can go 1 past and 1 before and even forming a .. pointer is ub. In practice mostly works out until compilers get smarter. The spir-v discussion will be along the lines of can we agree to do better then this and if the answer is no then we have to care
* AB: Someone might not be confident enough of all of their old architectures but that's a simple way to boil it down. As these things go across hardware generations it's hard to know how much of the market.
* KG: So, almost good news. Could have an extension saying new implementation has guarantee and anyone that does make that guarantee could use that extension. Could we plant the trees under which our children will shade themselves?
* AB: Discussion is progressing but tend to take a long time as it's a subtle discussion.
* JB: Question for AB on understanding SPIR-V spec, when it says UB in OpAccessChain spec, c++ has undefined behaviour, but POSIX says processes are contained and OS has security model to keep in process. So, c++ UB is bounded. Whats external bound of this UB behaviour
* AB: Vulkan could choose to constrain in the environment specification. One fo the two parts of question is do the robust buffer and image features fix the UB of the op access chain. That is not an immediate yes or not. 
* JB: So, if the Vulkan group doesn't know what the agreement was then we need to fall back to the OS vendors and say, does linux enforce this, does windows enforce between processes?
* AB: Don't know if the OS could do this because you're putting it over PCIe bus. Have own memory you've transferred in and page separation doesn't apply
* JB: So up to driver author?
* AB: Hardware architecture
* KG: Buck stops at IHV.
* JB: So up to authors of drivers to know what hardware will do.
* KG: Part of this is knowledge gathering and if we can figure out how to deal with it that's tolerable.
* MM: From first principles, all in agreement that clamping should be a viable thing that browser do to array and buffer indices and all in agreement that doing clamping can cause UB. That's a fact. So seems legit for the spec to say doing out of bounds might cause a data race.
* KG: Yup, think we're on board for that. Only other thing was someone mentioned saying it would be nice if we were able to say we didn't need clamping and could better define this behaviour entirely. But we should, regardless of appetite for that, don't remove clamping from v1.
* MM: Think that's right, the way you uphold without clamping is with branches and that has a different performance characteristic and don't think we should mandate that today. Maybe in the future, but right now it's the wrong solution
* KG: Resolution, make spec sufficiently scary and establish we're keeping clamping for v1 and waiting more news from Vulkan and Spir-v.
* DN: that's clear, are we going to discuss if this is a user problem, dynamic error?
* KG: The data race that caused it?
* DN: For the data races and for private/function scope.
* KG: For data race we have this language, just not clear that out of abounds can cause data race. So add the scary thing "if you access outside buffer can cause data races, data races are dynamic errors."
* DN: That's pretty good already
* MM: Think it's a bit past that, imagine you have buffer and inside is struct and inside is 2 arrays that have length. Should be legal to clamp to individual arrays instead of entire buffer. Out of bound does not mean out of bounds of buffer, but out of bounds of variable.
* KG: … type?
* DN: agreed
* MM: It's also possible to turn everything into points and the variable goes away, and so both options have to be valid.
* KG: The dynamic error is as soon as you access outside the logical bounds of the variable.
* DN: Should we still call it dynamic error for non-shared memory, function and private scope. Consistency argument, but data-race motivation doesn't hold.
* AB: If want to make dynamic error and programming error, not security issues and could remove clamping and make dynamic error and leave on user. Not portability argument, but don't have to inject code, maybe perf differences.
* MM: From security model, function scoped array, with dynamic index could leak into global memory and if don't clamp, have violated security boundary so think you need even for function scoped.
* DN: YOu do, but do we consider it a dynamic error. Can they just rely on the clamping or anything writes?
* JB: Your point is, right now it isn't an error to be out of bounds in those spaces and will we make it an error for consistency.
* MM: Feel like we should be as punitive as possible for v1, and can relax later.
* DN: I like it, that's the outcome I want. Think we should consider it a programmer error if you index out of bounds.
* AB: In my mind, remove all text and don't discuss behaviours and just say it's a dynamic error. there are multiple levels, what you read in spec is not what's being guaranteed.
* KG: Right, this is what gives you trapping, you just get it.
* MM: Historically this group has wanted a list of behaviours for the sake of authors, right?
* DN: Could be a note. You many get this or this or something else. It's a portability hazard.
* KG: Think those are helpful, but historically questions around testability why you want one of n behaviours and trapping wasn't one of them. Was imagining the minimum viable trapping was execution may stop. And that would be compatible with making it dynamic error. So, as long as no-one is opposed, sounds like everyone in the room is good? 
* DN: What's the that?
* KG:  **Indexing out of bounds is dynamic error**
* **JB: For all address spaces**
* MM: We have to have something that explains dynamic error, can't have term without text
* DN: Sure.
* KG: For out of abounds, non-normative note on you should expect read 0, or something else, or this other thing.
* MM: Not even that, can get values that don't' appear in memory, that's legal.
* KG: Sure.
* MM: So, when you do a read you get any value regardless of it's in memory. The only requirement is it can't come from outside the security boundary
* KG: We say you can get any value. I'd say it, it can be any number from anywhere, the security boundary is around this and overrides that to, I said anywhere but within the security boundary that applies.
* DN: Spec has indeterminate value, any bit pattern cast to type. It's garbage data. Doesn't matter security boundary as we don't define that thing. Have high level agreement on what happens. Split off what dynamic error is and maybe that's in WebGPU API. Maybe split in to 2 discussions
* AB: Feels like Indeterminate value isn't quite right in how we defined it. It's a frozen undefined value. A dynamic error has non-local effects. That's the key thing, you have non-local effects. WOuldn't call it non-determinate as we want that for other things.
* MM: Also if you say data races cause indeterminate values doesn't cover the case of program stops running.
* KG: Data races are not supposed to cause execution to cease. Enough for resolution?
* MM: Don't have feelling if it should be in this or that document but needs to be described somewhere
* DN: Will file spec to define dynamic error
* KG: Index out of bounds is dynamic error, DN will define dynamic error, dynamic error for all address spaces.


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**non-APAC**) Tuesday March 21, 2023, **11am-noon **Americas/Los_Angeles
* 

 
