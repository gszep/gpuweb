# WGSL 2022-11-29 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** ds

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** (non-APAC) Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-11-22 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1H-9Nlg1wrruk2WYQ3vwoeDJbbfToD9xe2PDU8zcTqWk) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * Myles C. Maxfield
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * **Dan Sinclair**
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
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * **Erich Gubler**
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * Teodor Tanasoaia
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
* Jason Erb


---


# 📢 Announcements


### Orgs: Please vote on proposed charter for the GPU for the Web working group!



* Not very many have yet, and we’d like to see more support!
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



* 
* 


---


# ⏳ Timeboxes (until XX:15)


### [wgsl: full reference to module-scope variable is uniform, the value stored there may not be #3602](https://github.com/gpuweb/gpuweb/issues/3602)



* (DN:  I filed this when thinking through options for workgroupUniformLoad.  I don’t think it’s needed otherwise, and would complicate the spec a lot.)
* DN: AB drafted change to go along with workgroup Uniform Layout. Taking JBs suggestion to key off of load rule to do analysis. Agree that is a good thing to do, trying to draft correctly. PR is up and approved but questions from JP. Nothing to do in group right now. Weeks ago said doing workgroupUniformLoad, just need to trust us to write down the correct uniformity rules.
* KG: PR number?
* DN: 3586 [https://github.com/gpuweb/gpuweb/pull/3586](https://github.com/gpuweb/gpuweb/pull/3586) 
* KG: **Sounds good for now**.


### [WGSL: allow bgra8unorm as texel format with extension bgra8unorm_storage #3640](https://github.com/gpuweb/gpuweb/pull/3640)



* KG: Surprised this touches on WGSL. 
* DN: If it's ok in API, then we'll make it so.
* KG: Yup. We do need to do this separately. Won't happen automatically, can't connect BGRA storage buffer and read as RGBA, would need in shader to say it's BGRA.
* DN: Have not looked at this. Does that mean we have to put something in backend that does swizzle on loads and stores?
* KG: Think this is setup so we don't have to,j ust have to match pipeline layouts. In same boat as you. Discovered this last night.
* **DN: Needs review.**
* **DG: Need time to get up to speed**.
* AB: How do we feel about extension per texel format? How many will we have? 1000?
* KG: Lucky in that we won't have a tonne probably. This is specifically BGRA8 which is a burden that will always be with us. As much as would like to not support, folks want it. If already decided to support in API, then we'll support in WGSL
* AB: Can it be required?
* KG: Probably not, but don't know. KN says it would be useful. Talked about this at some point and made decision, but didn't put minutes back into issue. Will dig in and pull up background info so we can make more educated review.
* AB: Should we consider some other way to do the enable. Like, enable texel_format or something. Thinking ahead, don't want so many enums.
* KG: Question of to many formats, I think the answer is no. Think we'll end up with half a dozen but not a dozen. Which is tolerable.
* KG: Related: [https://github.com/gpuweb/gpuweb/issues/2869](https://github.com/gpuweb/gpuweb/issues/2869) and [https://github.com/gpuweb/gpuweb/issues/2748](https://github.com/gpuweb/gpuweb/issues/2748) 
* KG: Which is post-v1 should consider format that can be either and if we did have an either format we wouldn't' need a BGRA format.  Wouldn't need optional BGRA that is sometimes missing, but would tell folks to use the magical one.


---


# ⚖️ Discussions


### [Add uniformity analysis opt-out for derivative-using builtins #3644](https://github.com/gpuweb/gpuweb/pull/3644)



* DN: This is PR which adds 2 opt-outs.
    * 1. super localized, attribute on call site which says do not hook call site into must be uniform. So never generate uniformity error for that call. Everything else stays the same
    * 2. Option on createShaderModule which says fragment shader derivatives we won't check uniformity constraint. Weakens the constraint. 
* DN: Reason for having both is we like global for folks writing WGSL by hand knowing what they're doing. We know analysis is conservative and wrong, and don't want to annoy folks who know what they're doing. For localized, it has properties we've talked bout. It's intentional, it's focused and if applied where you want you get it just at that point.
* TR: Wondered if we care that we may have things that work on some platforms but when generating HLSL with FXC might fail with barrier cases
* DN: This is drafted to not weaken barrier cases analysis. Just for computing implicit derivatives.
* TR: So, dynamic uniform case that was brought up, is that expressible or not?
* DN: no. we choose to not cover that case with this PR. It could be extended to barriers if we want too but wanted to settle derivatives first
* TR: Right, just wanted to check as the dynamic uniform is a case FXC won't like either.
* KG: Can we get a sample of that this would look like in code? This will fail to compile, this will mark as bad uniformity.
* DN: TR had an example from 21 days ago
* TR: That was dynamic uniform case
* DN: simplified it
* TR: Don't think that's the right case
* JB: Looking for example call where you add the attribute and it works.
* KG: Just an editorial question for this point in the spec.
* DN: An example would be good.
* JB: Looked, commented in PR. Attribute seems fine. The global opt-out as we said is something we don't feel the need has been established especially for folks writing things by hand who know what they're doing. Seems to us that that audience is best served by local opt-out. Then they can say I know this op is OK but still get analysis for rest of code. Don't think being an expert WGSL author means that you don't want checks anywhere. If that is true, then I question the value of applying the analysis to fragment shaders at all. I believe when I write WGSL I'll want this analysis on and thing that will be common/standard. If that's the case then people writing wgsl by hand will generally only want local opt-out. Better served by having checks apply to other portions of the code they haven't evaluated carefully. One way to make a bit better would be to lift from attribute applied to calls to an attribute that applies to everything within some expression. Assume that was considered. Do same as you do here with args to function call, just wouldn't add edges for everything in an expression. Would be easier to apply to larger pieces of logic that people know are alright. Haven't tried to draft that. Aside from that concern about global opt-out. The specific opt-out, if people are comfortable, that looked fine.
* JB: Question for Google, how are people using generators not adequately served by the attribute as proposed? Or, if that's a mis-understanding and the audience of global opt-out is for WGSL written by hand, they why do they not prefer local opt-outs.
* DN: Talking generators, will flip a switch, on a higher level tool which sprinkles this everywhere which is fine (based on original language semantics). Serves but does pollute the intent if folks are looking at the generated code will be more difficult to read. Thought you could pull that back, like shaders from spirv-cross debugged in xcode. It isn't 100% fantastic but a secondary concern it's spread all over the place. For folks writing be hand, JP…
* JP: There will be people who know what they're doing and they just want to turn off in a clean way for their code. We're proposing both because people will want targeted, but for folks who just want it out of their way, coarse grained does that without being annoying. Having both options appeals to both audiences.
* KG: Keep coming back to this, in a counterfactual world where rust doesn't exist, that would be compelling. But, we have a world that has rust which forces you to annotate and note when you do something risky and they made a choice to not do that and said just annotate. In the same way you can't trust people to write safe code. Counterbalance is it just isn't' as risky, the downside is very different. The results you get are different and they are different in a porting hazard way, and in a way we do care about in Web. Write once and run on a large number of architectures. Causes problems when people depend on things that aren't guaranteed, this gives us a guarantee.If we can do it in a way that isn't too onerous we should do that.
* AB: The comparison to rust would be more compelling if, I have a choice to write in Rust or not, you don't have a choice for WGSL. So, the only thing to do on the web is the opinion thing we've had feedback on. Make look like languages I've delt with before. Every step is antagonistic before people get started and seems like wrong position to take. If we had a hard line of everything must always be portable. So out of bounds has a single behaviour, but we've cut a different long on every position. It isn't' philosophical anymore but per issue. Compromises made in the past, why not compromise here
* KG: Not saying we can't,s aying we don't want too. Two people with different weights, trying to determine where to put the compromise. Can't just say we're going to compromise we have to have the tennis game anyway.
* JB: Would really like to see in some actual code how many of the derivate operations need to be marked up. I know it's a pain to say, we don't have an example, want to see example, but I think that's a bad weighting. We're going to inflict this on the web and we couldn't spend 2 weeks to see how this feature plays out? That's a weird weighting. Look at the impact we'll have, we can spend a few days to actually put together corpus of several hundred line long shaders and say this is real code and to make it work it needed to be riddled with attributes. If that shows up, that changes my feeling. My suspicion is that these shaders when working and packing will have one or two attributes, maybe. The rest will be a bug that needs to be fixed. That's my suspicion. As a committed, we don't have, in the record, any evidence either way. May have off the record evidence, but it should be put in the record, and if we don't have ti we should get it. What would move Mozilla's opinion if we had a real world correct shader that required extensive decoration everywhere and where tweaking the design of the annotation to permit on expressions instead of calls, or on blocks instead of expressions wouldn't substantially alleviate the problem.
* AB: If you don't trust our feedback, going to find a corpus isn't our responsibility. You should implement this and run it on the tests you have and if that says it's valid we can come back. It souls like you're saying we should gather that information. Just want to push back. We have this experience from implementing and running on corpus and the feedback we've gotten from our users is don't do this. Thats where this comes from to listen to our customers.
* JP: My issue is asking for a single shader example, it might be for a single shader you have 1-2 places but real games have hundred or thousands of shaders. The one we have is 1500 shaders and every one basically fails in one way or another
* KG: We knew that going in. We knew existing shades would fail. And, of course they'd ask us to just not do that as their stuff runs fine already. If we can satisfy them anyway and have the thing which pushes this forward in terms of correctness and portability that's an improvement and i'm willing to spend innovation points to get there
* JB: Talking about the unity corpus, we aren't talking about that. It's mechanical translation and we agree that isn't the concern. Second, AB of course I trust you, it isn't a question of disbelief, what I'm saying is that none of this evidence is in the record. Lets get one piece of it in the record and see whether it is, and not talking about generated, some hand written shaders that show that all the derivative ops are decorated or not. Not asking to do a lot of work, pick one example from corpus and put it in the record. Not something cherry picked to show the worst case, a plausible looking real world shader and get it in the record. Not questioning good faith, just saying that for the discussion to proceed the examples need to be put somewhere the committee can see them.
* KG: Would love to see a bad example as well where it isn't onerous but we can convince ourselves it's a unreasonable case and it's ok to write the annotations. Simple example, real-world non-trivial and bad, onerous example would be ideal. 
* JP: Point about corpus above is I agree that we mechanically generate WGSL so what the opt-out looks like doesn't matter. Their shaders are handwritten. There will be other folks who have 100's or 1000's of shaders like that which will have issues and trying to determine where to put things would be challenging. Doesn't mean it wouldn't be onerous for folks who have large shaders …
* KG: Fine to have folks run through sed to replace globally. Fine with that. Just replace all derivate functions with a _non_uniform to them. That would solve need, just find and replace those. Given that that would solve it, if an attribute is isomorphic to that …
* JB: Not sure about JPs point, not sure it follows. If folks are re-writing large corpuses in WGSL and doing it by hand, then the information we want to collect is, how often are the errors encountered real and need to be fixed and how often were they unhelpful. If the process of rewriting in WGSL alerts you to bugs you want to fix that's a huge win. Big point in favour of the analysis. What people are doing with their corpuses today isn't relevant. What we want to know is what will they do with information they've never had before. If they don't care about that information then we can simplify the spec quite a bit. But suspect that folks will want that information. FPeople doing these re-writes by hand will say I want this. But, maybe I'm mis-understanding what customer are doing. Are we talking about mechanical conversion, or re-write in wgsl, or are they finding the shaders they write are not helpful. Different question, when wgsl written by hand, how often helped and how often found analysis to be incorrect. If folks saying analysis is wrong all the time, then we know what to do and we take it out.
* AB: Investigating if it's correct and that's not just a priori knowledge. The underlying apis do not have the same requirements we impose. We put on training wheels and claim that's a good thing. no other apis think it's a good thing and those others shaders exist. Even if there was a bug would you notice? Lot of the time the answer is now. Do you qualify what is a correct answer? Technically this was an indeterminate value but, whatever or the results on screen look good enough.
* BC: Statement from someone who isn't mechanically translating shaders you think it is beneficial. How Unity translates shaders, if it's mechanical or not, isn't relevant if they find it useful. They were alerted to this months before making hard error. They area  company with large corpus and we give them these warnings and they didn't think it was valuable. They could have taken the information and re-structured code based on analysis. But instead they have a large corpus of shaders and do not think there is value from these warnings. Don't see how mechanical vs hand crafting changes outcome. 
* JB: When I say someone is working in bad faith, they're saying something they don't believe. I believe people don't investigate things. That isn't bad faith, thats just working to a deadline. What I'm hearing from this story is, They have a flow, it was working and getting things done and then that flow stopped. THen said turn it off. What I haven't heard is that they did the work to determine if it could be turned off. Which is why I want to see a shader, I want to see that the work was done to analysis some of this instead of just the flow being interrupted. To ABs point, there is technical correctness or do I care. What I mean is folks see the errors and want to fix it. It isn't' about do I comply with the strict analysis, i don't care. If this is mostly telling folks who mostly understand the case and who are writing by hand and they don't care then this isn't useful. What i'm seeing here is evidence that for mechanical conversions it is a road bump. Haven't yet seen something that says people who look at the errors don't care to fix it. If people see errors and don't care to fix it then it isn't a useful analysis.
* AB: Concerned with comment that if Unity has a job to get done, and what we think of ….
* KG: To clarify, specifically we're worried about is would they not be satisfied due to mechanical translation that they won't be happen. We do not want to put road blocks and the mechanical translation effectively gives a global opt-opt seems unnecessary as well. If anyone doing a mechanical translation can just inject the annotation globally through the shader and they don't need global. If everyone is satisfied with local opt-out then everyone is satisfied.
* AB: Is goal is to make folks aware then we want a warning and not an error \
KG: Want stronger then warning
* AB: Why are other things non-errors that lead to non-portability and this is. Can't wrap head around why this is the ring
* KG: Willing to spend innovation points on it
* AB: Why not others
* KG :Not willing to spend points on them. It really comes down to that. Why we don't talk about philosophy and drop down to individual choices.
* DN: Responding to JB. The issue from Unity saying it's too strict should be able to look at that and write out ot WGSL cases to generate some of the data looked for. When they say far too strict they have real cases. And I looked and saw that the analysis was incorrect. Raph also had compute shader examples. That can be next step in conversation.
* JB: Not talking about compute shaders. Other point is, i thought we had generate consensus of committee that the quality of the output of mechanical translation is not a concern. It was mentioned and I thought I hard someone say we shouldn't consider that. That's OK with me but that was part of the basis I'm operating from. If I mis-understood that and committee is concerned about translated code then comments about ruling out translated code doen't make sense.
* DN: Readability of that code is a secondary concern.
* KG: I would like to tackle this and move forward, take the local opt-out from this CL and other things we've fixed and go back to the uniformity analysis too strict issue and see if things are in better shape. If they are better enough and re-evaluate how strongly we feel about these things. Would love it if unity's corpus had automatic injection of ignore uniformity annotation and from there evaluate with them, and others, about whether or not they see the global opt out anymore. We're superon board with local opt out and strongly suspect that local is good enough. And can we prove that out. Or, we come back and see what the user has to do and see it's just annoying nd then we can see if there is more we need to do like the global opt-out. There is a bunch done here and don't want to conflate what users say they need and what they say they need. Would like to move forward that way, make PR with local opt out, make tooling and re-investigate.
* JB: That all sounds good, but also, honestly I would expect AB and me to be agreeing at this point because I don't think we're looking at different data. It sounds like AB is persuaded that the analysis in summary is not helpful to folks using derivatives. I would trust his judgment but I don't understand what he's looing at to make this conclusion. Would like to better understand how arrived at conclusion that this isn't providing help to users for derivatives. That would help move my position.
* AB: Can chat in office hours.
* TR: Wanted to ask whether or not making global opt out be turn error into warning instead of complete opt-out into silence. Starting from that point you get warnings and can annotate known cases which silence the warnings.But the global makes the rest continue to be warnings. Doesn't erase all the information we could get from where analysis might be wrong and could be improvised.
* JB: Think unhelpful warnings are as bad as unhelpful errors. Are these notices warnings or errors. Is this information actually valuable. If not valuable don't want as warning. If it is then want as error when folks can silence it.
* TR: But is that binary? Could be valuable sometimes.
* JB: Problem is you get flooded with warnings and you ignore. If not generally valuable then get put into flood then other warnings get lost. Just anti-warning because there is this flood character to them. One nice thing rust does is to let you turn off warnings in a granular way. It lets other warnings continue to be alive.
* DS: That's what Tex is saying. Silence them broadly to warnings, then you can go through and fix the errors piecemeal.
* TR: Also, do we not have the ability to turn off a single warning, like Clang can.
* AB: WGSL doesn’t specify any warnings.  Been deliberate about that. So can’t really answer that directly.
* JB: If it isn't localized it can be bad. Want to know other parts of program are wrong and turn off in one place.
* TR: Concern is other options are either no global so tool annotates everything even if necessary and correct and then loose information and can't improve easily or we have global option which turns it off and get nothing. No longer on slope you can use to improve code.
* AB: One last thought, where I am is to me this ithe idea sort of thing for a lint tool that exists separate to the tool. Or developer debugging tool. Part of the frustration is having language that people can understand and work through (part of my frustration) is trying to make it precise enough to hit all these cases and explain in simple terns but fall apart in the corners. When it's tooling can go a bit further then the strict case and can do nice things. So, think there is value in the tooling ecosystem, and that's where I envision WGSL in the future and in that future, is this an error or warning.


### [Workgroup Broadcast · Issue #2586](https://github.com/gpuweb/gpuweb/issues/2586) 



* PR: [Add workgroup broadcast and uniform load built-ins #3586](https://github.com/gpuweb/gpuweb/pull/3586) 
* Alan: reworked the PR to remove workgroupBroadcast and update uniformity analysis to maintain uniformity of memory views. workgroupUniformLoad parameter is required to be uniform. Credit to Jim Blandy.


### [Designing a uniformity opt-out #3554](https://github.com/gpuweb/gpuweb/issues/3554)



* PR [https://github.com/gpuweb/gpuweb/pull/3644](https://github.com/gpuweb/gpuweb/pull/3644) : proposed a fine-grain and coarse-grain opt-out
* 


---


# 📆 Next Meeting Agenda Requests



* Next week: (**APAC**!) Tuesday, December 06, **4-5pm** (America/Los_Angeles)
* **Write any PRs you’ve promised!**
    * There are no other v1.0 issues outstanding (other than the ones on this agenda!) that are not blocked awaiting proposals.
* WGSL schedule:
    * December 13: APAC
    * December 20: Non-APAC
    * (Christmas Eve&Day Dec24&25, Saturday&Sunday)
    * _December 27: No Meeting_
    * January 03: APAC
        * DN: Suggest we cancel
    * Note: Based on our current issues remaining for v1 though, we are very likely to cancel meetings due to lack of discussion topics, so we can likely be liberal with skipping meetings.
* 

[https://github.com/gpuweb/gpuweb/issues/3658](https://github.com/gpuweb/gpuweb/issues/3658) Add integer overloads sign builtin



* PR: [https://github.com/gpuweb/gpuweb/pull/3659](https://github.com/gpuweb/gpuweb/pull/3659) 