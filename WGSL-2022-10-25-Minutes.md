# WGSL 2022-10-25 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** **(non-APAC!)** Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-10-18 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1bZst9yqg7SvMTwexbpwqo-wwnmaFEfYv1Ri7-y2iRvY) 

 

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
    * **Dan Sinclair**
    * David Neto
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
    * Greg Roth
    * Michael Dougherty
    * Rafael Cintron
    * **Tex Riddell**
* Mozilla
    * Erich Gubler
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
* Mehmet Oguz Derin
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



* CG charter: [https://github.com/gpuweb/admin/pull/19](https://github.com/gpuweb/admin/pull/19)
* [wgsl: Fix how we say concrete left shift must not overflow (if known before runtime) #3539](https://github.com/gpuweb/gpuweb/pull/3539) 


### More APAC meetings during Nov and Dec



* We will be doing more APAC meetings during Nov and Dec.
* **We are also moving APAC slot one hour earlier** (4pm Americas/Los_Angeles), though this will drift again in early November with USA’s timezone change.


---


# ⏳ Timeboxes (until 11:15)


### [Runtime behavior of NaN-producing math functions #3365](https://github.com/gpuweb/gpuweb/issues/3365)



* KG: Waiting on MM PR
* MM: Still waiting. Will try to make proposal before I leave.
* DS: So how do these things interactive with compile-time behavior?
* MM: Resolution was (compromised) e.g. atan2(0,0) is compile-error.
* DS: I think we haven’t written this down?
* MM: Split issue: Runtime vs compile time. Compile-time is resolved, interested in runtime now.


### [wgsl: grammar for LHS is ambiguous about grouping its subexpressions, affects type checking #3530](https://github.com/gpuweb/gpuweb/issues/3530)



* (PR at [wgsl: Move * and & from lhs_expression to core_lhs_expression #3531](https://github.com/gpuweb/gpuweb/pull/3531))
* AB: DN is away this week.
* KG: Can table if we need.
* JB: It is the case that treating * as binding more closely is more useful for WGSL because it can't store pointer inside data structures so having the things to get stuff out of the data type member won't be useful, yet. However, sense at Mozilla is it would be extremely confusing to have precedence of * and postfix expressions different from other languages that use * op that way (c, c++, rust) all have same precedence. Will have people coming from those places using language and think it will be a foot gun. Unfortunate to have it be the useless one and require parens, the audience will expect that already. So, our feeling is to follow precedence. If we wanted it to be postfix operator, that would be cool, but want something simple for it.
* AB: This pr was to make consistent as opposed to intending to do that.
* BC: Not sure if it was intentional for DN to do it the other way around.
* JB: Maybe just a mistake
* AB: Fine with it matching other languages. The issue was LHS and RHS being different. 
* KG: So, **resolved in favor of doing things the way other languages do it (C, C++, Rust), just have to fix the spec**.


### [Allow signedness to differ between integer texture builtin arguments (C template) #3536](https://github.com/gpuweb/gpuweb/issues/3536)



* BC: Have discussed before and got consensus on post-v1. Since then, abstracts appeared and have been trying ot implement some spec changes for texture builtins. Integers being signed or unsigned. Thought it was odd that all integer params of builtins all have to be signed or all unsigned, no ability to mix. Implementing this was really awkward. Translating from spv -> wgsl, spv doesn't care about signed vs unsigned, for WGSL have to be same across the board. To do without casts all the time is terrible. Bunch of tests very difficult to do. Just trying to write, landing this and spent a few days writing WGSL and diagnostics are confusing as don't expect coord sign to affect level of detail sign. Orthogonal things. Keps the spec simple, to tie together. Don't think we could change this without breaking shaders, gave example of materialization failing if each type is independent. Contrived example, but still impossible for us to do without possible breaking changes. Would like to propose making a separate generic type per thing and allow each to be signed/unsigned.
* JB: The way sign is bound together and spec simplification makes sense and could be confusing behaviour. Should consider just being bold and making everything signed all the time across the board.
* MM: Long discussion, months for array indexing sign vs unsigned. Was hard to get consensus there.
* JB: Was that discussion before or after abstract?
* MM: Before.
* JB: Does abstract int change that discussion?
* BC: Not entirely. Abstracts naturally decay to signed, have made decisions like the query methods for number of levels are unsigned. Get mismatch from what you type and hwat is return. Lot of concerns come from there. Shaders to be fixed up are a bunch of that. People take return and miss match.
* JB: That's an argument for mixed
* KG: This proposal was to boil the ocean. Just have signed for … ?
* BC: Went other way, query methods went from signed to unsigned. 
* KG: Think consensus thing is to allow anything for these. Just feels weird given we don't normally freely coerce between signed and unsigned except when using builtin seems hypocritical. If wrote function yourself would have to pick one.
* MM: To restate, if i wanted to call textureSmaple and pass unsigned i just do it, it works. If i want to call textureSample and pass signed it just works. However if i wrote a wrapper for textureSample this could not be said and that's unfortunate.
* AB: Bigger complaint seems to be we don't allow overloads.
* JB: Don't want to write both copies of the function. It's not that i wish for overload, i wish for implicit coercion. We're in a different place now with better literals. Feels like lot of discussion were around code I could write and had to do stuff around out. Things go more smoothly when you just have one thing. Don't want to re-open everything, getting close tot he end. Disentangling those arguments makes sense but seems like, it's only going to get worse in the future. Seems ike we could simplify stuff and end a whole lot of questions if everything was signed except if you have representation things.
* MM: One argument, there is no texture for which that upper bit is meaningful. So, feel like it doesn't' really matter and let authors write what they want, if it's unsigned it's fine. The same can't be true for user code. That wrapper function could do anything, could be doing math in there. So the user code might be sensitive to the top bit, but the texture function isn't. Should meet authors where we are and accept this PR.
* JB: Not concerned about Naga implementation as far as that's concerned.
* BC: At this stage, trying to change these again, now in more relaxed state, constraining to signed/unsigned at this stage would be unplatiable. Lots of shaders out there that work. Preference is allow signed/unsigned for each param.
* KG: Can we just resolve to do that and also resolve to feel appropriately sad it isn't as pure as we'd like
* JB: I'm sad.
* KG: So, accept this PR.
* AB: Have to make PR.
* KG: Resolved.


### [Add draft text/wgsl media type registration. WGSL uses UTF-8 #3542](https://github.com/gpuweb/gpuweb/pull/3542)



* AB: Happy someone else is happy. Very little opinion, just get it done. Don't know about BOM.
* KG: Sounds like no-one is opposed.


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* KG: Wanted to ask, do we have more to do here? Have static analysis, did resolution for pointer to work group and pointer smarts.
* AB: Talked about future work and if we want c-like aliasing in the future so left open.
* JB: For other discussion, left spec as is, so we need restrictions for now. When do Clayton transform maybe can relax these restrictions.
* KG: Will close, if we want to open for post-v1 can make a new issue.
* MM: I am sad. (about alias analysis being needed indefinitely into the future.)
* 


---


# ⚖️ Discussions


### [Uniformity analysis is far too strict #3479](https://github.com/gpuweb/gpuweb/issues/3479)



* JB: Been in contact with Brendan about getting access to shaders in their corpus which provide useful problems that we can use to improve uniformity analysis. In the middle of drafting PR for an unsafe block. Talked a bit at office hours last week, what it amounts too is to say that the graph that uniformity analysis consumes when analysing a function would omit edges from code occurring inside an unsafe (not the real name) blocks. Then, this would be something useful for folks who want local opt-out and useful for folks coming from other languages where can't make local adjustments. Can just wrap whole function body if can't do better. Definitely then we should press on with making analysis more accurate and perhaps adding annotations to allow authors to express preferences but would like that in separate PR. Don't think this is right issue to discuss more detailed proposals like unsafe, annotations or analysis refinement that don't require user intervention (like tracking vector components). Want to see those filed issues and discussed separately.
* AB: To be clear, new type of statement? \
JB: Yes block.
* KG: Expression or statement
* JB: Statement. WGSL is not expression language so we have to pick
* AB: Preference is as few escape hatches as possible. If we do statement, just do that, if we do attributes just have that.
* JB: Let's talk about that in context of specific proposals. That question does make senes to talk about as it's how we get Unity going again.
* MM: First, making uniformity analysis more fine tuned is a good idea nad we should do it. Second, a little curious about the mechanism for this opt-out we talk about. Mentioned it could be a statement/block, why not have the mechanism be larger scope where it could be an extension for the whole module or alternatively, could be offline that shader author runs if they want to, but don't run if don't' want too. Why statement and not larger.
* KG: Reason for smaller scope is to retain uniformity for the rest of the function calland only opt out small portion known to be bad. Admit some of this feels a bit too obvious to me coming from rust as it's straight forward how unsafe works. If not used ot it could see how it isn't useful. In rust can do let foo = unsafe { bad thing } and then everything else assumes that thing is safe but you still get borrow checking and all the other safety guarantees. For WGSL you say this one texture sample, yea it's non-uniform, I know, but keep going and check everything else. The key is for it to be small scoped, that's the benefit, having it be too big a hammer, folks just mark whole sections as unsafe, can't make ti smaller without function composition is less useful.
* KG: As for why have step we always run, i'm borrowing heavily from Rust, in that I like tha Rust forces you to do these things, it isn't an offline check. that said, Rust needs that safety for it's guarantees. I think it's useful to have folks run everything through it and opt-out small sections. Think that moves ecosystem forward and is worth spending innovation points.
* JB: Like that explanation.2 reasons for part of spec and not lint, 1. If we make it lint, in practice it will be used rarely. Being manipulative through technology design as I want folks to run this analysis. Why, we have seen (Either BC or AB ahd examples) of FXC behaviour being .. by removing real bugs. By helping users, I think we're providing a benefit of making users alert to the problems they're having. Think we're catching real bugs and think we can have a good effect on ecosystem by pointing out these problems. 2. Was talking to … a developer of Mesa about barriers and non-uniform control flow and what she said is barriers can become unsynchronized. This barrier is unsync'd by another barrier elsewhere. If using internal to implementation can upset that and can upset driver. Think folks in the future will mass convert compute shaders, which I believe is not what Unity is doing yet, but when we have folks bring large bodies of compute shaders and i think we may be in a position of not being able to give it to them because we require barriers to be correct to make implementations sound. So, I think it may not be possible to opt-out in compute. Fortunately, graphics is only derivatives, and compute is only barriers, so for now opt-out fragment but not compute.
* MM: One thought about a block of unsafe uniformity-ness, a potential problem, not necessarily a deal breaker, is if you make a change in uniformity that can have an effect later in the program. If i have unsafe block, don't want to ignore the effects of code in the block from the perspective of future blocks. As, i do something in unsafe block that causes compile error 100 lines down outside block. If goal is to help authors, then design like that doesn't help authors.
* JB: I think unsafe only makes more programs permitted, don't see it making fewer permitted.
* KG: If it changed the locality of errors, then would see that as a core problem of unsafe block proposal. It's contingent on that not being a problem and us doing this in a clean way.
* TR: Have considered to mark variable as uniform-enough instead of block? If it could be marked in a way as 'user marked' as opposed to analysis-marked. If analysis can't tell, and you say it is, or it's uniform enough for texture sampling, because uniform across quad, then could propagate that, and could propagate to flow control and make sample valid in that context or derivative. Has that been considered?
* JB: Yes, that would be an additional thing. The way I'm seeing it right now is if you only have unsafe blocks you would have to insert unsafe block at every use of variable. If you have var and awant it treated uniform would have to wrap in unsafe, which seems silly. Would be nicer to put attribute on variable and have it just understood. Unsafe block is big hammer. Attribute is a refinement. Goes contrary to AB's wish of not having multiple ways of expressing this. Recognize that value and agree I am contravening it.
* AB: If it were just uniform attribute on function call could have rules on where it goes. Like function call or texture sample or barrier. Should consider these designs before having 1 proposal and the merits of each.
* JB: Advantage of block, i think, over attribute on calls, (It's calls that make the requirements, so attribute on call handles all cases I think) but what you'll do is make unity decorate every call with this attribute instead of just putting one block around thing and opting out. Effect would be the same.
* AB: there is a range. If applied to user defined function maybe nothing applies, lots of ways to play with it. Maybe getting too far down design path. Don't want to see PR where we haven't discussed, lets make an issue and talk about it.
* MM: How is this going to work when we add new features ot fragment shaders that are security sensitive. Will we run analysis twice, once in a way that honours unsafe and one that doesn't? How's that going to work?
* KG: Expecting eventually to formally respect the multiple scopes of uniformity, so may run analysis twice, or just track more stuff in passes it does.
* MM: There is a natural idea to have unsafe block take a param, only one thing at the moment (unsafeDerivatives) but make more in future.
* JB: One thing about unsafe in rust is they aren't a free-for-all. All of rusts restrictions apply, just certain operations you're additionally permitted to do. if we had things we have to enforce for soundness the unsafe block would always check those. It does seem like that's feasible.  Although it does complicate the implementation I had in mind
* MM: Could see design where we run ignoring unsafe blocks and then for all errors see if they come from unsafe blocks and then turn into warnings.
* AB: Don't happen to have an example of feature that introduces security risk that requires uniformity do you?
* MM: Not off top of head, Think there might be something with quad operations but not certain.
* TR: What about barrier case.
* <&lt;< Earth quake >>>
* TR: If you just have unsafe block around barrier, and it is really not uniform and end up with situation where you hang gpu or have driver issues or sync wrong barriers, isn't that bad and we want to avoid at all costs.
* MM: That is bad.
* KG: Hanging gpu isn't a problem, prefer not to happen but not a requirement.
* JB: Does WebGL allow hanging gpu?
* KG: Yes. Javascript allows you to hang CPU and exhaust memory.
* MM: Was clamin a few weeks ago that improper use of >.. &lt; can cause to reboot computer. 
* KG: Would be bigger problem, but introducing TDR is not a problem.
* AB: Have seen hardware where you kill X and have to go to console and reboot X. Have seen reports but haven't seen myself.
* KG: Goal is sufficiently rare and exceptionally and concentrated on old hardware. If new hardware that reliably creates blue screen don't want to ship.
* MM: From our perspective, any situation where it is possible to go to website and get locked out of computer, the threshold for that, acceptable % of users to hit that is 0.
* KG: Unfortunately can't make the requirement be 0. But the goal is catastrophically few people.
* TR: Guess can see the difficulty in marking values in uniform tag/template but that seems like safer approach in some ways.
* KG: One thing, this is the chance, maybe, for anyone who want to convert compute shaders to tell us it sucks. We're working on graphics, but if people expect to convert shaders and make as unsafe we wont' be able to give them that. So, we need to known now, or we need to figure out how to do it backwards compatible.
* RC: Making clarifications, when KG says hang GPU that's strong statement. Ok with me is we hang GPU and browser/os can recover. Don't lose data. On Windows we have TDRs were yes the browser hangs, but can close the browser but you can open it back up. Don't have to reboot the machine. As TR alluded too, if you say unsafe and then write code that access buffers or textures that belong to another domain running in gpu that's a case we never ever want to allow.
* TR: This isn't' unsafe, just uniform analysis.
* RC: Sure, but brings up the point of if you unsafe can you be trust me trust me.
* TR: Just worried about code gen that wraps every access or every barrier. When they go to investigate barrier says no good and they just add unsafe.
* MM: Can imagine architecture which has broadcast and all threads get one value and separate instruction which isn't broadcast but does memory access, compiler things some data is uniform but it actually isn't. Think that would be exploitable. Compiler makes wrong decision and can read data from wrong place.
* JB: Nothing in spec now where uniformity changes how code is translate. It just gives error messages or rejects program. Does not carray conclusions through to code gen.
* MM: In MSL we have concept of uniform annotation where programmer promises it's uniform and if it isn't it's UB. To emit that in MSL we'd have to ignore the unsafe blocks to know what is proved uniform. What the WGSL author says is uniform but we can't prove we wouldn't emit that code.
* AB: Couple points, Raph has commented and has compute shader which I think would be satisfied by workgrouBroadcast. INternally discussed maybe not allowing this for compute. When have arrays of bindings, vulkan has by default dynamic uniformity requirements which lead to undefined access if you do it. Few ways around it when we get there but as we add new features more places where uniformity comes into it.
* TR: If you're making values instead of doing the unsafe block you could, in an implementation, do something to more safely execute by forcing it uniform by doing a broadcast. Could get around the barrier situation by just wrapping each barrier.
* KG: One thing wanted to ask about SPIR-V requirements, it sounds like one of the questions that is important is when reconvergence happens. If you can use branches/merge blocks to force reconvergence, it sounds like AB is saying don't get partial reconvergence until get total reconvergence. Is that a correct characterization and does it makes sense to ask for SPIR-V spec clarification in that direction.
* AB: That is the correct understanding and we can't clarify without new functionality.
* KG: Want to make it more clear in spec. If read spec and combine with how you think it works you come to conclusion that derivatives converge and if they don't we should make that explicit. I'll file a spir-v issue.
* MM: Sounds like where we are is 3 things, 1. we should make the uniformity analysis more precise. Have 3-5 ways to do that. Think there is consensus that's a good thing. 2. JB will open issue where he creates >= 1 approaches for opt-out, probably at least unsafe block and unsafe attribute on variable. 3. This group is not willing to make this offline pass or extension.
* KG: Think that's an accurate description of the consensus.
* MM: Conclusion 4. once we design opt-out, the policy on where it applies can be done async, (does it apply to barriers or not, does it apply to derivatives or not).
* KG: What do you mean be async?
* MM: We can design the opt-out without deciding if it applies or does not apply to barriers.
* KG: Ok.
* JB: Shout out to AB for webgpu chat help navigating vulkan and spir-v specs. Want to request continued patience as we want to struggle with it. Thanks for sticking with us. More questions to come.


---


# 📆 Next Meeting Agenda Requests



* Next week: (**APAC**!) Tuesday, November 01, **4pm-5** (America/Los_Angeles)
* **Write any PRs you’ve promised!**
    * There are no other v1.0 issues outstanding (other than the ones on this agenda!) that are not blocked awaiting proposals.
