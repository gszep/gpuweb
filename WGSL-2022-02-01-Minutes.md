# WGSL 2022-02-01 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️ Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon Americas/Los Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Myles C. Maxfield**
    * **Robin Morisset**
* Google
    * **Alan Baker**
    * **Antonio Maiorano**
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * **James Price**
    * Rahul Garg
    * **Ryan Harrison**
    * Sarah Mashayekhi
    * Jaebaek Seo
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
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * **Dzmitry Malyshau**
    * **Jim Blandy**
    * **Kelsey Gilbert**
* Connecting Matrix
    * Muhammad Abeer
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* UC Santa Cruz
    * Tyler Sorensen
    * Reese Levine
* **Dominic Cerisano**
* Eduardo H.P. Souza
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Michael Shannon
* Pelle Johnsen
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


### Virtual F2F



* Virtual F2F week of (Monday) February 7th
    * (See email “WGSL VF2F February 2022”)
    * Focus: remaining work to finalize V1
    * **Scheduling survey should have been sent out by this meeting (feb01)!**
    * **[https://www.when2meet.com/?14369873-hVRTo](https://www.when2meet.com/?14369873-hVRTo)**


---


# ⏳ Timeboxes


### [Uniformity analysis #1571](https://github.com/gpuweb/gpuweb/pull/1571)



* Previously:
    * One week to review, expect to land next meeting or earlier.
* RM: David and Alan reviewed and approved.  No other complaints.
* DM: Have not reviewed.  Expect to review tonight. Will give answer tonight.
* 


### [Literal types #2227](https://github.com/gpuweb/gpuweb/pull/2227) 



* (Dneto: I think this implements the consensus.  It includes a bunch of renaming.  I went with “ConstexprInt” and ConstexprFloat in anticipation of adding at least basic arithmetic.  But the current PR only supports parenthesization and unary negation.  Unary negation is needed so we can write 32-bit MININT)
    * Dneto: Got a bunch of good feedback from Ben and Alan.  I’m reworking to fix/clarify the layering (“materialization”, defaulting, promotion).  Also interacts some with constexpr feature. 
* 


---


# ⚖️ Discussions


### [Should unreachable-code be an error or warning? #2378](https://github.com/gpuweb/gpuweb/issues/2378)



* RM: During the last meeting, folks stated positions, but did not reach consensus.  Might be useful to split motivations  There are two audiences: folks writing generators; folks hand-writing code. The motivations are slightly different. For hand-written code it’s easy to use comments to remove code from consideration.
* KG: In the abstract, yes, but want to emphasize the value in having dead code type checked.
* RM: Also in favour of that. 
* RM: If you have code you don’t want to type-check, then commenting it is better than making it dead.
* DN: In my mind, value type checking is about expressions and names and those are lexically scoped, but behaviour+continuity is truly about inter-block and inter-procedural analysis of flow itself; and that’s tightly bound to control flow.  It’s a false analysis to have a ‘discard’ in dead code to bleed out.
    * 
* JB: One view of type checking is it’s an approximation to the possible dynamic behaviours.  So in that view you wouldn’t want to type check in dead code.  But decades of practice does do that checking in dead code.  Behaviours and uniformity is trying to answer questions about control flow so it’s sensible to treat them differently.
* Ben:
    * Ben Clayton2:15 PM
    * fn a() -> i32 {
    *   return 5;
    *   discard;
    * }
    * 
    * fn b() {
    *   for (var i : 32; i &lt; a(); i++) {
    *     // ...
    *   }
    * }
* DM: Like Jim, want to understand why we should analyze flow in dead code.  We have a mandated model of when code is going to be executed. It’s an approximation.  But let’s use the analysis for what it’s for.
    * Example in a Vulkan implementation if code is actually dead, then it can’t violate any uniformity rules.  So a sufficiently smart analysis would see that.  But we have a conservative/coarse analysis.
* RM: Trying to understand one kind of analysis (value-type checking) is different from another kind of analysis (behaviour-uniformity).
* DM: My point is there should be no question about whether to analyze uniformity in dead code.  I believe we should not analyze behaviour or uniformity in dead code.
* MM: Point is either do both uniformity analysis and type checking, or neither.
* KG: As summarized by Jim, a lot is momentum/cultural. Even though in principle it’s different we’d be spending innovation points to do it.
* MM: We have previous experience/signal that type checking is done in dead code. But have no signal that uniformity analysis is done in dead code.
* RM:
    * fn c() {
    *   return;
    *   if (thread_id)
    *       derivative():
    * }
    * Think in this case the error should be reported. (In contraposition to Ben’s example)
* RM: We don’t have data on how often problems will occur and stop people from making an application.  It’s easier to remove a restriction later.
* AB: For Robin’s example, I’d say you can issue an error when that code becomes live.  And you haven’t lost anything.  To Myles’ position, we do have some signal; most languages don’t have a static check, and that itself is a signal. It’s not like the idea has never come up.
* JB: Question for Myles: In Ben’s code, what conclusion do you want about that loop.
* MM: Defer to Robin.
* RM: We have several options. (as laid out [https://github.com/gpuweb/gpuweb/issues/2378#issuecomment-1016834026](https://github.com/gpuweb/gpuweb/issues/2378#issuecomment-1016834026) )  (option 2 in the issue):. Check dead code uniformity but not let the behaviour result of that dead code flow outside.  I slightly prefer option 1, and say control flow is not uniform in this case.  Option 2 feels better to me than option 3, where we don’t check the dead code at all.
* DM: Comment on Robin’s example.  I would be very upset if it would be an error in this case.  Uniformity analysis would give me an error not for my program, but for a hypothetical program where one of my statements is ignored (essentially). So, why.
* RM: Have found it convenient to have more checks apply to code that isn’t currently in effect. Keep circling back to thinking of behaviour+uniformity as a kind of type-checking.
* Ben: Have two arguments:   use return to intentionally cutoff analysis; another view is check more than.  What’s going to annoy more people.  Presume the developer is experienced and knowing what they are doing.  My stance: annoy fewer people at potential loss of having a few people lose a feature.
* MM: Here’s a program where the compiler will complain about even though something essential is not exercised.
    * struct Foo { ... } struct Bar { ... } fn d() { return; var f: Foo; var b: Bar; f = b; }
* JB: "annoy the fewest people" has a lot of appeal, to me
* GR: I agree strongly. I don't feel we've universally applied that philosophy
* 


### [[wgsl] Support compile-time-constants (constexpr) · Issue #1272](https://github.com/gpuweb/gpuweb/issues/1272) 



* Where are we? What do we need?
* AB: Think this is tied up with literal typing unification.  But have a bunch of decisions
    * E.g. which types: scalars, vectors, sure.  Structures and arrays interesting.
    * Have broad consensus
* MM: Should be the same set of types and operations that overrides support. If you have decl foo = 1+1;  // debate over whether ‘foo’ has concrete type or not.
* MM: Think should decide same way for override and constexpr. 
* KB: override must have concrete representation since you can set it from the API side.
* ..
* BC: think override should have concrete type.  Google thinks a const definition _can_ have a non-concrete type, to allow refactoring into decls. E.g. declare pi once in your shader, and use it in other non-concrete expressions, with various other types.
* MM: you don’t have substitution ability for override.  Design tension with making similar language features work similarly.
* KG: As first approximation, yes, make them have same feature set. Break away from that when it’s justified.
* BC: Not clear that const and override have to be very similar. Can see override and let to be very similar.
* KG: Const to feed into override, not the other way around.
* BC: Three tiers:  const  (shader-creation time), in const domain;   override (pipeline time) can use const and other overrides;  runtime-tier (let and var). Anything below can use something from above.
* MM: Had early proposal: that if we land constexpr, we can remove let at module-scope. (Which Ben was saving for later)
* BC: Clearly runtime types need a concrete type.  Pretty clear overrides need concrete type (because feed values from API side). Need a contract for that.  The only place we don’t necessarily need the concrete type are the ones at shader-creation time.  Google stance is not that all const are typeless.  But instead, much like var and let, you can specify a concrete type, or not.
    * const pi : f32 = 3.14… ; // e.g. this make pi have an f32 value.
    * Const abstractpi = 3.14….; // is the more abstract type
* KG: Think we all agree that const can have concrete types.  Can land an initial constexpr which _requires_ concrete types. And can enhance later.
* BC: Don’t think it’s any additional work to support the more abstract types for constexpr evaluation.  
* MM: Ben, I think you convinced me. We’re both comfortable with allowing abstract types for constexpr.  Convinced by the statement that it’s about “storage”.  Storage for constexpr is in the compiler, not the GPU memory.
* MM: Should still be possible to put explicit declared type for const.
* KG: Need beginnings of spec text for this now.
    * Grammar
    * Annotations of builtins

BC: Need agreement on whether override…… (will provide example)


---


# 📆 Next Meeting Agenda (15m)



* Next week: VF2F!
* Proposed schedule:
    * Day 1:
        * State of the spec (dneto)
        * Literal unification
        * Constexpr
    * Day 2:
        * Uniformity analysis (and related issues)
        * ~~Atomics~~
        * ~~Smaller data types investigation (native vs emulation)~~
    * Day 3?:
        * Air time for Google, Apple, Mozilla and anyone else to present features or changes they want for V1
            * Ideally each company could have some slides/materials prepared if there are still changes they wish to make to WGSL
* Thoughts?
* 
* Corentin:
    * Triage all "[wgsl, no milestone](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone)" issues. Please someone make a pass before the VF2F though.
    * Resolve as many of the v1 issues as possible, so all that's needed is to write the resolution in the spec!
* …
* Topics:
    * MM: One question to answer: “What is our plan for how to not break stuff when we add things to the stdlib, e.g. namespaces, or Features…” I think it’s important to have a solution by the time the first browser ships.
        * DN: #600 was the issue I filed on this, which we resolved to have features, and maybe namespaces but we never went back to that.
    * MM: Another topic: “How to encapsulate/associate resources with a module without them colliding in name?” E.g. two modules with different resources, but using the same name, like “u_tex”. [https://github.com/gpuweb/gpuweb/issues/2426](https://github.com/gpuweb/gpuweb/issues/2426)
    * Burndown of [https://github.com/orgs/gpuweb/projects/2](https://github.com/orgs/gpuweb/projects/2)
* …
* DN: What’s the publication pipeline/status of the spec?
* KG: We can ask Francios Daoust. I will write up what the pipeline is like and where we are and report back.