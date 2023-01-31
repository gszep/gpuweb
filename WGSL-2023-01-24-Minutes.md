# WGSL 2023-01-24 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** ds

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** (non--APAC) Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-01-17 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1X_x02oZO8EYugNUVGAPPmCYk7cARzh5G3FXRWKQq0Xk) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * Mike Wyrzykowski 
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
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * **Tex Riddell**
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
* **Dominic Cerisano**
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



* F2F at Apple Park, Feb 16-17.
    * There will be videoconferencing (WebEx) for people who wish to attend remotely.
    * MM: I’m setting up a group dinner on Feb16. Send me preferences or ideas for restaurants!


---


# ⏳ Timeboxes (until XX:25)


### [wgsl: vec2 shouldn't be a keyword; disambiguate parsing of type construction expr vec2&lt;i32>(0,1) #3739](https://github.com/gpuweb/gpuweb/issues/3739) 



* [Lookahead disambiguation of less-than vs template argument list (v2) #3770](https://github.com/gpuweb/gpuweb/issues/3770) 
* [Is it intended that keywords can not be identifiers? #3692](https://github.com/gpuweb/gpuweb/issues/3692) 
* DN: BC posted v2 of pre-tokenization. Goes through source and identifiers that are template lists. Things in various contexts allowing nested expressions. Wondering folks opinion on interposing a thing which changes the meanings of certain &lt; > signs to be TemplateIntroducer and TemplateTerminator
* JB: Moz was excited for maybe borrowing the solution from c# and it seems like c# just has a (???) keyword. We're not stoked that the commitment to looking like c++ is so strong that we are adding infinite lookahead with heuristics. We have to pay infinite lookahead and don't get a firm answer, it's ambiguous in some places. That seems severe dedication to a fixed syntax. Personally it's hard to say the sky will fall if we don't match c++. What all this heroic effort says to me is that Google needs to re-consider whether or not it's open to the idea of making a small change to the syntax. It was drafted to use turbofish, we don't use it in other places but it's analogous to d syntax. It's pretty severe, adding a new pass, can't tokenize as we go, and doesn't disambiguate, just heuristic. `x &lt; y >> z`. Ordinary expression, nothing contrived. I think we're in a difficult situation as we talk about differing syntaxes if it isn't c++ folks won't use it, and that's just not true. Think we need to step back on negotiating positions and be more open to syntax changes.
* BC: c# does have something similar, can give links to where it does the lookahead. Infinite lookahead is one way of phrasing, the second draft is that it's a token transform, but it's a pass of the tokens. Not easily pipeline-able. Example of `a &lt; b >> c` I replied and we already have precedence. If that &lt; was a bitshift you'd need `()`s. Ordinary expression maybe, but WGSL already requires parens, don't see a substantial difference if you need parens to disambiguate.
* JB: Like the way that we require parens in the precedence chart. Think that's a good move for clarity. Just seems like with the parens for associativity we actually asked for that and had the option to not have that and thought it was a positive benefit. Introduced by permitting fewer combinations on purpose. This is a set of parens that are required because we can't bear the glares of our co-workers because they want what they're used to.
* KG: Don't think it's entirely that, just preference for it
* AB: Don't allow expressions in array already. Already have restrictions on expressions
* JB: Apologize, taking workers' opinions into consideration.
* KG: This will become a discussion issue. Spitball idea earlier was something simpler which may break down in complicated situations. One of the ways I parse things as I'm reading and a natural difference to how to write `&lt;` binary expressions and template types and generics is in regards to the whitespace involved. Such that almost always, but not entirely, it's common for there to be whitespace on either side of the &lt; sign. In which case it's almost always a &lt;. Don't remember the last time I saw a template argument that had space on both sides. Commonly there is space on the interior but almost never on the exterior. (`Identifier&lt;  >)` right heuristic, but might disambiguate in different contexts. 
* JB: `identifier&lt;` generic `identifier &lt;` less than?
* KG: Maybe a way to differentiate would be needed
* DS: Whitespace significant makes me sad
* MM: If we make it significant then we say all binary ops require on both sides.
* KG: Not sure that entirely follows but think it's different for arithmetic vs logical
* EG: What about tab
* KG: Counts as blankspace, we have grammar for blankspace. It's not that we never ignore blankspace, it already separates identifiers, not like it's never used, but this would be an expansion of that. 
* DN: Glad you raised it, also have the allergic response to it. Rant against python, one of those points. It's on the record. Pre-tokenization is one solution, new syntax is another solution, another thing at play is either "never have user declared templates' or allowing 2 different syntaxes for this kind of thing. So vec2 stays as it is, the set we have is a good base, user declared templates have a different mechanism with a different look .Not sure if we want to open either of those
* BC: More than user, any core types that take arguments, don't want different syntax for that and have to hope we have them reserved.
* JB: Not sure if it came up or not, already have the case where we ???.So we could say you don't get to use &lt;> as type parens unless you put `new` in front. `new vec3&lt;f32>()`. Or `vec2()` don't get generic args unless you said new.
* BC: So, `new` has heap allocation connotations. With pointers would strongly advise a new prefix. Doesn't solve the need for user generics. Would you prefix the call to user generic function?
* JB: Maybe have inference for those arguments as well.
* BC: Not everything can be inferred.
* KG: Glad to have this on the record, think we've realized none of the things we have are super appealing to everyone. Didn't realize new had parsing value like this, so that's new to me. Interesting in the context of languages that do use and require it. Like having that in the solution space.
* DN: To firm up Jims solution Are you allergic to long lookahead of pre-tokenization?
* JB: Upset about mis-characterizing Googles opinion, I apologize
* DN: It's ok, want to sharpen position
* JB: It seems like heroics, it's clearly heroics given you've worked so hard for so long and now to just give in. The other side of the trade off which is preserving the syntax just doesn't seem worth it to me. Obviously we can't in this meeting take absolute positions because we have to leave things open for discussion but you're spending a lot, is that really the balance you want? The balance seems bad to me. I don't absolutely object but it's expensive.
* EG: Wanted to ask for what other options were
* DN: New syntax
    * backing away from user declared having similar syntax
    * Or not having user declared templates.
* EG: May have mis-understood something said `using` syntax?
* DS: We have aliases for somethings like `vec4f` but not for things like `texture_storage_2d`
* KG: You could write an alias for that and then it would be in expression context.
* BC: It's a combination of people not being fond of the alternative but possibly more weighted is changing the language will throw us off. Thursday is the branch for last chrome release where we can do deprecations. If we're changing syntax we'll delay the chrome v1 which is pretty disruptive.
* KG: Understandable. The main reason we're talking about this now is, since we do want generics in the future we need something in place we have confidence we'd move towards in the future. It's hard to have that confidence in something like this without having a solution. What we're doing here is we want a solution in mind that is prototype-able in v1 even if we don't ship in v1. Something we can go to moving forward, even though there are no generics in v1. Does that sound like the right amount of caution?
* BC: **That's the stance we're going**. The proposal for the lookahead has landed in tint and we've made it a hard error for ambiguous cases. **Wanted to get feedback**. Primes us if we go down this path to do this without disrupting shaders at a later date. Need a solid plan for language evolution.


### [Should WGSL have a ternary operator? #3747](https://github.com/gpuweb/gpuweb/issues/3747)



* KG: Our proposal was that there is a similarity between ternary and if/else and it feels like, in Mozilla and the group maybe, that there is a preference for if/else rather than ternary. Also if/else expressions are not something we want in v1. While ternary could be in v1 because it's straight forward but w**e should not put in v1 to give time to do if/else if we want post v1.**
* DN: 100% agree.


### [Shader compilation is fragile because FXC #3691](https://github.com/gpuweb/gpuweb/issues/3691) 



* > **TR and GR said they'd take a brief look and see**, weren't sure if this should be an issue in the first place but haven't looked in detail.
* TR: There are a few levels. The specific run into is a bug in old version of FXC and there is a followup comment on the Tint issue. On the other issue, that seems to be something for the group. For issues in that backend path, or any backend path, there needs to be a mechanism to surface things so the user doesn't just get things to fail at runtime
* KG: Have that as UAs. Can give more context then the api allows us to give. Thought we had string error messages programmatically so I think we're set here. Even if this is that something failed and we don't know why, it wasn't spec compliant. Think we're set there.
* TR: **Only other thing is if WGSL supports dynamically uniform control flow with barriers** because that's something that is not supported on FXC or some implementations if you by-pass it. So, dynamically uniform will break things even though SPIRV supports
* DN: **WGSL requires static uniform that it can prove is statically uniform**.
* AB: **For barriers**.
* TR: Right just for barriers.
* KG: **Satisfied with this issue**.


### [Concretization of abstract expressions can produce surprising types #3716](https://github.com/gpuweb/gpuweb/issues/3716) 



* JB: Two examples BC chooses, when reading it may help to know the first example is self contained. Everything in the first example, don't compare to the second. The problem is the array C gets to stay abstract in the first case but it doesn't get to stay abstract in the y case. Basically the second example shows similar with right shift operator. Our observation is that array indexing is a special case because the index operand, the type of the index operand does not affect the expression, it's not like + or an ordinary binary op. It seems like, maybe we missed something, we should be able to let the result of the subtract stay abstract even when the index type is not. Basically it's a matter of adding the right overloads to the subscripts. Right and left shift operators are analogous to array subscript in that the type of the right operand doesn't affect the result. On an operator by operator we could finesse this and make both work out.
* AB: Will always have to concretize the abstract array because you have to index at runtime so you can't have an abstract at runtime. At some time, concretize. The question is when does that occur. Understand the point about the indexing being a different type and it becomes a question, part of it is how long do you defer. I have runtime and will have to concretize, but don't know when I have to.
* KG: Distinction between concretization of value and types. We determine the runtime type at compile time but it's a distinction of when the value are concretized. If you have an array that is being indexed dynamically the type of the result is still know at compile time. 
* AB: But it's not abstract
* KG: It's not abstract, we could say the resulting type is abstract but the value is not.
* BC: The problem comes down to having an array of abstract and have an index, regardless of type, you have a runtime value, in the backend you have to make an index operation. So the index operands have to be values, so you have to concretize the array. What you concretize to is bottom up. The first time you see something that requires concretization we kick in the rule. In the case of array we default to i32.
* KG: I get that, just saying knowing that you'll have to provide a dynamic runtime value and given a type for it, knowing it happens and emitting the framework for that code don't think we need to concretize it. Depends on the backend if it's easy or not, conceptually the argument doesn't really mean it has to be concretized.
* BC: Problem is the abstract values can be outside range of 32 bit integers. Backend may not be able to represent value if provided in abstract form.
* KG: Need to think about this more.
* JB: Understand we need to concretize the index but I don't see why we need to concretize the array. By the time we're generating code, we've settled types, the abstract type we just have decided at that phase, ultimately everything has a concrete type by the time we generate code. So, why does the array itself have to be concretized.
* BC: What would you generate for the array in the backend
* JB: Don't choose until we've determined everything
* KG: Determine it when further along
* BC: Right but need to bubble up through expressions to infer, then have type you have to propagate back down. In that chain, if you have a builtin overloaded which is the target for concretization, now you have to infer the param types for the inner expression and don't think that's solvable. Think we have ambiguous overloads.
* KG: That's not a death-knell, would have to say it's an error for now
* AB: Is that better than concretizing when hitting a non-const expression?
* KG: Possibly due to this issue where it's surprising
* JB: The context is that this doesn't seem like a major issue, it's a quirk that people live with. If we find a nice solution, great, but don't have super strong feelings.
* BC: Filed the issue because I was caught out, it wasn't hard to fix, got compiler error that types didn't match. It might have folks scratching their heads. All I did was add a `.` to make it a float again. Took 10 seconds. Might take a bit longer, but not the end of the world.
* KG: Does provide a bottleneck in that folks have to concretize earlier
* MM: All this concretization stuff is novel. Wonder if folks have written code that interacts with this subsystem are not also compiler writers
* BC: Adopted from go. Although, go has a single abstract type, there is no float, just abstract float. THe distinction between float and abstract float adds complexity.
* MM: That wasn't the point I was trying to say. We're trying to make decisions that are best from authors, do we have experience from any authors that have looked at this and did they run away screaming?
* BC: Don't think most folks notice. That was the goal. For the 99% you write literals and constants and it does the right thing. This was the one place I saw it do something unexpected. We're probably nit-picking at the algorithm right now.
* BC: Generally when folks talk tint vs naga, naga doesn't do this and folks have heard that tint does it and they don't have to think about it and people like not having to pay attention.

[Uniform control flow vs textures for flat interpolated varyings #3668](https://github.com/gpuweb/gpuweb/issues/3668)



* KG: Think the resolution we came to is that this is something that is reasonable.** Many users expect this, not guaranteed, it's our job to be the messenger that this doesn't work**. Think the only thing we wanted to do was, in the error message, is to distinguish between these things. If we can tell there is a uniformy error that relies on flat and would be ok if flat was valid, then we can update the error to say the thing you want isn't real. It's a nice to have UA thing but nothing to do in the spec, per say, then have the issue to point folks at. Does that sound reasonable?
* DN: Sounds reasonable to me. Also, since then, have diagnostic opt-out so they can turn it off if they want, but up to the UA to do better
* MM: Why is flat non-uniform?
* KG: Doesn't have guarantees you want. Reconvergence in SPIRV for things that are dynamic uniform is not guaranteed to reconverge.
* MM: What does that have to do with flat?
* KG: Flat is not dynamically uniform. Might think all things in quad get the same value, but the uniformity model in the spec says these values may not reconverge.
* AB: Reconvergence is at a different scope then flat
* KG: Have to return to the root of the execution
* TR: Flat is quad uniform but not subgroup uniform which is needed for the reconvergence
* KG: This is the secret difference, where technically we have these two things. Reconviergence is not guaranteed to happen on a flat, at least not in the cases they think it will. Does that make sense?
* MM: Not really …. But ok
* KG: Yep, pretty much “because Vulkan”


---


# ⚖️ Discussions


### Language evolution [https://github.com/gpuweb/gpuweb/issues/3149](https://github.com/gpuweb/gpuweb/issues/3149)  



* KG: Main thing has to do with generics and going back to anything we want to talk about more on #3739.
* DN: Want to articulate one thing, we have made a dichotomy between an enable for hardware features and sugar is just implemented without opt-in. We also want to make the distinction that things go into core if we all think are valuable and we'd all implement. There is a middle ground where some think it's useful and others not, and having an enable for that is fine. Doesn't always have to be a hardware feature. 3 categories
    * Things in core, that hopefully everyone implements
    * Sugar landing at different times in different browsers
    * Hardware features
* MM: Little scary for the spec to say something that doesn't have consensus. Feel like the new category, would be explicitly non-standardized and the browser ships itself.
* KG: Like, you get it in spec even if not ready to implement. Like to have things in spec so we agree how it would work and address concerns and have conscience even if we're going to implement soon or not at all.
* AB: Think MM's concern is valid. Also wouldn't want spec to have a bunch of this is only for this platform. Littered with extensions which aren't widely available. If it's in the spec, maybe it's gated on hardware but expect it to be on all platforms. Can be experimental internal browser things to test. Things in some implementations that aren't in spec that have own enables, but side with MM that spec is widely available thing
* KG: Agree but caveat that would like to give feedback on what browsers ship and might accidentally cause to be built on top of the language. Browser might want to expand in a direction and browser gets enough mindshare that we want to ship, but another UA can't implement and we have a breaking change. Want to avoid cases like that. Understand not too many things one-off shipping. Experimentation happens outside spec. But don't encourage shipping things outside the spec
* AB: Agree to design what is going to ship. What goes in published WGSL spec vs not. Us having a design or PR is enough if everyone is commenting to not be in the spec.
* MM: Distinction I didn't describe properly, group collectively thinks it's a good idea and we want to move in a direction. Worried about someone coming up with a proposal, builds behind flag, and comes to group saying just for us, please put it into the WGSL spec
* KG: Happy to say no to that.
* MM: Also don't want to encourage folks to ship non-standard things, not what I meant.
* KG: Core is to sway judgment concerns. Fundamentally a judgment issue.
* MM: Maybe the right way to get conclusion everyone can agree with is that the 3rd category is ok as long as the group has consensus there should be an enable for the thing and that process is the same as all other processes.
* DN: Sounds right to me
* MM: Ties into pull request for vendor agnosticism and not land things in the spec that are prefixed.
* **MM: So are we good here for v1?**
* **(yes)**
* BC: **We need to determine a way to tell what version of WGSL supported**
* MM: Sounds like a query function and new issue. **Different from this issue**.
* BC: **Will discuss internally and may put up an api pull request**.
* KG: Suspect there will be a lot of appetite for a constant
* BC: What could you do with it? We don't have `if consteval` would need it in a block
* KG: Could const assert and error out.


---


# 📆 Next Meeting Agenda Requests



* Next week: (non-APAC) Tuesday, January 31, **11a-noon** (America/Los_Angeles)

[https://github.com/gpuweb/gpuweb/pull/3713](https://github.com/gpuweb/gpuweb/pull/3713) Add diagnostic filtering; range diagnostics apply to control flow constructs and compound statements #3713



* Overhauled per feedback:
    * Fix the reporting for triggering locations
    * Per committee feedback, make the spec description closer to the “good” algorithm. So the uniformity analysis now needs to be aware of diagnostic severities.   
    * Generally reworded the uniformity analysis overview algorithm, for clarity.

[https://github.com/gpuweb/gpuweb/pull/3792](https://github.com/gpuweb/gpuweb/pull/3792) Add wgsl_version_at_least builtin function.



* Defines versioning scheme with major.minor, and basic sem-ver guarantee. Declares this version is version 0.0 (with intent to go to 1.0 at a later time)
* Related to Language evolution [https://github.com/gpuweb/gpuweb/issues/3149](https://github.com/gpuweb/gpuweb/issues/3149)  

[https://github.com/gpuweb/gpuweb/pull/3784](https://github.com/gpuweb/gpuweb/pull/3784) Reserve common scalar types, and `quat`


### [wgsl: vec2 shouldn't be a keyword; disambiguate parsing of type construction expr vec2&lt;i32>(0,1) #3739](https://github.com/gpuweb/gpuweb/issues/3739) 



* [Lookahead disambiguation of less-than vs template argument list (v2) #3770](https://github.com/gpuweb/gpuweb/issues/3770) 

JB: So, does Google shipping mean we can't change spec?

KG: No, we reserve the right. This is a vendor deciding to ship something and we have historically can and will break if needed to get things right. This has happened in WebGL.

JB: Good.
