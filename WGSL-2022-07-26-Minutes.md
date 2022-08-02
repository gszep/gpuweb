# WGSL 2022-07-26 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-07-05 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/12XYFS987X3xxaCUwNR1-UzcSPu8GrkDpZQUEFvqCeBc) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Daniel Glastonbury
    * Myles C. Maxfield
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
* Microsoft
    * Damyan Pepper
    * Greg Roth
    * Michael Dougherty
    * **Rafael Cintron**
    * **Tex Riddell**
* Mozilla
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
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* Mehmet Oguz Derin
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson


---


# 📢 Announcements


### Call for Editors



* Corentin sent out an email beginning this process, check your email!
* 


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


### FYIs and Notable Offline Merges



*  [https://github.com/gpuweb/gpuweb/pull/3176](https://github.com/gpuweb/gpuweb/pull/3176) Describe discard as demoting to a helper invocation.
    * Introduces and describes helper invocations
    * Updates behaviour analysis.
* [https://github.com/gpuweb/gpuweb/pull/3189](https://github.com/gpuweb/gpuweb/pull/3189) renames creation-time constant, etc to const-declaration, const-expression, etc
* [https://github.com/gpuweb/gpuweb/pull/3226](https://github.com/gpuweb/gpuweb/pull/3226) rewrite value and variable chapter


---


# ⏳ Timeboxes (until 11:30)


### [@size attrib only applied to member types with creation-fixed footprint #3157](https://github.com/gpuweb/gpuweb/pull/3157)



* Takes Myles’ suggested fix for [https://github.com/gpuweb/gpuweb/issues/2997](https://github.com/gpuweb/gpuweb/issues/2997) 
* DN: Originally, something like we should not allow you to say something has a size given runtime array. Other things occur like creation time expression of abstracts and how big is abstract. Only do it for things that have a sensible size, fixed footprint things. Members of structure must name types and abstracts can’t be named. So, this is a tightening.
* KG: LGTM


### [Clarification of loop identifier lifetime #3153](https://github.com/gpuweb/gpuweb/issues/3153)



* DN: One solution is delete the paragraph: [https://github.com/gpuweb/gpuweb/pull/3247](https://github.com/gpuweb/gpuweb/pull/3247) 
* DN: DS pointed out confusing language about scopes inside loops lasting to end of loop. Proposed solution is delete the paragraph as we don’t have it elsewhere. Then resort to general scoping rules. Question in the middle about something different, the existing language disallowed a shadow case and deleting the paragraph then it stays. The wording from KG would allow a thing we didn’t want to allow. Would like to sidestep by deleting paragraph.
* AB: Concern would be if you declare a var in the initializer, thinking about compound statements, might think it’s a larger scope then otherwise, the end of the thing that encloses the loop. Want to make sure the initializer section ends at the for loop.
* DN: Paragraph is in loop statement, looking at for statement an init clause if it’s a declaration it’s scope ends at end of loop body. Already pretty well spelled out.
* KG: If deleting the paragraph works, that’s cool. Don’t see how rewording changes meaning. Would be cool to understand how declarations in for loops are like scopes but not actually scopes. Like, 2 for loops using `i` but can’t use `i` inside the scope.
* DN: A scope is ‘blah’ isn’t really defined thing. We talk about the extent of source code in which a declaration can be referenced. The scope attaches to the declaration. Not a concept of braces make a scope (not as first class as might want). Raises questions rather then resolves then.
* KG: Trying to read and figure out what is and isn’t allowed and phrase in terms folks would understand. If the answer is more complicated then I understand then I can’t explain it.
* DN: By deleting the paragraph what did that paragraph do in the first place. Finding another way to do that, happy to iterate.
* DS: Don’t understand why what KG wrote is wrong
* KG: Two ways, internal body to loop is like an anonymous braced scope and we’re saying that isn’t the case. Things in the ()’s are effectively inside the scope of the for.
* DN: Re-reading text, think it’s fine and Myles is incorrect thinking it allows more. There are scopes and they can overlap. The overlapping is fine, what is bound is resolution.
* KG: Pointed out we have examples below, might be helpful if we can’t link line to inject inline to show what we mean.
* DN: Will drop PR and we’ll make it better.


### [remove keywords that are context-dependent names #3166](https://github.com/gpuweb/gpuweb/issues/3166)



* DN: It’s a TODO and there is a PR to remove type names
* DS: LGTM
* KG: So, could say let align = 2; and that’s OK?
* DN: Yes. let flat = 4; etc.
* KG: const const = 3?
* DN: No, const is a keyword. Auto is still reserved.
* KG: Sounds right. Will see final list when PR goes up.


### [wgsl: trunc behaviour is not specified for infinity #3146](https://github.com/gpuweb/gpuweb/issues/3146)



* DN: Part of larger “what is infinity”? Looked at ieee794 and …. No comments. Call for review, what should be done here? Seems silly to make trunc(inf) == max&lt;float> seems jaring and wrong. Would rather not do that. Third party filed about range issues and domain issues on builtins. Going to go catalog those.
* KG: Hopefully can just do what ieee does. Maybe you get inf.
* DN: Would like world where we have inf and we support them.
* KG: Do we remember what long pole was?
* AB: Vulkan
* KG: Detectable?
* AB: Tried to introduce controls but not on instructions. If turn on only get subset of things. Need to go further to provide for GLSL extended instructions.
* KG: Would be nice eventually but tolerable for now.
* KG: DN will write proposal.


### [Rename staticAssert to static_assert #3232](https://github.com/gpuweb/gpuweb/pull/3232)



* JB: Thumbs up
* KG: +1
* DN: Will merge \



### [Allow expressions in attribute parameters #3233](https://github.com/gpuweb/gpuweb/pull/3233)



* Allows const-expression if attribute previously said literal (align, binding, group, id, location, size)
* Allows override-expression (workgroup_size) if override were also allowed
* KG: Had consensus I believe
* AB: This is making it work with everything else we’ve done.
* KG: Cool. Think we’re all in favour. Let’s take it.


---


# ⚖️ Discussions


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* Previously
    * JB: Let’s talk, I want time to think on this. Can we come back next week? (yes)
* JB: Lots of discussion in bug and meeting notes. Tried to review and summarize.
    * HLSL clearly specifies the semantics of `inout` parameters, no wiggle room or variation. Copies in at beginning and out at end.
    * Languages with pointers always explain their semantics by saying what assigning to *p means, and what using *p as a value means: Do the load and store at the point of evaluaton. If you have pointers in your language, you are saying you do not do what HLSL does. If we felt OK having HLSL behavior, then we should not have pointers - bad fit.
    * So the spec needs to exclude, somehow, the HLSL-like behavior. Spec language now requires implementations to give proper pointer-like behavior within the context of a single function (in HLSL, this means duplicating expressions). The case you can’t handle with expression duplication is pointers passed as params. The spec currently has careful phrasing (§8.4.1) which seems to exactly identify the cases where an HLSL translation based on `inout` parameters would behave differently from pointers, and calls all of those cases “dynamic errors”. It carves out exactly what we can’t implement in HLSL and declares it erroneous.
    * Looks to be exact. If we’re going to take the hit of letting something be a dynamic error, then we should at least get an exact analysis in exchange - and it looks like the present language achieves that.
    * In that sense, language is good. In particular cases MM brought up about “hope this is well defined or portable”, they seem to be so. And the cases he said we can’t do seem to be spec’d as errors.
    * Our main reservation about all this: calling something a dynamic error is very close to telling the user it’s their problem. In practice folks will observe this behaviour difference between windows and other platforms. By calling it a dynamic error, we’re basically saying, “Welp, we told you so”. Don’t feel like that’s … I understand why we make the compromise but it doesn’t feel great. It’s not helpful to tell users they shouldn’t have done something after they’ve had to debug it.
    * We can invest in diagnostics and warnings and give advice but it just doesn’t sit well. Feels uncomfortable being a dynamic error.
    * Understand there are cases Google wants to support. Rationale of current language is that folks can do things they know are correct (and reasonable). Example given previously: array of 2 elements, using 2 pointers into the array as front/back buffer. Static analysis would say these pointers are aliased, but it’s wrong. Likely static analysis would forbid even though it is correct. Any static analysis will forbid correct code. The current language is written this way to allow all correct code.
    * Feel that pointers are a new gift we’re providing. Folks in GLSL and HLSL don’t have pointers, we’re providing pointers that do stuff. Not too bad if they don't do everything. If they have limitations. We already have restrictions to ensure they don’t dangle - those are static and conservative. The user may know in practice that the pointers won’t dangle, but we’ll forbid the code anyway. Already imposing these restrictions. WGSL is already more flexible than other languages. Doesn’t seem like such an imposition to statically forbid correct and useful programs, as opposed to the more exact dynamic error definition.
    * Think this should be static error. Most useful for developers. Can still provide many useful use cases. Still more flexible than other languages. Static errors will guarantee portable behavior. Making static, if in the future we get better backends which can do full unrestricted pointers, this leaves room for that. 
    * Seems a lot of use cases that we’re concerned about permitting via the dynamic definition have alternative implementations that a static restriction could permit:
        * Could do buffer selection in draw call.
        * Could pass array indices around instead of pointers. When doing pointers can always do array and indexes instead.
    * Not concerned about someone saying that these static restrictions are too strict: we can ask what they did in GLSL or HLSL, which doesn’t have the pointers at all.
    * If we go with static rules, I don’t think folks will say our pointers are useless, just that they aren’t quite as nice as they’d like.
* TR: Wanted to point out in HLSL that i’ve seen that violates copy in/out and relies on that violation. Happens all the time. Group shared arrays and passing into function having function index based on thread id and modify/do something with array so each function instance from each thread is modifying one element. The strict definition it’s a giant collision on copy out of modified value and 1 thread would clobber with values already and value change. In practice inline everything and alias the array, don’t copy, and modifies original array. Seems to work. Scariest thing. People rely on this behaviour. Case you’re talking about with pointer to array. Wouldn't’ want to rely on this in the future good to make better. Have ref or pointer type passed through that doesn’t copy. Currently violates in HLSL and folks rely on this. Might fall into this dynamic error case. But, might work and folks might rely on it.
* AB: Thank you for reading spec. To TR’s point, we do allow pointer to workgroup. Regardless on aliasing, think we’ll claim that works. Separate from thai aliasing discussion. Aliasing is around multiple references and that would fail on single ref. So, related but occurs even if we turn aliasing into a static error.
* DS: Should we file a separate issue?
* AB: Would like to hear more from TR. But letter of law it wouldn't work but would buy everyone relies on this and they probably do more then we think. Would mean you can’t have pointer to workgroup anymore. Just function and private and we just have short names.
* JB: In the process of reading the spec, I found myself wondering, if we’re imposing all these restrictions, maybe we should just have inout and remove pointers. Taking away so much …. Understand it’s water under the bridge
* AB: Think worth mentioning, if we emit DXIL it isn't’ an issue, this is HLSL, in future can have real pointers.
* TR: Absolutely.
* DN: Regardless of HLSL evolution, OK with we’re heading to world with pointers. Ok to rely on not quite true statement as long as tests pass and no-one goes and backports restrictions into prior targets then am happy to rely on defacto behaviour. In terms of re-writing, we and partners have long chains of src A -> WGSL. Don’t want change that has to ripple back. Need to much insight to do re-write JB is talking about. Strong motivator to say dynamic error is OK.
* JB: So, saying making static error would make WGSL less useful as an intermediate stage in translation. What would be upstream where that is an issue?
* DN: Originates in inout -> spirv -> wgsl.
* KG: Can’t tell it’s inout, so don’t know transform that happened?
* DN: GLSLang of inout makes copy in caller, copies in as unaliased then copy in/out in caller. Impedance mismatch on one level and we take away on another level. Don’t want to add turbulence
* JB: Sympathetic to that. Think that needs to be a lower priority argument then serving folks writing WGSL directly. Also, it’s a vague requirement, since we don’t know exactly what sorts of things GLSLang can produce. If we had specific examples, then I feel like we could accommodate those in the static rules. Can always make fancier and more permissive without crossing the line and permitting behavior that HLSL can’t implement. Seems we could keep those translation chains alive in practice if not in principle.
* TR: Am i getting right that case where passing group shared is part of the well defined behavior?
* AB: No, we’d allow it
* TR: Want to point out folks relying on this find out because sometimes it doesn't work. Aliasing of array relies on simple analysis that only modifying an element or something so we can alias this, not doing multiple things (modifying both pointers in same scope and copy in/out) then it has to do copy in/out so can’t eliminate. Doesn’t know anything about thread share vs local. Can eliminate it based on other pointer not changing, that happens in FXC. DXC doesn’t have as good aliasing and arrays analysis, a lot of FXC code that isn’t in DXC. Sometimes don’t eliminate copies and would be major source of problem. Relying on behaviour is bad and we don’t want to encourage.
* JB: So, saying, whether arrays are copied in/out in entirely, or just 1 element depends on opportunistic compiler analysis that may take place?
* TR: Yes. Iimagine copying in/out and thinking arrays same kind of thin, not group shared vs ???. Then optimizer says not coping in/out then can do alias. Original could be modified and it doesn’t know then it can’t eliminate.
* KG: Accurate to describe as inaccurate copy ellison?
* TR: Yes. It isn't’ the same kind of thing. It doesn’t understand the fact the arrays are different, group shared is different from thread local. It’s not accurate.
* DS: TR can you file an issue?
* TR: Yup.
* JB: It sounds like the cases that the spec is calling dynamic errors cover all these controversial situations - is that correct? We forbid the cases where you could tell in/out vs true pointers. Only a code gen issue in cases that’s visible. So, have we managed to skate around the problem?
* AB: Question is if you can fail to eliminate the copy in/out if there is more then  1 ptr. If you can fail to eliminate with 1 parameter then issue no matter what
* TR: Can access global and param at same time?
* AB: Disallowed.
* TR: Don’t know about multiple params. Not common case where pass same array multiple times.
* AB: That we’ve explicitly disallowed. in/out of array that is group shared and DXC doesn’t compile then only option is to eliminate ptr workgroup as function parameter
* TR: Can’t guarantee copy is eliminated. Would like to say so, but not confident as it’s optimization it’s relying on. Could be some cases where it doesn’t know it can eliminate.
* KG: Scary part sounds like “what would you do in HLSL”, “I’d do this”, “you know HLSL doesn’t allow that”, “yes but works” ….
* AB: Even if dxc fails, driver may optimize later.
* TR: Possibly not, as it’s an illegal optimization. If we leave a copy in/out of thread local then behavior is not the same. If they optimize that away…. Maybe just doing because they know the pattern. Seen on internal code and told don’t do that. Lang says copy into thread local into array and back out at end so please don’t do that.
* JB: Think that’s similar to concern about it being dynamic. Folks will just do it and will count on it and we’ll have portability problems. Concerned about that cost. Appreciate DNs explanation of more cases where we need to have flexibility of dynamic rule. Would committee be interested in PR making static error and would look at GLSLang output and see if that gets carried through?
* AB: Would want to consider trade off between conservative whole program analysis vs what we’re gaining from having ptr params.
* JB: Ptrs for atomics
* AB: There in globals. Losing some refactorability, but still accessible. That would be the loss, can’t have function which calls atomic on bunch of different variables that my have atomic
* JB: So, the static analyses you think are feasible to implement would all be so restrictive that pointer params aren’t especially useful?
* AB: Given this plus what TR said, it depends. If we remove ptr workgroup and we just have function and private, that doesn’t really give useful functionality. This is useful for large storage, why not using return for function. Maybe it’s philosophical. If take away workgroup, don’t know if there is enough left with pointers and that’s what we should consider. Just banning or having analysis, what’s left in carveout and is it useful.
* JB: Don’t understand workgroup concerns as don’t know D3D to WGSL mapping
* AB: WorkgroupShared -> Workgroup
* JB: So groupshared is WGSL workgroup
* DN: Fine making one set of rules for builtins and different set for user defined functions. That’s where array length and things are fine to be different and atomics are fine to be different. Question on if you lose enough functionality about passing ptr to workgroup into helper.
* JB: Don’t understand how restriction arrives
* DN: Have use defined function acting on atomic passed as ptr then banning ptrs to use functionalities you lose that functionality. Might be concerning. GLSLang is probably the easy case, often in the middle of a long chain of things. Other compilation flows like CLSPV, less important but still useful. Need to talk internally
* JB: My position: what could we do to make this static. Think the user experience will be very different and concerned about non-portable behaviour.
* KG: Think more, and regroup. Also need Apple in the room.
* JB: MM has some ideas and concerns on this as well.


### [wgsl: language evolution: managing rollout of core (essentially sugar) language features #3149](https://github.com/gpuweb/gpuweb/issues/3149)



* KG: Want MM for this, so we table. Next week is APAC.


---


# 📆 Next Meeting Agenda



* Next week: **APAC!** Tuesday, August 02, **5-6pm** (America/Los_Angeles)
* [#3236](https://github.com/gpuweb/gpuweb/issues/3236) disallow repeated use of builtin inputs and builtin outputs (within the same shader stage)
* [#3218](https://github.com/gpuweb/gpuweb/issues/3218) parsing of strange float cases, and [#3255](https://github.com/gpuweb/gpuweb/issues/3255) add limits to avoid nuisance floating point literals  
* [#3153](https://github.com/gpuweb/gpuweb/issues/3153) Clarification of loop identifier lifetime
    * Now has PR: [https://github.com/gpuweb/gpuweb/pull/3267](https://github.com/gpuweb/gpuweb/pull/3267) 
* 
* [#3253](https://github.com/gpuweb/gpuweb/issues/3253)  invalid expressions in const-expr and override-expr should be error (no later than pipeline creation time)
* [#3266](https://github.com/gpuweb/gpuweb/issues/3266) wgsl: permit certain resource variables to overlap

FYI notable merge



* Uniformity analysis now assumes function return is a reconvergence point
    * [https://github.com/gpuweb/gpuweb/pull/3254](https://github.com/gpuweb/gpuweb/pull/3254) 
* 