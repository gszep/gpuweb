# WGSL 2023-01-17 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** ds

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** (non--APAC) Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-01-10 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1wKOVNUJKKpZ1U-9_JaALnfkgdIq_pBrF9gJy5hPox2M)  

 

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
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * **Erich Gubler**
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



* F2F
    * F2F at Apple Park, Feb 16-17.
    * See email “ACTION NEEDED: Please RSVP to WebGPU/WGSL F2F”
        * Please RSVP by end of this week, Jan 20th at 5pm.
        * **If you do not RSVP, and so aren’t on the list, you *cannot* get in to the venue.**
    * There will be videoconferencing (WebEx) for people who wish to attend remotely.
    * MM: I’m setting up a group dinner on Feb16. Send me preferences or ideas for restaurants!
* **text/wgsl has been registered as a media type at IANA.**


---


# ⏳ Timeboxes (until XX:25)


### [Designing a uniformity opt-out #3554](https://github.com/gpuweb/gpuweb/issues/3554)



* [https://github.com/gpuweb/gpuweb/pull/3713](https://github.com/gpuweb/gpuweb/pull/3713)
* FROM LAST WEEK:
    * JB: I would like another week to better understand the language used here.
* Anything come from that?
* Whats left:
    * Better explanation of the triggering location. (DN)
* JB: Commented in PR. Overall looked fine. Concerned about tags assigned to function and params when diagnostic is filtered. Only real substantive question..
* DN: When tried to write with explicit tracking, got wrong. **Need to address with new text to compute the right things**. Good, narrowed down to that
* JB: Sounds like just waiting for new text
* JP: So, just editorial? Good to land?
* JB: Pretty substantial, the algorithm is(?) wrong.


### [wgsl: vec2 shouldn't be a keyword; disambiguate parsing of type construction expr vec2&lt;i32>(0,1) #3739](https://github.com/gpuweb/gpuweb/issues/3739) 



* Ambiguity parsing: `A ( B &lt; C, D > ( E ) )`
    * `A ( B &lt; C, D > ( E ) )`
    * `A ( B &lt; C, D > ( E ) )`
* DN: Spawned lots of internal discussion. Trying to be respectful of future. Example from BC, if we have generic templates, and have 3 things (oo declarations, shadowing and user templates) get into corner. Looking for options. Provided 3 which change syntax, all kind of distasteful in one way or another. BC has proposal from an hour ago [https://github.com/gpuweb/gpuweb/issues/3746](https://github.com/gpuweb/gpuweb/issues/3746)  on how to use algorithm to disambiguate to favour the template interpretation. Was just [playing with toy grammar to look at this](https://github.com/gpuweb/gpuweb/issues/3746#issuecomment-1385897818) .Current state of mind, think we should think about this, or we don't panic or change and defer. Hard problem is anticipating user declared templates, but we've also said use JS in the past. Not resolved within Google but want to raise and cause a dustup as now is the best time to do it.
* KG: Agreed. We should figure this out.
* JB: At moz we were friendly to the suggestion that WGSL follow D syntax. Haven't had a chance to look at proposed heuristics. Doesn't sound good to me that we can't clearly tell users how to write one or the other. Think having heuristics which go above and beyond sounds like ti would create a delicate situation. Understand it appears to work fine on existing corpus. Don't understand it yet. I think that if we take as a given that we must have c++ constructor syntax then that's just a stringent requirement. It doesn't make a lot of sense to me. The world isn't made of c++ or this one thing. The fact is, things do not explode when you make a minor variation in the notation. Don't want to be boxed out of considering new syntax. We already know the consequences and we are talking because they are unpalatable. If we choose new syntax we can use the same for both declarations and constructors as it's a service to users to have a single syntax. It is difficult to do that if we accept the straight jacket of c++ syntax. Feel like the folks who are saying we must use c++ syntax haven't' given much room to negotiation. When say what want, say why like it, not just my way or the highway.  Can folks giving objections provide the conditions and what they're open to negotiations on.
* KG: Thanks, that's valuable.
* BC: Internally contemplated the various proposed syntaxes. `!&lt;` that seems more palatable with certain people as it's more c++ like. Does suffer from some ambiguity on the terminator. Can ! the start &lt; but  but still suffer from &lt; boolean. Can do nicely with D syntax as it handles the nesting, good for parser simplification, folks don't like it. Looks weird. Major complaints, if you have a template list followed by user function. Type like array&lt;f32, 2> if used syntax `array!(f32, 2)(2, 2)` that `)(` seems to be what upsets people. Counter to that for single param you can remove the parens like `bitcast!f32(2)` For folks with c background, looks like a c function pointer. Criticism is taking features from many languages and have c syntax, and rust syntax and now adding d syntax. Folks raising concerns is that our general population of shader writers will be up in arms about it being a late change (Google was trying to ship this year). Changing types now would upset folks. The primary contender for change folks aren't keen on. If it parse easily or not, folks like c++ template signature. As for the don't know if it will parse, the way written is possibly not the most elegant but is fully deterministic. No magic determination based on identifiers, pure parse based on tokens. Tried to lookup c# which has same problem, and think what I’m proposing is the same as they have. Written c# for many years and never had it go the wrong way. Ran through 50k WGSL shaders and 2 went the wrong way in pathological cases doing bad things with for loops. Moderately confident the heuristic parse wouldn't be a major foot-gun and easy to guide users into fixing. Fix is to add parens and would have had weird parens anyway. Already require parens for binary ops and associativity.
* JB: If it is true that this is what c# has done, that is encouraging. If there is something we can take off the shelf that has burn time on someone else's dime that changes my feelings a lot. **We need to take some time to read the proposal**. Someone else can understand the c# approach and maybe that's the way forward. 
* BC: Not absolutely certain it matches c# perfectly. Tried to find other languages that solve this and took inspiration from c#.


### [Lookahead disambiguation of less-than vs template argument list #3746](https://github.com/gpuweb/gpuweb/issues/3746)



* DN: Same as above.
* KG: Group in same issue?
* DN: Yup. #3739


### [wgsl: stop using "type" to create type aliases. #3738](https://github.com/gpuweb/gpuweb/issues/3738) 



* DN: Request to rename `type` from type aliases to `alias` if we're anticipating overloads, templated or not, may want a way to create  new type. AB says just use struct for that. I think alias is slightly nicer.
* KG: I like alias or using
* DN: Either would work.
* KG: Sounds reasonable. 
* JB: Alias is nice and explicit. 
* BC: Using in c++ is overloaded with namespaces.
* MM: What might we awnt to use type for in the future?
* DN: To create a new distinct type for type checking, overload resolution, that otherwise is the same as the original type, differ by name.
* MM: Is there an example in c++ or another language?
* DN: In c type matching is by structure, in c++ it's a breaking change where it's by name. Velocity and Position are both Vec3 and don't want to call function expecting Velocity with Position.
* BC: go has `type` and `type =` and one is an alias and ones a new type. Could use an f32 but can't implicity use it as the wrong thign.
* KG: Not just primitive types but any type. All that aside if making type alias and condensing to one word, alias is the better word then type. Type is ambiguous and not certain which you mean and alias is clearer. Think calling it alias is the way to go
* MM: typedef? I guess we dont' want that because the order is flipped?
* KG: Still confusing which one you want. Alias vs type is the core thing. These are aliases and they can be used interchangeable. vec2f is interchangeable with vec2&lt;f32>.
* MM: The thing we might want to use type for in the future is better then struct.
* KG: **Resolved, switch to alias**.


### [Shader compilation is fragile because FXC #3691](https://github.com/gpuweb/gpuweb/issues/3691) 



* KG: No milestone
* AB: Not sure if we want to do anything. We have to live with some FXC issues and categorize as spurious failures
* BC: Part of the implementation issues with FXC
* MM: What it comes down to, if there is a way to create a category of failures tha FXC fails on we can ban that category. Can't change WGSL spec so Vello’s coarse.wgsl shader succeeds. This is one of the uncategorized failures
* KG: Fits in to the existing set of uncategorized. Think general problem is that we're taking on that danger/risk/work as implementations and the individual parts that don't work well we'll talk about. We're promising that we'll make it work on fxc knowing we'll fail a little bit. Materially we're saying we'll make it work.
* RC: Summary for this issue? Didn't follow what exactly is the issue, is FXC correct or is it in the wrong?
* MM: Raph claims the generated HLSL is valid.
* RC: Has that been verified by a 3rd party?
* KG: Unknown
* JP: Sort of, we did workgroupUniformLoad in Chrome, Raph tried and it did resolve the issue. The WGSL compile would reject due to uniformity first, using workgroupUniformLoad we can validate that FXC does not reject for this shader.
* RC: So there is a workaround?
* JP For this specific shader. Raph says he expects there to be other cases where that is not true.
* RC: **TR and GR said they'd take a brief look and see**, weren't sure if this should be an issue in the first place but haven't looked in detail.
* MM: Is workgroupUniformLoad part of the spec now?
* JP Yes
* MM: Cool.


### [Concretization of abstract expressions can produce surprising types #3716](https://github.com/gpuweb/gpuweb/issues/3716) 



* (tabled)


### [Is it intended that keywords can not be identifiers? #3692](https://github.com/gpuweb/gpuweb/issues/3692) 



* (tabled)


### [wgsl: rename static_assert to const_assert #3737](https://github.com/gpuweb/gpuweb/issues/3737) 



* KG: Yes/no
* DN: Yes
* JB: Yes
* **KG: Anyone else? Sold.**


---


# ⚖️ Discussions


### [Accessing an "invalid memory reference": Trapping behavior · Issue #3272](https://github.com/gpuweb/gpuweb/issues/3272)



* DN: Major concern, need text to address uniformity analysis. The two invocations in same quad, one traps due to array ref, now uniformity needs to understand that. Think it will be a large challenge and expect unpalatable
* MM: Proposing no effect on uniformity
* KG: So, anything that might, but spec trap, can either continue or discard to helper?
* MM: Right. If the compiler thinks its too hard to maintain the contract to not change uniformity then the compiler should not trap.
* KG: Great, still working on concrete proposal?
* MM: I guess it depends on what DN wants. PR? English in a doc?
* DN: I don't think this is well described enough to understand how complicated it will be. Still skeptical of it just discards. That has other behaviours which effect uniformity. Lots of hidden things so want to see more concrete.
* **MM: How about if i write a bunch of examples of input programs and how they would/would not trap in the generated code. Would that help? A few different interesting cases. Describe what the compiler would produce.**
* DN: That would be more then we have now.
* **MM: Ok, will start with that then.**
* DN: This is not a desugaring, right? Can't just discard, would be super return
* MM: Yes.
* DN: And super return, means no further side effects?
* MM: Envisioning fi the compiler proves all invocations in quad trap, then would be super return and quad killed. Otherwise the compiler would have to implement its own discard. If the system discard is fine could use it, otherwise predicate future IO.
* DN: Right, and in compute shader if trap before barrier, then potentially not in uniform control flow and program is invalid. Broad strokes need solutions for compute as well.
* MM: Will start with an example of what you just described.
* AB: If you elect to trap where otherwise had data race, is program required to be correct?
* MM: What does correct mean?
* AB: Didn't execute the data race, and that's the only undefined behaviour. So, an implementation with trap could remove the race and another implementation does, whatever?
* MM: So, 2 threads write to same index of buffer and that index is out of bounds?
* AB: No, thread 1 has multiple access to memory. THe later one is data race with other invocation but earlier access traps, so second access doesn't happen. Don't know what observable effects there are, just curious
* MM: Not understanding the question
* KG: Orthogonal. When we say you can trap and there are no more side effects. If that was true and there was a side effect to have race then it wouldn't be invalid as the race doesn't happen.
* MM: If the compiler decided to trap
* KG: Yes.


### Language evolution [https://github.com/gpuweb/gpuweb/issues/3149](https://github.com/gpuweb/gpuweb/issues/3149)  



* DN: Read comment from MM last week, thank you for stating clearly, haven't discussed internally due to other things
* MM: Defer or just in?
* KG: I think we're colessing here, I think some of the concerns some of us have are things we want to make sure are handled but aren't deal breaker problems.
* MM: Yea, also think the search space is pretty open and I bet there is some solution to keep everyone happy. Just haven't found that middle spot yet.
* DN: To  pull out one pice, don't want to maintain N versions for future sets. I agree with that, and my picture may not be correct, but there is a window of versions that the compiler may support and that window maybe as narrow as you like. Doesn’t have to go back to beginning of time. So, company A starts in 5 years time and only builds from that point. Think we want to allow narrow windows. Have not gone back to revisit. I agree you don't' want to, forever, maintain compatibility or the fact you'll reject old programs. It's ok to accept more then the min.
* MM: Assume narrow window is 4,5,6 and program comes along and says I"m v2. that's the interesting case.
* DN: Because requesting to not use anything after v2?
* MM: If asked for v2, what does that mean? What actions do we take? Would be a shame if the src identified itself as v2, and browser doesn't' support so reject.
* DS: Part of that is keeping backwards compat, a v6 compiler still compiles v2.
* MM: If we keep backwards compat indefinitely, why should the source even declare itself? Either you support or not?
* JB: Helps developers know when they overstepped their portability rules
* MM: Can be offline
* JB: It's a lint
* BC: What MM was saying was one of Googles original proposals. Add features and things keep compiling and we make sure things keep compiling. Don't want to support N different versions/feature sets. Don't want permutation hell. Having monotonic forward language and we agree what is in each monotonic increase is a nice way to proceed. The whole querying about version was folks concern about overstepping. Write program and accidentally use feature in newer version you didn't' expect, test browser and accidentally break things for a lagging browser. The whole I want to know what the highest water mark feature of my shader was to try to prevent that. Maybe linting would solve that. If there was an offline tool.
* JB: Moz hasn't talked about this, personally don't' have a strong feeling if this needs to be in browser or linter. Linter seems OK to me. (In response to why folks want this, wasn't saying the browser should do it).
* MM: Sounds like folks haven't thought very hard, maybe can't do full resolution, maybe can get consensus that feature dev of WGSL can be backwards compatible?
* KG: Goal is backwards compat. My feeling is eventually if we do feel it's valuable enough we cut a new version. But, going forward we're backwards compat.
* MM: Cool, benefit for a lot of folks. Thumbs up
* KG: That's the direction of consensus I think.
* JB: We know one thing we need is this user shadowing stuff that we've been working on. That's the only think I can think of right now. Are there other pieces of the spec that need spec language to make this rolling strategy work out? Or is just roll forward
* DS: That's the template thing
* AB: Thats a choice, we don't' have to have that feature
* MM: We should have the ability to have the feature.
* AB: We've said JS is the preprocessor, so we don't need to build future supports for it. Do we need template because we have JS? \
KG: Different shades on the gradient
* MM: Question on hand is, should we make a small change now so in the future we could have the opportunity for templates and generics
* KG: Overall on the same page.
* JB: Other spec work that's necessary?
* BC: Similar to the keywords, the enumerators on types is something we're having on types. They aren't reserved but how we resolve these is active discussion. If we have a parser generic thing that supports expressions and these enumerators do they incur shadowing as well?
* KG: What's an enumerator
* DN: Context dependant name `centroid` `rgba8unorm`. String used in specific spots, but isn't an identifier or keyword. If generalize we need to be able to use these declared, but not keyword name things.
* JB: We will not be able to add reserved words going forward. When are we making things work where we lean on reserved words?
* KG: Not to fill in the gaps too much it's important colour that we don't' have to positively never add keywords, there is the opportunity, like the array flattening stuff in JS ran into where they figured out a way forward where worried about splat. A question about balancing the forward compat of old websites.
* DS: Enables could enable new keywords
* BC: Getting into permutations of a naturally progressing language avoids, but that's one way to do it.
* (tabled)


---


# 📆 Next Meeting Agenda Requests



* Next week: (non-APAC) Tuesday, January 24, **11a-noon** (America/Los_Angeles)
* 