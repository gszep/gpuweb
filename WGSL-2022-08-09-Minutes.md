# WGSL 2022-08-09 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-08-02 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1uYo4dp4hSzpKwKOx0raagrtyYkM_M8ybDih7NXG1OSc) 

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
    * Ben Clayton
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
    * Rafael Cintron
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
* Mehmet Oguz Derin
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson


---


# 📢 Announcements


### Call for Editors



* Chairs are wrapping up the Call for Editors. Thanks everyone!
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



* WIP:  Tooling and tweaks to make the grammar friendly for recursive descent.
    * [https://github.com/gpuweb/gpuweb/issues/3286](https://github.com/gpuweb/gpuweb/issues/3286) 
    * Key thing is to refactor expression rules to push “unary_expression” out and to the left, then hoisting it up.  Need to be careful to retain left-associativity in this case
    * Current state: [https://github.com/gpuweb/gpuweb/files/9293278/wgsl.recursive.txt](https://github.com/gpuweb/gpuweb/files/9293278/wgsl.recursive.txt) 
        * Still has epsilon-rules in some places. (epsilon = match empty string)


---


# ⏳ Timeboxes (until 11:20)


### [Allow optional ()s on enable? #3207](https://github.com/gpuweb/gpuweb/issues/3207)



* DS: Seemed like a nice quality of life thing
* DN: Against it, Ben also against it. Future world thing where thing looks like expr or not.  E.g. import.
* KG: Prefer not doing it.
* DS: Close as rejected.


### [Precision requirements for atan2 appear to be too restrictive for some backends #3209](https://github.com/gpuweb/gpuweb/issues/3209)



* GR: correct we do not have those precision guarantees. Flush denorms. Don’t have explicit atan2 intrinsic. 
* DN: So, does MSFT have a statement of rules of what the functional spec is?
* GR: Not beyond what was just said.
* DN: So case by case basis and RH will try to come up with a rule that seems right
* GR: Are you familiar with the … spec
* TR: Usually only have this if there is an instruction but there isn't’ for this it’s expanded
* MM: Seems there aren’t that many floats so we could check all of them, maybe not on every device but on every member
* GR: Could piece together based on … and function spec
* DN: If there are expansions we can support that, would be really useful.
* KG: If there is an expanded def of what atan2 becomes we’re good to go. If there isn’t we’ll do testing. Sounds like there isn’t one.
* GR: Don’t believe it’s document but it’s fairly independent
* TR: Expansion is done in DXC which is opensource
* MM: DX11.3 spec doesn’t mention atan
    * [https://microsoft.github.io/DirectX-Specs/d3d/archive/D3D11_3_FunctionalSpec.htm](https://microsoft.github.io/DirectX-Specs/d3d/archive/D3D11_3_FunctionalSpec.htm)
* TR: Because there is no native instruction
* JB: Link to DXC expansion?
* TR: Will add one
* [https://github.com/microsoft/DirectXShaderCompiler/blob/b3101a2feb078a9da9c78b44d9b61c8c312b646e/lib/HLSL/HLOperationLower.cpp#L1530](https://github.com/microsoft/DirectXShaderCompiler/blob/b3101a2feb078a9da9c78b44d9b61c8c312b646e/lib/HLSL/HLOperationLower.cpp#L1530)
* 


### [Accessing an "invalid memory reference": Trapping behavior · Issue #3272](https://github.com/gpuweb/gpuweb/issues/3272)



* (KG: This might be a larger issue, but I want to timebox it regardless this week)
* MM: Idea is compiler team things it’s valuable to have trap behaviour as acceptable as out-of-bounds access. Two reasons, 1) some devices can do this trap faster then clamping. 2) If you can guarantee out of bounds causes program to stop then subsequent accesses that are guaranteed closer to in-bounds don't’ have to do bounds checks. Only way to get to that access is if the first one passes. Some situations where if the code is the right way the perf could be faster.
* AB: Don’t understand why there is a refactoring that is available that isn't’ to clamping. Why not remove the second clamp just as easily. Missing something here. Talking about compiler team, does that mean metal has Trap? Don’t see it in spec.
* MM: Compiler team means the webkit compiler team.
* KG: How is trap implemented
* AB: Right, if we emit MSL do we have to bounds check, msl own’t do that
* MM: Right
* AB: So we can’t emit trap?
* MM: MSL Doesn’t have this behaviour, so you do it yourself.
* KG: To AB first question, how is trap faster given clamp tells you if out of bounds. Think the answer is in order to get same perf benefits you’d have to trap into parallel set of code that does the same behaviour on one side if it trapped out of bounds it keeps checking, if in bounds it stays in fast path.
* JB: Optimization in first case is you know that where you are in the code all the dominated traps have passed so like inside conditionals. With clamping you don’t get info on those previous operations. No indication based on where you are in control flow what those clamps did
* KG: Well it’s as-if clamping, so could branch
* JB: If you did it with branches, yes, but don’t have too
* KG: This sort of thing, in the trap case, you are branching. So the way to get the same optimization using clamping is to just branch and have fast path and slow paths where clamp had to engage. Then fast path where you don’t clamp at all. That’s why trap is attractive because you only have golden path. Other code where clamp failed doesn’t have to be generated
* JB: Don’t think trap is a branch. We’re doing this on non-uniform values. These trap behaviours can’t possibly be execution mask implemented as it’s terrible. Idea of having fast/slow path only makes sense for full warp branches.
* KG: … ?
* JB: Would need info from MM but believe talking about something that isn't’ just branch op. You do something special.
* MM: The behaviour proposed is trap would not affect uniformity analysis. Two ways to make it so. 1) only trap if nothing below you in control flow is sensitive to uniformity 2) do it like demote to helper where thread doesn’t end but the IO is predicated.
* KG: Right drop all writes and sanitize reads. Keep executing some amount of the code.
* JB: But you have to run both slow and hot path?
* MM: If every other thread traps, yes. With trap there is no slow path, it’s either the same as fast path or super-return kill in which case threads are along for the ride. If every other thread traps they’re the same, if they all trap then kill the warp
* KG: Can this be done as demote to helper?
* AB: Only in fragment
* JB: Not clear how this gets implemented.
* KG: Possibly in office hours

    DN: Intuition where the dominating check gets a win is where same analysis could be done and could derive you dn’t have to re-clamp. Because you don’t need extra trap checks is same as not needing extra clamp. Myarray[i], myarray[i / 2] once clamp for i you can re-use in ½  rather then re-clamp and can thread the index value. If there is monotonicity in the clamp


    JB: Was saying if you could omit, but you’re saying thread the result of the clamp.


    MM: Confused about that. In these two lines with array access of i and i/2. First is clamp, so some temporary from clamping i, then take that temporary and divide by 2? \
DN: Later where you have i which is monotonic smaller you can use the clamp value.


    JB: Not right, second i is in bounds and we can’t touch out.


    MM: Right, that’s not correct.



### [wgsl: limit nesting depth of switch statements #3294](https://github.com/gpuweb/gpuweb/issues/3294)



* DS: CAme from a clusterfuzz bug with deep nesting of switch and fallthrough.  But FXC doesn’t support fallthrough. So Tint uses inlining to get around it and blows up (codegen time).  So looking at options here.
* JB: Is it exponential?
* DS: Not sure. We’re using top-down expansion. Might go better if bottom-up?  But 10 is fine. Less than 1s.
* MM: But also codesize is a problem.
* DS: Has only been a problem on FXC. DXC doesn’t have this problem, nor any other 
* KG: Seems fallthrough is ok if there’s no body.
* DS: One suggestion is allow fallthrough if there is only no body.  And Myles had another suggestion is remove fallthrough.
* MM: fallthrough is often a mistake in C source.  Case without a body is probably ok.
* KG: So one option is remove fallthrough as a feature, and allow case clause or default empty.
* AB: clauses require a body.
* …
* KG: How did we land there?
* DN: Swift has this. Could go multiple ways, things kinda worked out well.
* KG: A practical approach.
* MM: Is it worth discussing the last statements is not a return or break. If there wasn’t fallthrough
* KG: Question of unfamiliarity for folks who expect fallthrough, Javascript does this?
* JB: Rust as well doesn't’ do fallthrough.
* KG: Favourite is to** add default to list of things for case**.
* MM: Have to worry about constexpr variable named ‘default’
* KG: It’s a keyword
* **DS: Also remove fallthrough_statement and put `fallthrough` in list of reserved words**


---


# ⚖️ Discussions


### [invalid expressions in const-expr and override-expr should be error (no later than pipeline creation time) #3253](https://github.com/gpuweb/gpuweb/issues/3253)



* JB: Substituted one problematic criteria for another one. We defined how out of bounds works, so we can’t say this is how wgsl works to define the cases. Need to make judgment call on what would be likely used on purpose and what by accident in reasonable code.
* MM: After talking about trapping, our logic came from if you immagine trapping being added then one consequence of out of bounds is program stopping. Thought that was strong enough to have implications on this issue. But for something like arcsin(2) there is no way… coming from idea we accept trap
* JB: Should we not trap on arcsin(2). If we have trap we should trap on that, it’s meaningless, it’s a mistake.
* MM: Tough pill to swallow. Can’t articulate why.
* JB: LIke idea of aligning trap and static errors in constexpr.
* MM: One thing important with trap, no trap is ever forced, no situation where compiler has to trap. With arcsin(2) would compiler be forced to trap?
* JB: Assume similar language for domain errors as out of bounds where you can choose 1 of multiple behaviours. Think this is ok, if you stop execution and tell the user about it. Not the same category as giving various answers
* AB: How do you tell the user? Sounds like pipeline keeps working and gives, something. Feels worse from portability argument
* KG: Have option as a UA but not required. Behave like you did something weird in GLSL and get blackout bliz
* KG: One thing for our heuristic, if it’s easy to choose, two behaviours user might want and we don’t know which they do, if we pick our default so user becomes aware and can fix it. For out of bounds access at compile time, get error and can tell them to add clamp. But they don’t get wrong behaviour at compile time. Strictly can’t share the code and can’t expect inlining.
* DN: If can summarize, then bailing out loudly is a good option as then the user can choose how to react. Want to +1 what JB wrote in issue which is that “using the behaviour on purpose”. To me arcsin(2) isn’t sensible and wouldn’t be used on purpose. Those aren’t in the domain of those math functions and the computer should approximate the math. For a runtime vs compile time inconsistency for me one guide is if you get puzzled use a constant value and the compiler will tell you if it’s bad. For the math domain things, hopefully heading to world with gpu having nan and full support. CPU will have nans and we should find those cases. In future date we can cut off old behaviour and make it more strict. Think we should head towards world where we enforce what IEEE gives us.
* AB: So, sounds like we would have compile time error if make NaN in const expr. IEEE calls that a result. Can buy where one domain produces another but don’t think you can get to a place where it isn't’ an error without it being weird. Either don’t use NaNs or have full nan support. We don’t now because we wanted to be consistent with abstract float.
* KG: Like idea of requiring compile time NaN is error. Think that’s a cool way to get most of the benefit.
* AB: THink it’s OK but calling it an IEEE thing isn’t right as it isn't a problem in IEEE.
* KG: Think it’s ok, we don’t have IEEE floats, at compile time we still don't, just a different flavour, which is NaN is compile time error.
* MM: So eventually, will add IEEE 754 compat extension and that extension would relax this compile error?
* AB: Unlikely we get to full IEEE
* MM: It’s an extension
* AB: Don’t think there is a lot of hardware to get full, caveats on it.
* KG: Consider a more float option
* MM: Could go way way further and still get a bunch of hardware.
* KG: Semantics if we call it IEEE.
* KG: In favour of strawperson which does compile time NaN generation as error
* DN: Think already on board with out of bounds is an error
* KG: Want to protect 1/0 ability
* MM: Little confused. Coming from expectation that in WGSL will define arcsin(2) is something that isn’t NaN
* DN: Don’t like that but can litigate later
* MM: Because of the policy just describe, if a compilation could make a NaN that’s a compile error. If we define arcsin(2) to not be nan it’s portabile and well define it wouldn’t be hit that conditional and be a compile error
* KG: Might want to use that at compile time
* AB: You’ve picked some special rules to follow, why out of bounds and not arc sin
* MM: Philosophy, if many possibilities of behaviour, lots for out of bounds, because so many options that’s the boundary condition.
* KG: But if always one solution?
* MM: That’s just how the language works. If one possibility is the program stops running, then pretty strong hint something is wrong.
* DS: So we define arcsin as doing anything?
* AB: We sort of do
* MM: Do it like division?
* AB: Some hardware loads away NaN so it won’t exist.
* KG: Not proposal. 2 things, compiletime and runtime. Runtime has no NaNs, compile time has NaNs and if you end up assigning that NaN out to a constexpr result that’s an error.
* AB: That’s fine, but not what MM is saying, MM says give answer like 1 or 0
* MM: arcsin is already really slow, so checking if > pi or &lt; -pi doesn’t make slower so it’s a win
* DN: Reduces readability as it’s a trick in the spec.
* JB: Not something folks should do on purpose
* KG: But have to be aware in practise
* MM: What is it
* JB: Depending on arcsin on out of domain inputs, Folks should not do that
* AB: What is cut off between slow and fast. Is tan slow or fast? Any trig?
* MM: One criteria, is does HLSL have an intrinsic for it?
* DN: atan2 vs atan, that’s a sharper edge
* KG: Some time just have to choose and swallow doubt and pick something.
* MM: Could also say, does it take longer then 100 cycles.
* KG: Even harder to litigate this then just decide atan2 is slow and tan isn’t. We should think about this more.
* AB: Should also consider portability is being raised but trap is making things less portable. Want to know where we pick and choose
* JB: Withdraw what I said, as assuming it was a hard stop, if program continues to run then it isn't’ a portability problem.
* MM: Can defend portability, there is already a bunch of options about what might happen with out of bounds and this is an anti-pattern in wgsl. Taking something that in other languages is worse behaviour, in others it’s undefined, and we’re talking the set of possibilities and making them smaller. Have a list of 7-8 behaviours for out of bounds, and proposing making that 8-9 which is better then other languages.
* AB: Except for spooky action at a distance where I’ve done an discard and do this as well. Extra side effects, not one access.
* KG: We’ll think about it.


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* See also:
    * [wgsl: pointer-to-workgroup in user-defined functions #3276](https://github.com/gpuweb/gpuweb/issues/3276)
* MM: inout is pretty cool!
* DN: BC is away and want to talk to him about proposal.


### [wgsl: language evolution: managing rollout of core (essentially sugar) language features #3149](https://github.com/gpuweb/gpuweb/issues/3149)



* KG: Rollup with links to other discussions. In V1 so will get triaged. Sounds like in short need solution to 1) do we need to figure out for v1? Seems like we should, but depends on if we feel like we need to do something different or if we can keep doing what we’re doing
* MM: Urgent question is if user defined functions shadow standard library, if answer is yes then don’t need to decide now
* AB: We agreed on shadowing predeclared builtins. Think important part is compat going forward and settling on maintaining backwards compat. If we agree on that then don’t have to worry about it.
* MM: If there is a breaking change it has to be opt-in, standard on web platform.
* KG: THat’s what made me satisfied, we’d be using opt-in for breaking, for non-breaking weight on scale and decide if they need an indication. User defined functions shadowing makes easier. 
* DN: And also shadow predeclared types like i32 and f32. In doing that and removing keywords, couple PRs posted and a technical problem comes up where mat2x2&lt;f32> how do you resolve as a type constructor or function call. Need to invent words. Types as well is main point.
* JB: Maybe in the future we define generics which might shadow wgsl types
* MM: Could imagine that
* DN: Need name for thing with type parameterization which needs a type.
* MM: The stdlib can have overload, so if I make a foo and stdlib has a foo, do i shadow all of them?
* DN: Yes
* AB: Yes.
* KG: Today yes. Answer is probably to allow overload which we do in the stdlib anyway. How much work is user defined overloads?
* JB: Don’t think that’s the problem, question is if you get confusion as some calls go to user function and some to stdlib. Would be confusing. If there is a redefinition then that shadows all overrides or things from other scopes. Mixing depending on types of arguments would be bad.
* MM: Point of shadowing is the author makes function and calls and won’t accidentally call stdlib. If we don’t fully shadow then adding to stdlib might take over in precedence rules.
* KG: Do we have precedence resolution?
* MM: We don’t because the only overloads are in the stdlib and don’t need general system, if decide users can do it then will need a general system.
* KG: If decide to never support, like said functions never coerce their types. Then overload resolution is find the one for the type.
* DS: What about abstract?
* KG: Can you name it
* AB: Through const
* MM: Imagine sample function taking offset int and offset float. That’s the scary part.
* KG: Don’t want that anyway?


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, August 16, **11am-noon** (America/Los_Angeles)
* 