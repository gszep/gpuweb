# WGSL 2022-06-21 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon** Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-06-14 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1N--8pETKrEyJbWhTEx-xTy92jowpr-bj9wuql-hbh0s) 

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
    * **Dan Sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Price
    * Rahul Garg
    * Ryan Harrison
* Intel
    * Hao Li
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
* **Dominic Cerisano**
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



* 


---


# ⏳ Timeboxes (until 11:30)


### [uniformity analysis + basic tail-branching #3007](https://github.com/gpuweb/gpuweb/issues/3007) 



* DN: last week, stated MSL and HLSL have wave reconvergence at end of structured block including function call. Doesn’t fit intuition on how drivers are structured. Working on example to show what’s happening. Based on VK subgroup reconvergence test and ported to DX12. We think drivers like Nvidia and others have same backends on compiler stack and think Vulkan behaviour would appear on DX12. Haven’t seen docs to explain that. In MSL close brace was started to do the same thing. Don’t recall filing an issue, but there was an issue with metal team filed?
* TR: Wanted to try to clarify about reconvergence at end of function, trying to say logically all the threads that were active before should be active after function call. Doesn't’ mean converged but visibly a wave operation after function call they have to be converged. In other words, implementations are free to diverge until an operation that would expose that divergence is performed.
* AB: So wave ops are like barriers? SPIRVs don't’
* TR: The model is the wave runs in lock step. When wave op happens if the behaviour is different then if running in lock step then not according to spec
* KG: If that’s true, isn’t that what barriers are for?
* TR: Barriers synchronize the waves in a group
* KG, a group of waves
* TR We don’t have a wave barrier, as it’s assumed the wave is in lock step.
* DN: Point of comparison, subgroups in opencl had a subgroup barrier added to ensure all the threads got to that spot. Saying, in HLSL model acts as if any time you could sense the divergence there is a barrier. Any thread still active at all would have to come back and participate in that group op.
* TR: That’s right. If you have a wave op you have to insert the logical wave barrier (you said thread group)
* MM: For comparison, metal requirements are weaker. Different policy for ops like barriers and derivatives then wave ops. For wave ops it’s insert and if some divergence can be observed via wave ops that can’t be observed from barriers or derivatives.
* DN: Can metal force the reconverge like HLSL?
* MM: Not sure. Haven’t spent time thinking about wave ops.
* DN: Concerned we’ll have an execution model that does not map to hardware. Dangerous territory. Question for Apple, how do you compile HLSL to metal in a portable way.
* MM: We don’t compile hlsl
* TR: If you have a loop and a wave op in a loop could you have threads in the loop in different iterations doing the wave op. That’s not allowed in HLSL. In HLSL if the thread is active then it has to be in that wave op. Does metal allow divergence of threads in that subgroup to the point of threads being in different iterations?
* MM: Would have to see example to answer.
* TR: Another example, the convergence case of wave op after function or block diverging will that wave op happen many times on different subgroups of threads because they reach it at different times?
* MM: For that case, if I understand, a function call that inside has divergence and there are [???] in the divergence and then after you do a wave op?
* TR: Could be an if that has expensive divergent code. Could thread that skip do wave op while other threads in the if?
* MM: In spirv those are different constructs. Metal doesn’t have an opjoin. Talking about a return in SPIRV that skips the op join.
* KG: Given others don’t skip it, should we just not skip it? Said there maybe overhead, right?Have to assign to function local thing and then return at the end of the function. That’s the transform?
* MM: The argument mentioned last time, which I think is the strongest is don’t think programmers mental model for GPU software involves different policies for returns
* DN: Trying to expose what hardware is doing, not invent new stuff. Don’t lie to the programmer
* KG: That’s the fiction we’re in. Proposal is what if we can help the programmer do the thing they want at low enough cost. It sounds like this is just how other systems work, there is no way to not join. Seems like good indication that if there is something to gain it isn’t sufficient to make folks care. Only question, how bad is this to implement as a transform assuming it’s likely it’s a spirv-only transform.
* MM: Implementation of transform exists in spirv. Not an implementation cost and integration cost.
* KG: Yes, but spirit of concern is complexity of having our code do what we want, not how we go about doing that.
* MM: Ok.
* KG: If one answer is ship spirv-cross, that’s not useful.
* MM: Suggesting lifting a piece, not all.
* KG: How do we feel about this,do we want to volunteer to do this work? Do we think the tradeoff is worth it to match what devs are expecting even though in spirv there is a case where it’s less optimal?
* JB: Lifting in spirv-cross, or pieces of spirv-cross is not feasible due to different IRs and converting back and forth. Would have to re-implement ourselves. What do spirv users do when they want to write a function where they can use interlane operations after the function returns? Do they normally re-write their functions to have a single op return instruction?
* KG: Two options, do an get correct behaviour or don’t and sometimes get correct behaviour
* DN: And they don’t notice.
* KG: Unfortunately a lot of the uniformity protections protect against it might go wrong vs it will go wrong
* JB And the transform is to take the early returns and take the tails and make them conditional instead of opreturns
* KG: And hoist the return
* JB: Yea, store in a local
* BC: JP wrote a transform for tint which we think is sufficient and is very well tested
    * [https://dawn-review.googlesource.com/c/dawn/+/93660/5/src/tint/transform/merge_return_test.cc](https://dawn-review.googlesource.com/c/dawn/+/93660/5/src/tint/transform/merge_return_test.cc)
* JB: We do similar gymnastics. Think it’s implementatable. If all the implementations take same approach of rewriting spirv then i think this is something that FF can take on. Already doing various things like this.
* AB: How often do we need to be right about reconvergence for the guarantees in the spec for that work to be valuable? Know that not everything works but it tends to happen in cornercases you don’t come across most of the time. What we release in 1.0 might be difficult to observe if we release subgroups/wave ops it becomes easier ot observes. At what level of going wrong is it a spec issue?
* KG: Interesting question
* MM: Barrier after function call and barrier could deadlock, seems like a bad time
* KG: But, how likely is it to deadlock. Would be sad to ship something where it works by luck even if we don’t have to be that lucky. It’s sad when looking at a system with older drivers and there is no formal protections we can use to get the thing we want and would like to avoid scenario where we don’t have sufficient protection there. That said, folks do have a workaround if they try it and it doesn’t work. But, if we’re all on board to do the transform, can just do that knowing there is a gap we’re filling. Good point we don’t need to be perfect
* TR: Don’t really know what the difference between doing the transform and doing an OpJoin is?
* MM: Can’t just say OpJoin wherever you want.
* JB: Really just imposed a tree structure that fits into block graph but it’s really a tree. You can recover the tree from the graph of blocks.
* MM: Another way to say is in order to stick OpJoin where needed all of the leaves of the tree would have to re-converge to hit it according to spir-v rules and you’ve just built the transform we’re talking about
* TR: So, if around function call the implicit join may not join what diverged in the function call just what diverged for the if? Is that right?
* AB: Theoretically, everything that entered the merge instruction that leaves through the merge block will reconverge at the merge block. If warp in if true not confidence the driver will just remove that and fake reconvergence
* MM: Doesn’t describe the function issue
* AB: Not what  he’s asking if
* JB: Wrap all functions in an if
* TR: Not sure of limitations here, and if there are problems with an if in code and a function call and the function has divergent returns how is that dealt with today?
* MM: Would be an alternative implementation where one would have all the control flow reconverge in the function, the other would be wrap all function calls with if(true). Implementations could pick what they want to do.
* KG: Can we sign up to have reconvergence happen and we will promise we will make in spirv or if we can’t make work it will work enough so folks won’t be mad where it doesn’t work. We’ll claim we will do it, especially as it relates to uniformity, so they can do if’s in uniformity and not have to hoist themselves. Feel like we should do that as we think we can do it. Going to say we’re going to do taht.
* JB: If just spirv we can handle that.
* AB: Just want known reservation that don’t know how well this has been tested. When we looked in spirv it didnt’ work. Doesn’t give tonne of confidence in other backends as it’s usually the same folks working on other drivers. We’re perpetuating an illusion around errors that we introduction to make folks lives easier. Almost circular in that we want to get rid of uniformity analysis errors by working around errors.
* KG: Important distinction, sounds like you don’t agree with what proposed. Not that we paper over but that we do the transform to give maximila hope they will reconverge. Driver might make it impossible but we’ll try our hardest, saying in WGSL we’ll try hardest to give reconvergence here and also update our uniformity analysis to reflect that. Not promising anything new, we’re going to do the transform the use would have to do anyway when uniformity rejects. They have to hoist the return and we’re just going to do it and if that doesn’t give reconvergence in the driver then there is no hope but we’ve done our best for the driver.
* MM: What AB said it doesn't’ work, what is ‘it’?
* AB: We looked at reconvergence on various drivers and it didn’t work for what was promised now. If i add a CTS case I know will have driver bug, is that driver a non-conformat and need to be denylisted?
* MM: Those tests are test that the spirv guarantees were not followed by hardware?
* AB: Yes.
* MM: For later case, talked in WebGPU case about driver bugs in CTS. A lot of arguing but resolution I believe.
* KG: Think we can give assurances, I view it as we should surface the guarnentees underlying implementations have and make them eaiser to use and if drivers break guarantees we should respond on a case by case basis. If we find drivers have no guarantees we can come back to drawing board but that’s a bad sign for the spirv requirements. Don’t know how to operate if all the tools are broken. Don’t have a strong answer for how we should design then.
* MM: Question is, are the tools are so broken should we behave as if they don’t exist? Think the answer is now
* KG: Should surface guarantee in API and respond to make it so.
* DN: Ask MM  to repeat Metal guarantee, on wave reconvergence group operations do not force reconverge but derivative do. If only targeting d3d and not metal would we only care about derivative requirements?
* MM: Not sure about not care part?
* DN: Would we have to spe canything with uniformity and derivatives?
* MM: Derivative after function where function has divergent returns the derivative after is legal.
* KG: And do that by reflecting that in the uniformity analysis and have it pass
* TR: So, derivative ops force reconvergence if we have to do this transform to get reconvergence?
* DN: If metal doesn’t need transform and HLSL doesn’t and because discard is demote then if only targeting D3D and Metal then we would not need uniformity at all for derivates
* TR: I see
* AB: That’s impossible
* MM: Need to know derivative in non-uniformity control flow
* AB: Can violate condition of if to force reconvergence down branch
* TR: derivative only needs quad convergence not wave convergence
* KG: Talked about one level uniformity but technically 2-3
* MM: Now unobservable identical but may not be in the future.
* KG: **Continue next week**.


### [Include arrays in creation-time constant expressions #3056](https://github.com/gpuweb/gpuweb/issues/3056)



* BC: Implementation shader creation time expression evaluation, tried to deprecated module scoped let as we said const was superior and discovered shaders with module scope let arrays. (and by whole load, quite a few). Wondering if arrays could be part of const expression plan. If we do that, could other things like going further for abstract numeric in arrays, do we have structures in const expressions. Haven’t seen any shaders with structures but seen a lot of arrays.
* KG: Would punt structures for now.
* BC: Happy with that. Any concerns?
* MM: In shaders found with let at global scope to define an array how do they spell the array? What characters do they type after =?
* BC: Have to type array&lt;f32, 5>(1, 2, 3, 4, 5). Immutable lookup tables
* MM: So we synthesize constructors so if i said array&lt;f32, 17> we’d make the constructor?
* BC: Bit lost. Always ben able to have array constructors as lets so you’d say array&lt;i32, 5>(1, 2, 3, 4, 5)
* MM: If the array is size 17 you write 17 things
* BC: No way to writer fewere (modulo 0 initializer). Can’t do single element splat with array unlike matrix and vector
* MM: See no reason to not have this.
* BC: Do we want non templated form to match mat and vec to infer type 
* AB: If want abstract then we have to do it
* BC: Yup.
* MM: [??]
* BC: So, array(1, ,2 3, 4)
* MM: Makes sense
* MM: Phrased as compiler creates constructor, do we do this for structures already? So you cannot use a structure in the global scope?
* AB: Don’t see why constructo rat module scope
* MM: What do you put after equal
* AB: Member wise initializer for structure.
* MM: So that should work with const also?
* KG If not asked for, don’t want to do it. If it’s free, sure but these things aren’t necessarily free and want to focus on user wants
* MM: View compiler generated constructors as identical for arrays and structures
* KG: Second request, spec most basic thing and get that through an dwe can propose structures for const. Maybe too afraid of it being spec work
* BC: Also implementation work to support structures in Tint. Would be keen to leave structures to future support in const.
* KG: **Resolve for now, allow arrays and it sounds like generic arrays**.
* BC: Sounds good.


---


# ⚖️ Discussions


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* MM: Seem to remember close to agreement.
* KG: DN wanted to think about it
* DN: Stuck on the other issue. Concerned we get a useful thing folks need. All things point to users don’t need this and ti will be slower code.
* KG: Vs what we have now which makes fast code because we forbit alias
* MM: According to js if you can prove 2 pointers are equal then you might generate faster code, and also if you can prove they’re not equal, you might also generate faster code.
* DN: Don’t care about equal case, not code folks will caring about. If equal you can specialize the function itself and send a single param.
* MM If remember correctly all vars and pointers into a function for us to emit code as you can see everything in function. Using global analysis. Sounds like thing just said contradicts that.
* DN: Talking about function params. That’s the interesting case. Assume restrict basically.
* TR: Wonder about how this impacts global case? Can see how locally analyze and prevent illegal case, but if you have pointers to globals all hell breaks loose. Don’t know what might alias
* MM: Not quite understanding. If rules in WGSL are in-out params to function which are pointer type can be assumed to not alias anything, if that’s the rule then inside a function …
* TR: Do you mean alias anything the function has access too?
* MM: Yes
* TR: So can’t pass pointer to global the function might have access too
* AB: Only one thing can do writing
* MM: function takes pointer to int and called where pointer is set to global and inside function use global then in that situation if the rules are that the params are not allowed ot alias then you violated the rules of the language.
* TR: But you can’t locally analyze the function, you have to globally analyze
* KG: HEre you can because you can’t pass in global scope pointers
* TR: It’s a pointer to something in global scope
* KG: Pointer has storage type and we forbid function argos that are in global space? \
AB: No, you can do pointer to workgroup, private and function
* KG: OK. So you can
* TR: There is an example which does this exactly
* MM: Worth clarifying about in all this discussion talking about rules of language if someone breaks rules the compiler doesn’t emit error, you get non-portable behavior
* TR: Trying to make it so compiler emits errors right?
* KG: Balance, tell people if doing something wrong
* TR: Is it possible, for v1, to ban passing pointer to something global to function call? Or something like that?
* AB: For user declared functions, sure, we can add more restrictions
* TR: Would solve issue for now, we’d have to re-write functions to make it work
* JB: So, take private and workgroup out of function argument pointer?
* AB: That would be one way to do it.
* KG: Does that break a lot of things?
* DN: Filed an issue about workgroup variables with atomics you have issues and one solution is to not allow to function params. So, already have issue to pull out workgroup. Would have to think about private.
* AB: Since we’re going to HLSL with copy in/out, confused how we’d make alias do the right thing?
* TR: Requires re-writing function and specializing for global
* MM: Trying to get there with last example written where for something like this we can’t specialize as we don't know if they alias or not.
* TR: Which example?
* MM: foo(&myArray[bar()], &myArray[baz()])
* MM: If we wanted to take that line of WGSL and go to HLSL we don’t know if they alias or not so we don’t know if we specialize
* AB: We ban that case already. Function restrictions, every pointer type must be function param or direct address of identifier expression. Variable identifier. So, indexing into array is not creating a valid call argument.
* MM: Didn’t know that.
* KG: Ok. Should we come back to this?
* DS: Sounds like we need to think about what it means to ban private as we already have a bug for workgroup
* MM: Also think about since this is illegal could we determine alias without being exponential time?
* AB: You can detect but you’re conserviate. If some writes are conditional you have to flag them and you error on code that isn't an error. Not great user experience
* JB: All static analysis has this issue. Naga collects information about all the globals every function touches and we know there are no cycles in call graph and since we know what globals a given function touches transitively including all callers, it’s easy to detect you’re passing in a pointer to a global that this function might access and that’s not allowed. And if tint does the same thing and i suspect you are and maybe we already have the information we need to report errors. The only misfortune would be describing it in the spec.
* MM: One option is to just not have globals.
* JB: That’s metal. We’re already faking in metal and passing in params so that’s why we do the analysis.
* KG: **Lets do thinking**.


### [should WGSL allow shadowing of builtins at module scope #2941](https://github.com/gpuweb/gpuweb/issues/2941)



* DN: Larger conversation on language evolution and smooth monotonic. Divided features into sugar or not. Sugar is done on software side, rolled out by all browsers at same time (e.g. do {} while). What we want is monotonically increasing set of sugar features. There is backwards compat because they’re added. Then evolve language with minimal tuling support. No a big set of on/off features, just outer shell that’s increasing. Then if you ever have breaking change call it WGSL2. (Basically semver). Want devs to opt into higher shells for functionality. Big debat, in shader with declaration of some form, or API side saying I want to use WGSL features up to 1.55. So, tooling can be simple and implement a single level and understand everything below and when going through code, report the maximum minor version used (you used up to 52) and all good. Then on the API side can say use up to 55 so you don’t get accidental feature creep. Can set version to what is know to be supported in all browsers. Framework to think about how features evolve without having to add things that require namespaces and complexity. Then debate is in vs out of band. Once that’s resolved we can then say do we allow a user declaration at module scope to shadow a builtin that maybe introduced? Would like understanding about monotonic increasing and backwards compat features. At least start conversation.
* MM: Not sure if can sign off on this immediately because 1) from dev point of view if there is no technical reason why one feature depends on another it’s hard to say i want to do feature x but i have to do y first. From a browser dev point of view 2) this is not how javascript is developed and JS arguably has 2 versions with use strict and think much of the js community sees that as unfortunate.
* AB: To the first point, choosing only to implement some features is bad for the language. For c++ standards, you may have experimental support as things get added but you don’t say you do c++17 if you skip some of the features. Think rust and go are similar. When you support the version you support all the features. We’re going to agree to all the features. We’re just not going to agree to features we won’t do. If there are things a browser doesn’t want they won’t be standard features. So, not a big deal. For js, won’t speak to that.
* BC: To reinforce what AB said about 1). Would hope the WG would agree to features that make it into the next minor release. So, if things that are blocking like Google doesn’t want to do a feature of the point release they we shouldn’t have agreed to have in the release. So, like we're agreeing on v1 now, i’d foresee us with this approach we’d meet a YY interval and agree what goes in next minor release and agree we fell comfortable to implement it.
* KG: This seems like something in this direction seems reasonable. Need to give more though. Important question is core one of what do we do with new builtins. If we make `determinant` do we use the user one as an override of the builtin, that seems tentatively where we were going.
* MM: Discussing 2 different models. a) was proposed, every version is numbered and author declares what version they want and if they collide with stdlib then they’re doing it wrong and get compile error. b) is the language is not numbered and the sugar features are implemented whenever the browser gets around to it and strictly additive and don’t cause existing code to break and when new sugar features become implemented in the browsers then authors start using them.  Authors start using at own schedule. Both are possible, c++ is example of first, JS is example of second.
* KG: Not exclusive that way, can have increases that have bits that tag along anyway. Want to read in more depth.
* BC: Make clear that proposal is not when you request a new feature request new stuff gets enabled. You don’t have new symbols appear because you requested later version. This would have 2^N compiler complexity of enabled/disabled features. Proposal is so compiler doesn’t know about target version, it just compiles and then reports high water mark and at shader level you can check.
* JB: To js model, dom apis are introduced piecemeal but JS language is linear They have year numbers and you say which year snapshot yo have implemented. So the lang and standlibrary are separate things.
* KG: **Think offline and come back next week**.


### [wgsl: function param that is pointer-to-workgroup is problematic if the store type contains an atomic #3067](https://github.com/gpuweb/gpuweb/issues/3067)



* 


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, June 28, **11am-noon** (America/Los_Angeles)
* 