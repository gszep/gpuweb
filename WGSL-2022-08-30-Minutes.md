# WGSL 2022-08-30 Minutes

**🪑 Chair:** dan sinclair

**⌨️🙏 Scribes:** ds

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-08-23 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1JIIwP4p6DIDA6aEd53k9VXzOzFic0ZSoMpVw-_1EOCU) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Daniel Glastonbury
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
    * Tex Riddell
* Mozilla
    * **Jim Blandy**
    * Kelsey Gilbert
* Connecting Matrix
    * Muhammad Abeer
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
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



* Previews now deploy to GitHub instead of another external service! (Oguz)
    * [PR #3384](https://github.com/gpuweb/gpuweb/pull/3384)
    * [D8fa793](https://github.com/gpuweb/gpuweb/commit/d8fa79345e5902de19590f34c9171d137e44842d)

**Notes**, TPAC in 2 weeks, we are not meeting face-to-face at TPAC, we will meet online as usual.


---


# ⏳ Timeboxes (until 11:15)


### [Clarify meaning of context in integer example in TYPE INFERENCE FOR LITERALS #2970](https://github.com/gpuweb/gpuweb/issues/2970) 



* David: **Yes, this is editorial**.
* DN: Forgot marked as not-editorial, re-marked as editorial. 


### [wgsl: Other language types in reserved word list · Issue #2983](https://github.com/gpuweb/gpuweb/issues/2983)



* Dan closed the issue


### [Numeric coercion leads to surprising results with division #3326](https://github.com/gpuweb/gpuweb/issues/3326) 



* JB: Easiest way to understand is read first paragram to get example. If is surprising is matter of taste. Don’t actually have any changes just wanted to make sure it was what everyone understood. I don’t think it’s ideal as folks often write vec2(1, 2) and will be comfortable with that making float values in some contexts. Think some folks will be surprised but we’re committed to the approach we’ve taken and if we’re all OK we can close
* MM: To understand, the surprising part is ½ does not give a half. Is there any existing shading language where ½ gives .5?
* JB: Don’t know. GLSL would forbid this as you need the . somewhere
* MM: Would be an error
* JB: Yea.
* MM: Integer division doesn’t work?
* JB: It works but wouldn’t  ….
* MM: Looking for difference between compiler error and error where result is 0
* JB: Seem to remember GLSL requiring . everywhere, so don’t know what would happen. Maybe need ivec2 or something.
* **DS: So, resolution close working as intended (or won’t fix)**
* **JB: Will handle.**


---


# ⚖️ Discussions


### [invalid expressions in const-expr and override-expr should be error (no later than pipeline creation time) #3253](https://github.com/gpuweb/gpuweb/issues/3253)



* DS: Which half is this
* MM: Split into runtime vs compile time and we want to runtime behaviour first. This is compile time and we should talk about the other first.

#[3365](https://github.com/gpuweb/gpuweb/issues/3365) Runtime behavior of NaN-producing math functions



* DS: Roughly Apple thinks we should define domains of things and all results get defined
* MM: More subtle then that, but yes
* DN: We’ve defined some of them but not all.
* MM: First thing interesting to discuss is historically viewed as portability vs performance trade off. If that is acceptable then would be reasonable to talk about classes of distinct functions. 1 might be inverse trig as they have particular perf profile which is different from others. One way to nibble away is to first scope to just those functions and say is it reasonable to make well defined for -inf to inf just for these because it’s cheap. One option is make them well defined and pick some value they should be equal too or, don’t pick a number because it’s wrong and programs which produce should be ill-defined and produce …. Something. What do people think?
* JB: Not in favour of leaving things as undefined as a way to punish authors. What I do want to discourage is people using specified behaviour as a way to do tricks. The reason nan is first choice is that it is the most useless value that maximally discourages folks from doing tricks. By this argument, maybe having arcsin(2) == inf would be a feasible way forward because it’s a difficult to work with value that is kind of infectious and probably messes with things. Would be a way to give fully spec’d behaviour and discourage tricks. Looking ahead to compile-time case and my goal is to discourage from mis-using and help catch errors would like to do something that graduates well to making pipeline and shader module creation errors. Nan is appealing as it’s the cleanest graduation to the errors I want at static points. Inf is an annoying value so, arguably cleaner transition. Making zero is well specified which is better then just being anything.  Having it being any impl chosen value is least favourite choice.
* AB: What is punishment. Say we had nans, if i had 2 nans does nan== nan? Or is comparison unordered? What is the value of picking one value if, when we switch to nan the value isn’t comparable.
* MM: When opting into extension you opt into different flavor of math
* AB: JBs philosophy is to get as close to nan as  possible. Undefined value is the closest we can get with how comparison works. If nan != nan then having undefined value where don’t know where comparison will go is similar. In my mind, is the punishment you get with an undefined value the same as nan?
* MM: Little different, on dev machine will probably get consistent result but on my machine it won’t
* JB: Regret introducing idea of punishment. Want to be helpful and help authors catch errors and part of that is declaring things off limits. Claim is rather than Javascript approach of giving everything a value and users use in weird ways we should do what we can to help folks discover mistakes. Nan is one way and think that was the intention. Undef value doesn’t help.
* DN: This is why I like talking about compile-time constexpr rule first. More capability to provide feedback early that something ahas been done wrong. Having distinct runtime vs compiletime behaviour is a feature as these are faulty programs. Like framing to be as helpful as we practically can be. In compile time side make it a static error (however we want to say) . We should be as close to that at runtime as we can while restricting practical limits. So, would advocate a number like -716.3 is better then 0 as you won’t index array with that. 0 is worst answer as it slides past too easy. Nan is best, inf is good, large transcendental number is better then zero
* MM: Think reasonable compromise. The reason arguing is because of portability and if we make compile errors for places we know the compiler can report an error then that is portable. The second order goal was to enable the slippery behaviour but group is rejecting that so think it’s reasonable. So restating. For runtime behaviour, pick a value, inf or -713.7 and for compile time we make it a compile error.
* DN: Like that better, so will take it.
* JB: Nice thing about inf is it’s infectious so work to warn of problems.
* BC: Don’t know how to do inf without broken compilers being an issue. If have to guard arith with polyfills to produce portable behaviour and start injecting inf or  nan into program and those backends cannot cope then might end up back in situation where we have compiler bugs coming out. 
* MM: Question for vulkan folks. Is inf support greater than nans? If same then no use of one over the other.
* AB: They are the same.
* BC: Not tied specifically to vulkan.
* JB: That’s disappointing.
* DS: Could it be inf or -713.24 or is that just worse?
* MM: FOr portability it’s worse
* BC: Know DN said 0 is worst but have high confidence 0 would work.
* DN: Have not’ consulted on what’s the worst possible worse
* MM: No opinions on which value, maybe take week to pick a value
* BC: Float max would be as good as zero
* DN: Negative float max.
* MM: Folllowup, was talking about breaking into equivalence classes and talked about trig functions. Is this path forward for all or just trig? Which functions should it apply too. Could for float division and denominator zero.
* DN: Was going to raise division. Vulkan is only defined for normal number denominator. So other cases were not important and not tested (inf ULP error). The domain is not so crisp.
* MM: Zero is non-normal?
* DN: Yes. And don’t want rule for x = 0 blah and then for things that are close to 0 do foo
* MM: Right so what we do for 0 is what we have to do for close to zero
* DN: It’s a fuzzy boundary.
* JB: Distracted by division, original question is which functions. In my mind, compromise for unusual value was all functions with limited domains
* MM: Try to enumerate
    * Lot that are totally defined
    * Another set defined except by 1 value (division for example)
    * Bunch defined for half of domain (log)
    * Only defined for narrow band (acos)
* JB: Think we should treat all of them with restricted domain the same, so log, acos, sqrt should all …
* AB: Expect sqrt and log to be fast
* JB: sqrt is tricky as it’s heavily used.
* MM: Because of colour space transformations
* JB: And distance calculations.
* DN: Reminded that OpenCL has handful of functions which are cos and native_cos where native is fast and has larger error bounds. That kind of escape hatch is explicit opt in without an enable. Not advocating but pointing out exists.
* MM: Have opinions, should talk about when we get to non-fast path proposal. So, proposal made, similar to JB proposal is, we have behaviour for all functions which are not defined for around half the domain or more. So function defined for everywhere except for sentinel values it’s fine. But for things like sqrt would be defined.
* DN: Why pay for complexity of authors having to remember  which functions have half-infinite domain and others which have a finite size gap in their domain. What’s the motivation for making some special?
* MM: What’s the counter argument?
* DN: Have a rule which says when you try to compute function outside domain have domain error which results in x. Rather than saying half the domain on the real line is one category vs an interval size 2 which is different. Seems like weird corner to get tripped on.
* MM: Concrete function, normalize, defined everywhere but 0, 0, 0. Should normalize(0, 0, 0) be a range error?
* DN: Normalize is funny case where epsilon close you have gigantic errors anywhere. Haven’t thought deeply about that. In some sense, if length is less than x you get some category and this discontinuity to compute category. Argument for I have well defined interval of size 2 vs half line. A single point is nuanced.
* MM: Happy to treat half the range and a single band as the same. No problem.
* DS: Summarize, all happy with compile time errors when we can detect. For runtime, for error bands we define the value return. For single value we …?
* MM: No real opinions, treat as zero division case.
* DN: Division case, closer to zero the closer answer to inf so you blow off to infinity and hit overflow handling. Normalize is always some vector.
* BC: From usability have had to put in normalize(0, 0, 0) = 0 
* MM: Surprising expected normalize(0, 0, 0) = (1, 0, 0)
* BC: From graphics that usually produces speckling with things crossing zero boundary.
* MM: That’s fair.
* DS:** Spec to write for compile time. Spec to write for half or narrow band domains. Table for next time single item ones discussion next time. Discuss sqrt or ones expected to be fast.**
* JB: Agree
* MM: Had decision for division by 0 should decide if we want to be consistent or not with normalize(0, 0, 0)
* DN: ...
* JB: Thought was defined in IEEE but sounds like inifnities don’t work either so not workable plan. 
* DN: Think floating point divide should be able to say inf for those reasons. In spec the abstract float doesn’t have nan or inf. Says a computation that results in inf or nan is compile error.


### [Add a static alias analysis #3336](https://github.com/gpuweb/gpuweb/pull/3336) 



* (previously [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457))
* AB: Writeup of JBs proposed analysis
* JB: Great that I can sketch and AB writes language. Couldn’t find any problems with what is written, a few editorial and clarifications. Otherwise looks fine to me. Looks like it prevents cases between inout and ptr implementation and preserves the future facing options that we are aiming for. I like the language proposed in PR.
* MM: Haven’t read yet. Couple questions though. One is the distinction between globals and parameters. Understand it is possible inside a function if it takes a param by reference so it can name a global and pass in as param. Imagine this analysis makes this a compile error?
* AB: Maintains the distinction that something has to write to be an issue. Majority of work is to compare global to param. Paraa to param is easy at call site.
* MM: Next, if no globals still some analysis to do, don’t think that’s trivial as the caller could have … or maybe it is trivial
* AB: It’s relatively trivial. Trace to the root identifier. Functions can’t return pointers so they’re all based on small identifier expressions. In fact, passing param to function it’s quite restricted in what you can pass in that it has to be address of or param.
* MM: Ok, so it isn't’ possible to write a program where a function gets called a root identifiers for the values passed in alias but also are declared in different functions. That parts cool. Other statement about it’s reasonable to have a struct foo, bar, and baz inside each other. Foo owns bar which owns baz. Reasonable for function to accept foo and baz as params. This analysis would forbid if the foo and baz are passed as aliases. I think that would be a common case. Little unfortunate it would be disallowed. Think it’s important for buffers with nested structs and you want to pass in leaf node and top level struct as sibling params. This would be disallowed which is unfortunate.
* AB: Have a lot of ptr param restrictions already. No storage or uniform buffers, have to also remove workgroup. It’s function and private. Separate restriction on declaration, either another param or address of variable. Can’t subscope the memory locations prior to any of these changes (until we have more powerful pointers)
* MM: Why not send pointer to member of buffer, seems like common use case.
* AB: SPIRV doesn’t allow it.
* MM: How does GLSL compile something? Copyin copyout?
* AB: When you reference part of the buffer it isn’t passed as pointer. Either use global directly or load what’s needed and pass in and hope compiler optimizes as necessary. Similar restrictions exist in HLSL where hoping compiler does fast thing
* MM: Interesting question is HLSL and GLLS allows author to say phrase please pass in member of buffer by reference
* AB: No, not by reference. Can pass in what is equivalent to part similar to WGSL by passing in part of array. HLSL an dGLSL copy in/out is not true reference.
* MM: Think talking past each other. Program is function takes in/out and caller passes a field of a buffer.
* AB: This is what TR brought up as issue in hlsl with shared memory. Inout param with part of shared memory buffer the copying means every invocation is touching everything and the trampling which may or may not occur means you don’t get what you want. Don’t know who is writing the result, whichever invocation is last. Similar thing for something with our storage buffer, every invocation has a race as each invocation is writing all locations even if they didn’t intend. Seems like good thing to disallow.
* JB: Think for TRs concern it would be nice to have concrete example. Have ideas of what we’re discussing but is a bit nebulous
* AB: Imagine array of shared memory and inside function use invocation id to write 1 element of the array. Because in/out every invocation copies in and out. The last invocation clobbers all results.
* MM: Not program I was describing.
    * void foo(inout int x) { … }
    * foo(myArray[thread_id])
* MM: Think this would be common and unfortunate fi WGSL authors can’t describe this 
* AB: Capable of describing it but not with ref param. Use return value and assign back to array read from.
* MM: Right have to do copy in and out themselves. Think it’s unfortunate as wouldn't need to do it in GLSL or HLSL to do copy in and out.
* AB: Don’t know, feels like nitpicking as in function there is an assignment whereas in WGSL you have an assignment. Doesn’t seem onerous to do such a transformation. Would be problematic is when you have to deal with memory locations that would then be affected by every invocation.
* JB: Think if we want to say in/out with ptrs and then give ptrs which can’t do what in/out does is disappointing. Don’t know what to do about that if SPIR-V can’t express as seems pretty fatal. Can see MM’s point about it being less than ideal
* MM: Think proposal here is WGSL -> SPIR-V compiler would do what GLSL -> SPIR-V would do which is emit extra copy instructions to represent program.
* AB: This one example where pre-divided memory where every invocation touches own reason but have to worry about case where someone passes case adn does the thread_id inside the function instead of at the boundary. Do we know everyone is going to write that way? Have feed back from TR where they’ve seen workgroup memory passed as in/out and that’s unreliable depending on underlying hardware. Are we concerned about allowing 1 case and other cases are unportable which would be surprising or do we say restructure code slightly in order to handle scalar case (where you split memory up and reassign it).
* JB: My priorities are portable behaviour comes before making WGSL pointers a perfect substitute for other shader languages’ features, like in/out. Would definitely prefer the behaviour be restricted and portable then more featureful.
* MM: Just to bring up again, the counter argument is we add in/out to WGSL and that would be both portable and featureful.
* AB: But only so portable as it used in just the right way. Doesn’t take a way footgun of clobbering one array value with another and getting unexpected results. It’s defined but not what I want it to be. It’s a gotcha. Do you have the rest of the rules around these things? To me in/out is historical and backwards looking. Think it’s bad idea to keep with language moving forward. To say we need an attribute which is possibly a complicated compiler transform because someone doesn’t want a read/assignment seems overboard.
* MM: You said the compiler transform is complicated and the users isn't, but those are the same complexity. Having trouble understanding what you mean by footgun, would appreciate an example
* AB: With in/out and ordering you’re passing same location twice. With compiler transform it has to deal with all cases, but for user they have a specific case. Not the only case the compiler has to worry about.
* MM: Could ask to get a couple lines of example code?
* GR: Think the idea is if we defined alias these two params this is what will happen, how many folks will actually read spec to see how in/out works and how many do it by mistake and shoot themselves in the foot.
* AB: foo(x, x) has a footgun
* JB: If restrict reading to just proposed change and not all of ptr restrictions it’s easier to understand
* MM: Is foo(x,x) the every member assigning example?
* JB: Here’s an example of TR’s concerning code:
    * void foo(inout int x[100]) { … use x[i] … }
    * foo(myArray)
* AB: Take your example and … then in/out looks like every member of the array and copies every element in and out.
* MM: Feel like that’s orthogonal, is it possible to assign over all members of array with a single operation we have that already
* AB: As in/out is described that’s exactly what will happen. DN filed issue that TR described about workgroup memory which has some details. ([https://github.com/gpuweb/gpuweb/issues/3276](https://github.com/gpuweb/gpuweb/issues/3276))


---


# 📆 Next Meeting Agenda Request



* Next week: ****APAC!**** Tuesday, September 06, **5-6pm** (America/Los_Angeles)
* [https://github.com/gpuweb/gpuweb/issues/3276](https://github.com/gpuweb/gpuweb/issues/3276) 
* 