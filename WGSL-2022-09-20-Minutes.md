# WGSL 2022-09-20 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** ds

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-09-13 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/132iZ5v2W4_mu0MB0weaIzZiBrk4Kj0F_B3Psnlk7sJ0) 

 

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
    * Ben Clayton
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
    * **Greg Roth**
    * Michael Dougherty
    * Rafael Cintron
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



* Notable merges: [https://github.com/gpuweb/gpuweb/pull/3447](https://github.com/gpuweb/gpuweb/pull/3447) Clarify flushing of conversions between different width float types. Fixes [3421](https://github.com/gpuweb/gpuweb/issues/3421)
* 


### WG Charter



* **Please Review: [https://github.com/gpuweb/admin/pull/18](https://github.com/gpuweb/admin/pull/18) **
* 


### CG Charter



* **WGSL Only: [https://github.com/gpuweb/admin/pull/19](https://github.com/gpuweb/admin/pull/19)**


---


# ⏳ Timeboxes (until 11:15)


### [Sign change error should be for &lt;< #3459](https://github.com/gpuweb/gpuweb/pull/3459)



* MM: I’m surprised that this restriction would occur for ints. For comparison, the definition of + says “Addition. Component-wise when T is a vector. If T is a concrete integer scalar type, then the result is modulo 2^32.”
    * MM: This makes the language inconsistent.
    * DN:  AbstractInt is a different type from i32.   If addition in AbstractInt overflows then it is a shader creation error.
    * DN: AbstractInt is not pinned down to a bit width.  You want to be able to expand the range of AbstractInt while not breaking the semantics of shipped code. That’s why overflow induces a shader-creation error.  AbstractInt is an approximation of mathematically pure integers.
    * (I misunderstood the thrust of the distinction.)
* MM: Confused as section of spec says shift left (concrete) so though talking concrete and not abstract
* AB: Error which could be both abstract or concrete
* MM: Imagine program 
    * let x = 0x0FFF_FFFF;
    * let y = 8;
    * let z = x &lt;< y;
* MM: What would happen with the above?
* AB:  If change sign of number as shifting, stops being multiply. When tried to make errors consistent put entry in wrong column. If shouldn't have error at all we can talk about that but error is don't change sign on left shift.
* MM: Example is `let`. So, x and y are concrete.
* AB: Error only occurs for override and const expr. At runtime would get modulo. Don't say anything about changing sign.
* MM: So, well defined answer that can be relied upon?
* AB: I believe so
* KG: It has a set of allowed behavours, allowed but not required to change sign bit?
* DN: x &lt;< y in this case is not const expression so does modulo behaviour (required)
* MM: As long as that's required, that's no problem.
* AB: Modulo is about how many bits of the shifter to take. Sign bit is independent of modulo behaviour.
* JB: In some languages shifting off end is UB, but seems like vulkan shift left logical says bits go left.
* MM: So value of z will be 0xFFFF_FF00;
* AB: At runtime, that is what we expect.
* MM: Satisfied with runtime, for compiletime, rather then match runtime we make error because it's confusing that shift left causes number to flip sign?
* AB: Right. Matches what we did for abstracts.
* JB: Also have option of cast to unsigned an dshift, so no error if you want that behaviour. Can catch mistakes but workaround if that's what is desired.
* **Resolved: Accept**


### [wgsl: add way to get special values for a given numeric type (e.g. max, min, lowest) #3431](https://github.com/gpuweb/gpuweb/issues/3431) 



* DN: Because inf and nan is not reliable, and want to have max, want the most negative or positive thing would be nice to have way to say that. C has macros, c++ has stuff. Names are confusing. Some mechanism to do that would be nice, not dogmatic on what that is.
* MM: This is a good feature in general. From experimentation, HLSL doesn't seem to have this
* TR: Think that's right
* MM: For GLSL not in compiler explorer. For MSL uses FLT_MAX processor defines. Little sad with that as think ergonomics of global isn't great. because some GPUs support inf and some dont', saying largest or biggest isn't quite right as there is a bigger number.
* AB: Could be verbose largest_positive_normal().
*  ds: FYI there’s this too:  [https://shader-playground.timjones.io/](https://shader-playground.timjones.io/) 
* MM: Idea by BC to have highest&lt;type> but making functions rather then variables. Increases probability that adding feature that deals with &lt;> will play nicely.
* DN: Right, wasn't implying variables. Function form is nice.
* MM: Don't want std::numeric_limits. Went back and looked and we don't have namespaces.
* KG: When you say functions do you mean `smallest&lt;f32>()` that gives smallest
* AB: Dislike exposing template like params outside types.
* MM: DG had similar thoughts. Grammar distinguishes between type constructors and function calls. Doesn't have to be true. Adds a bit of complexity to grammar and spec but doesn't necessarily add implementation complexity.
* KG: `smallest(a: T) -> T`
* MM:
    * let x = 3;
    * let y = smallest(x);
* DN: Give me the thing which is the smallest of whatever this thing is.
* MM: Names smallest is confusing, expect that to act like min. Would expect `min(x)` to just return x.
* KG: Right, maybe ergonomics don't work out.
* DN: Left unit for comparison of this type
* MM: Good as it internalizes the the [...] but is useful when you compare them.
* KG: Worth thinking about, but don't' want to spend innovation budget here. JB, what does rust do?
* JB: Rust has associated constant on type `f32::max` not sure if it's type or module. Both a type f32 and module std::f32. Some kind of scoped associated constant. Probably not what we want here.
* MM: Use case to consider, imagine buffer of float and want minimum float. Variable lives outside loop and is initialize,d loop goes over all and comparse current to outside. Usecase here is the outside gets initialized to largest thing, there is no comparison to pass in. There is no `x` it's a standalone. Should make sure we can satisfy the use case.
* KG: +1. Might end up with smallest_like or smallest_of and could just have smallest_u32. Also like u32.min or f32.smallest. Think about it a bit and give some time.
* MM: That’s also what Swift does, e.g. int.min
* **Let’s table and think more before deciding here**


---


# ⚖️ Discussions


### [wgsl: pointer-to-workgroup in user-defined functions · Issue #3276](https://github.com/gpuweb/gpuweb/issues/3276) 



* ( PR at [wgsl: ban ptr-to-workgroup params to user-defined functions #3422](https://github.com/gpuweb/gpuweb/pull/3422) )
* DN: Don't like Apples arguments and don't think they're consistent. AB wrote some feedback. Don't know how to progress on this. Not sure how to structure this conversation.
* KG: What I wanted out of this was to try to center on options we have. Hard for me to decode options presented.
* MM: Think there are 2 thoughts that we have. 1. Disappointed to see from ABs points that y'all only found single option as positive option. We take resolutions by consensus so was hoping for positive and negative options and pick intersection from there. 2. Think it's probably worth discussing more about the don't add any kind of out param language feature now but add fully formed parameter feature in a year or two. Think there is more runway down that path which can be discussed more.
* KG: So, that would be for more fully formed pointer parameter feature? (MM: yes)
* MM: Pointers that alias and can be taken from address space
* KG: Right so fully formed pointer parameter feature
* MM: Right DXIL, SPIR-V and MSL representable.
* AB: Addressign point about positive options, we're here as trying to restrict spec, took a long time to get to this position. This is already a compromise position. We're always going to have differences in address space functionality as that's fundamental in SPIR-V. Variable pointers for example, won't get in all address spaces, limited subset of address spaces. Haven't looked at DXIL to see what it has. Would be surprised if they were standard pointers without restrictions, but would require review to understand. Don't think we'll get ot pointers are the same regardless of address space in a year from not. We aren't going to get there so don't aim for that.
* KG: Think agree we wont' have all pointers everywhere fully supported in the broad sense. Not super against having different rules for different pointers. Wonder though, how bad would it be from a writing perspective if we don't let people use pointers for parameter values.
* DN: Would be horrible. People will yell at us. When weight thinking about that vs sort vector type alias which we resisted. That's a tiny thing vs restructuring code to change return types and declare locals. That's going to be mutiny. Don't want to go down that path.
* TR: Curious, maybe there is a minimal pointer case that could be made to alleviate the biggest pain point without adding the complications to workaround backend limitations. A kind of out type param for pointer. No aliasing, thread local address space output pointer. But, not for general pointer use everywhere for params on functions. Alleviates that pain point of returning multiple things and dealing with structures.
* AB: That's the option we picked. Pointers option 2 is you only get it on function and private (and could remove private) if you wanted in/out then add local variable before function call, init with address and pass to function. With little code you get functionality you want. Static alias analysis removes issues. Pointer option 2 gives users enough without having to spec fully pointer functionality and what is offered where.
* MM: Wonder if missing forest for trees. Maybe what we need is multiple return, return a tuple. Then we don't need out params at all.
    * var x = 7;
    * var y = 8;
    * x, y = foo(x, y);
* DN: People use for in and out. Will read existing shaders and we'll have simple explanation if you see a do b. Re-writing function return types is a bigger deal.
* KG: Couple things in play, the pr we have bans pointer to workgroup and leaves other pointer things which gives construct AB mentioned. Other thing is what if we got rid of pointers altogether which is a bigger hit to folks writing this code. Even though these are in a similar area these are different feeling things. Folks seem on board with ban ptr-to-workgroup. Seems to be consensus for baning that. Main discussion is if we want to go further and address pointers in functions and if should be inout and how do we want to do them. Can we just accept this PR for now to move forward and we can keep talking?
* MM: Not very happy with this option.
* KG: Moves closer to maximum you don't love?
* MM: Fair way to say it. In HLSL you are capable of describing this. In MSL you can describe this. In SPIR-V I don't know if you can describe this. So, WGSL is no longer the intersection.
* AB: WIth base SPIR-V you can not express the thing. THere are differences in how you'd express them, would have to write differently.
* DN: The genesis of treating workgroup differently is TR feedback that this feature is not always translated in a way the programmer expects and is almost always a bug and folks should not be writing code this way. In spirit of more seat belts for programmers to not accidentally get into sticky situations, that's the genesis for this PR.
* TR: Wasn't quite sure how saw it as expressable in HLSL where there are no pointers and no defined behaviour, It may do this, but isn't defined that way.
* MM: Have discussed author writes programm where group shared array is passed to inout param of function. Not case of interested in. Referring to similar case where author is using inout in group shared correctly. Don't pass whole array to function, do subscripts outside and pass member to function call.
* TR: Understand, would be legal but would be different behaviour as defined as copy in/out at function call boundary as opposed to translating to group shared operations
* MM: Right, that's true. You're right it has different semantics to pointers. The fundamental piece is the language feature is being able to have an out param to a function it isn't the semantics of inout or the semantics of pointers. If we're going to have out params but have the language features but be less permissive then pointers and out params then that's unfortunate.
* TR: Wondering where it's less permissive then out params in current proposals. Looking and think the main crux is the root identifier is the same then it's an alias and that's illegal. That's the main sticky point. Making that alias illegal means you can't use it in the same scenario where it doesn't actually alias.
* MM: This would be representable in HLSL and MSL (and maybe SPIR-V) but not in WGSL if we accepted this PR
    * `groupshared int groupSharedArray[10];`
    * `void foo(inout int value) {`
    * `    value = ...;`
    * `}`
    * `foo(groupSharedArray[thread_id]);`
* KG: One of the things that comes to mind is that to me I like MMs point this is about the language feature and what we're trying to do here. Understand that while inout in the language instead would likely cause a maintenance burden if we want to support I do wonder. The goal isn't' no maintenance it's a reasonable maintenance burden. Trying to figure out the boundary. It isn't off the table but I'm one voice of many (and not speaking for Mozilla in this case). We have discussion about static analysis on agenda, would this be easier if we did static analysis first. Would this be easier without dealing with alias of 3336?
* AB: Think there is an extra restriction for pointers from SPIR-V where you have to pass the whole object as the parameter. That's a requirement we get from SPIR-V. Wonder if that is being added as the extra complication. It's taken all together as restrictions are problematic vs this one restriction is problematic. It's the sum of 3 restrictions is a bridge too far.
* DN: That constraint of pointing to whole object means MMs example of passing indexed place in array is not permitted in baseline SPIR-V. So, what they want is outside the intersection of the language and is only available with future additions which maybe in a year or two
* KG: Because you can't don't that thing directly you could instead have the implementation do inout itself.
* JB: Don't think we have a way to translate that
* KG: Create temporary?
* JB: I guess but then you have copy in copy out
* KG: inout we're talking about is always well ordered copy in copy out.
* MM: It seems like compilers will hoist the defer into the register and act on reg and store out to memory. So it's always possible to provide transform
* KG: Just don't write races
* JB: That's what i' worried about. With the aliasing restrictions we'd like to have maybe that is a transformation. Hadn't occurred to me as something we could lift. Don't know if it needs to be v1. If we could do local transform that permits passing pointer to components of things we should do it. but maybe not v1
* DN: Right now, restriction on ptr to workgroup such that it can't hold atomic as it definitely isnt' want is expected on copy in / out. Can have atomic vars inside workgroup. If inout will still have corners and barnacles.
* JB: So that transformation won't always work
* DN: It isn't' clean as you might think. Will have to carry bannicle over. Argument is inout design has confusion for programmer where they will trip over own feet and you need aliasing restriction anyway. No magic to make confusion to go away if it's by ptr or inout.
* AB: Think we should consider the spec ramifications. Parameters are not assignable right now, spec language needed. If you leave function and private, inout like behaviour is simple. Without function and private ptr it's a lot harder to write. Not sure if gain benefit for functionality. Vs having spec as is. If we're planning on growing to pointers we already have the basis and it's more forward looking then adding stop gap which we plan for no-one to use.
* MM: Wanted to agree with point by JB about restriction on pointing to whole object. In longer comment mentioned was a restriction we'd like to remove. This PR goes in the opposite direction we think. Encodes deeper into the spec
* AB: Don't think it does
* JB: Don't see that but want to work harder to make things easier for programmer when translation is possible. Confusion is which translations are possible.
* [...]
* **KG: Is it fair then to call this discussion “function-out-param language functionality”?**
* [...]
* [the following is KG’s recollection]
* MM: I want to mention that tuples [and destructuring] might also be a way forward to give us a function-out-param feature in the language.
* KG: We definitely didn’t vote against tuples, we just triaged them into post-v1. However, if bringing them back into scope for v1 might possibly cut this gordian knot, it’s worth entertaining the idea and checking in with implementors on implementation complexity (with regards to shipping/mvp targets), and also worth considering what the impact would be for authors and/or porting.
* DN: Every day that goes by without people being able to run their code is a loss.
* JB: I was worried that it might be late in the game for v1 to add tuples/destructuring.
* DN: Adding to the spec is probably non-trivial. Would need to fit it into our uniformity analysis framework and 4 people in the world understand that. (which is separately sad)
* MM: Could we define it as a source-level translation?
* KG: I think so, but I do acknowledge that building our stack/scaffolding of translations too high is also not ideal, even if we know each piece works.
* JB?: I think it would be better to not build them so high, yes.
* KG: **Tabling this for the week so we can talk about the next topic. I would like to see more of a consolidation of solution spaces, so we can focus our discussions**. I also want to think about tuples myself, and generally how we might aim for some way forward on function-out-param.


### [Add a static alias analysis #3336](https://github.com/gpuweb/gpuweb/pull/3336) 



* (previously [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457))
* KG: DG took look earlier and some nits. Does Apple have thoughts
* MM: Both read and understand. Seems to do what it says on tin. View this and previous issue as single thing.
* KG: Is there a path out of this combined issue that doesn't want this PR?
* MM: Think DNs comment about if we had inout wed' still want this we think that's reasonable. We just don't want acceptance of this PR to have bearing on larger issue but if we can make progress now that's fine.
* AB: Having inout would change the stuff about global variables would no longer apply and would only have callsite issue. Just in function.
* KG: Just part of language we could ditch?
* AB: Sure.
* KG: Then think we should take this and have slightly firmer ground for discussions.
* KG: Will give some thought on how we can make forward progress
* MM: Was wondering about opinions on returning tuples
* DN: Well and good, but late in the date for v1. Maybe for post-v1. Users will want this feature, will see existing shaders and want a log cognitive load translation to WGSL.
* KG: That's my gut feeling. Like tuples and love destructuring and think all languages should have but they're generally tolerable to not have in v1 and we've triaged accordingly. But,  if that is the sword to cut the knot then would like to talk to JB to see how bad it would sound implementation wise. Would be a way to get this kind of thing if implementation load isn't too hight and isnt' load to existing authors vs what thye have today. So think it should be a bit on the table.
* JB: Haven't been talking tuples as I thought we didn't want to introduce into spec as we want v1. Tuples are fine, they're much smaller then other things. If multiple value return, destructed assign/binding would help move past would be interested in looking at proposals.
* GR: Do we have a date we'd like to hit for v1
* JB: Last year.
* GR: Right, ASAP depends on P.
* DN: Think we are losing a lot of value which folks don't have functionality they can use. There is only so much polishing of rocks I want to do.
* KG: Right. Need to think if implementation cost isnt' too hight and we can jump into that soon rather then continuing to discuss as it's difficult to get consensen. Maybe time difference there, even given all have to do impl work it might be acceptable. 
* DN: Someone needs proposal that addresses unifiority analysis and is coherent with spec. 
* MM: If src level translation then uniformity falls out.
* KG: Hope that's what we can do, not that I want that many translations, if already shown  true then could hoist on that framework
* JB: Don't like the spec to have too much of that stuff. Nicer to go into uniformity and see the constructs described.
* **Resolved: Merge this (but remember that this doesn’t sway the larger discussion on function out-params)**


---


# 📆 Next Meeting Agenda Requests



* Next week: Tuesday, September 27, **11am-noon** (America/Los_Angeles)
* 