# WGSL 2022-05-17 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** DS

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon** Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+is%3Aissue+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-05-10 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/193iPVMOfiyGEoRVsM0bcymNRgJWmMEyDK4s9VK3kx1g) 

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
    * **Ryan Harrison**
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


### FYIs and Notable Offline Merges (new section!)



*  Added missing condition for out-of-bounds sample_index in textureLoad overload for multisampled textures. [https://github.com/gpuweb/gpuweb/pull/2880](https://github.com/gpuweb/gpuweb/pull/2880)
* Clarify convertibility of constant-time expressions.  Does not depend on the value.  (Already had an example showing this). [https://github.com/gpuweb/gpuweb/pull/2862](https://github.com/gpuweb/gpuweb/pull/2862)  
* 


---


# ⏳ Timeboxes (until 11:30)


### [consider namespaces for WGSL · Issue #777](https://github.com/gpuweb/gpuweb/issues/777)



* KG: Had a lot of desire from Apple, let’s hold off


### [☂️ Initial internal feedback #2213](https://github.com/gpuweb/gpuweb/issues/2213) 



* Google: Thinks all items have been addressed.
* KG: Is it satisfied, probably but need to ask Apple.


### [How should WGSL map to the availability/visibility parts of the Vulkan Memory Model? · Issue #2228](https://github.com/gpuweb/gpuweb/issues/2228)



* AB: Suggest closing, probably won’t get answer from RM
* KG: **Lets close and if RM comes back we can talk about it.** A lot of folks use personal githubs for this kind of thing, so maybe could hear back. Will close for now.


### [forbid spelling out pointer types that are not instantiable #2886](https://github.com/gpuweb/gpuweb/issues/2886) 



* DN: Follow up to JB issue about formal params to functions. Thought was only case needed but thought about type aliases and think we should ban that. Compiler could be simpler if not needed
* JB: Strongly support
* KG: Makes sense to me. **Resolved**.


### [Clarification around `sampled type` in the Texture Builtin section. · Issue #2849](https://github.com/gpuweb/gpuweb/issues/2849)



* Editorial: explain sampled type better
* DS: Not explained in spec, needs to be explained.

(5!)


### [Clarify behaviour of `fract` with very small negative numbers · Issue #2822](https://github.com/gpuweb/gpuweb/issues/2822) 



* DN: PR [Clarify fract for small negative numbers #2900](https://github.com/gpuweb/gpuweb/pull/2900)
* DN: Found by RH when testing float. Fract should exclude 1.0. Some impls include 1.0 due to rounding so say that could happen. Think right answer is to not say fract is correctly rounded … think PR wrong …. Will fix.
* KG: Point about being technically true due to infantesimal we only have small values. PR is probably good as it stands. Land it.


### [Attribute 'Valid Values' should be updated for abstract integers · Issue #2809](https://github.com/gpuweb/gpuweb/issues/2809)



* DN:  Editorial change PR: [Clairfy attribute numeric params #2902](https://github.com/gpuweb/gpuweb/pull/2902)
* DN: About attribute values and currently says +i32 literal which makes seem like requires i suffix which we don’t want. Rewording to integer literal (abstract integer) which yields a positive i32 so you have to coerce and has to fit into i32 range.
* KG: **Approved**.
* AB: **Maybe missing prepositions in diff**.
* DN: **Will incorporate**.


### [Spec clarification around untyped LHS and abstract RHS · Issue #2802](https://github.com/gpuweb/gpuweb/issues/2802)



* DN: **Already taken care of in separate PR**.


### [Missing specification of error type produced · Issue #2730](https://github.com/gpuweb/gpuweb/issues/2730)



* KG: Think we did this?
* AB: Not exactly. Open question about if we want shader creation errors on static out of bounds which is in another issue which isn’t resolve. Others have language which is updated to make it clearer there is an error.
* KG: DM filed other issue
* AB: 1803 on agenda later …
* AB: If resolve on that happy to clean up language.
* KG: Other thought is if we agree on other errors besides that one can **punt that one to 1803** and mark it as resolved or editorial
* AB: **Editorial**.


### [Integer signedness usability · Issue #2588](https://github.com/gpuweb/gpuweb/issues/2588)



* AB: We’re at minimum of what we wanted to do, curious if others want to go further. Don’t have internal consensus yet. Hard to read through builtins and tell what’s going on. Prefer any open ints and find better way to write it. Can discuss later or now, if others have opinions
* KG: Probably in good place for V1. MVP quality. If folks have issues would say we’re satisfied for v1 but not post-v1.
* AB: Can **move post V1**
* KG: Sold. Still accepting ideas, but want them to be important
* DN: Some questions, does it slow down compiler and does it blowup CTS in a way that’s painful.
* KG: Blowup how?
* DS: Combinatorially, huge number of test cases for all combinations.

(10!)


### [Behavior of discarding everything #2523](https://github.com/gpuweb/gpuweb/issues/2523)



* KG: Might be resolved.
* AB: Would like to leave as is. We know Naga has seen case from code generator. Given problems in metal backend feel like changing code gen is easier to not do unconditional discard. Not big change and makes a bunch of stuff easier to explain.
* KG: As long as has satisfactory workaround, fine for v1?
* AB: Yea
* KG: Happy with that. **Will copy notes and mark closed**.


### [Error with literal indexing in a shader #1803](https://github.com/gpuweb/gpuweb/issues/1803)



* AB: As stands, it’s an undefined value out of bounds. Any value is possible for that type. Folks have expressed static values should be shader compiler error. Deferred to creation time expressions which we have. Could categorize as if creation time expression it’s shader creation error otherwise standard out of bounds behaviour defined elsewhere. Personally not hugely satisfying but if folks like can write it up.
* JB: What do you find more satisfying?
* AB: Leaving as is. Not a big deal as we’ve defined the behaviour. Tooling could add warnings but making a hard error is making a decision for someone
* KG: Included towards that. Symmetric and consistent across constexpr and dynamic access. Easy to implement compile wise. If have and confused emit 0 and a warning. If that makes you more satisfied makes me too
* JB: Not going to jump up and down to flag as an error at compile time.
* KG: **Sounds like consensus on that** and we can revisit later if needed
* JB: Getting errors from FXC where it unrolls loops and detects dynamic out of bounds which we’ll never detect. It’s annoying so we don’t want to throw that at other folks.
* TR: FXC is weird here. Like FXC unrolling and going out of bounds on an infinite loop. Not sure how to deal with that generating HLSL and going to FXC
* GR: DXC doesn't’ do that
* JP: How undefined is hte undefined result? Is a branch non-uniform?
* KG: **We’ll commit to saying it’s uniform**. Should give a uniform value.
* JB: What if index is not uniform
* KG: Then non-uniform. Feel like accidentally hitting trip wire of this being out of bounds should not change uniformity.
* AB: That’s fair
* JP: Then do we need different language or is dynamic out of bounds always uniform?
* AB: If same expression we shouldn’t analyze it differently.
* JB: The FXC case is coming from community case of WGPU.
* AB: **Assign to me** and I’ll cleanup.


### [Uniformity annotations for global variables · Issue #1791](https://github.com/gpuweb/gpuweb/issues/1791)



* AB: Proposed post-v1 or post uniformity feedback which is starting to trickle in. Think wait on feedback. We have a label for uniformity and this is tagged.
* DN: Currently resolved/needs-spec should be post-mvp.
* KG: **Mark it polish post-v1**.
* AB: Think a lot of this comes from consideration of translating from SPIR-V -> WGSL. Not a v1 target for us.


---


# ⚖️ Discussions


### [Are padding bytes of structures intialized with zero value expressions? #1747](https://github.com/gpuweb/gpuweb/issues/1747)



* Offline
    * MM: After the [2202-05-10] call today I consulted with my team, and we came to a different conclusion: [https://github.com/gpuweb/gpuweb/issues/1747#issuecomment-1123121829](https://github.com/gpuweb/gpuweb/issues/1747#issuecomment-1123121829)
* AB: Took action to talk to Khronos. The vulkan spec is how it is, and won’t change. Agree with us that if we want to emit a different struct it would be sufficient if we initialized those members by adding padding fields. Tint does in metal backend. Talked about workgroup memory safety concerns. Important thing is whether padding exists in global memory or not. How can you observe the effects, have to bring back via global memory. So important to not have padding fields in that memory. Or if in workgroup memory, initialize to zero and copy the zero. Feels like adding the padding instead of relying on not coping to zero initialize and then it’s safe. We can say it’s zero initialized and leave in it could or could not be touched as implementation defined. Think that’s portable enough. However, Apple may have different opinion.
* KG: Sounds like could we have it not trample on byes, no. Have a path forward as if they didn’t trample the bytes.
* JB: Can we get that written down somewhere?
* KG: Or in Khronos issue would be ok
* AB: **Will add links and summary**.


### [Defining "Undefined Value" #2888](https://github.com/gpuweb/gpuweb/issues/2888)



* KG: Tried to write down thoughts. Hopefully makes sense.
* AB: Good summary. Not sure where we can go on which operations. Mulling internally.
* KG: Would it help, being a terminology issue, where spec says undefined in places. Could pick different word with newer definition, could pick ‘indeterminant value’ and that causes UB if branched. Not to add more words, but to be sure when we say ‘indeterminant’ we mean the new thing and ‘undefined value’ is uncategorized. Would that move us forward?
* DN: Yea, like ‘indeterminant’ Value in not starting with a ‘U’. Real question is does multiple observations of that indeterminate have different values and want to say no. Must be the same.
* KG: Calling that ‘non specified’ value.
* AB: Example internally is spec doesn’t require nan/inf preservation but a float / 0 would expect inf. If hardware doesn’t respect inf would get an unspecified value. Probably wont’ change on all uses unless the compiler optimizes. Don't’ know how to break apart to when it could occur. Would probably always get same result on hardware, but if compiler decides to do something different …
* KG: Things like that open up non-specified values, but i guess cause divergent behaviour. For infinit a / b * b, multiplying by b/b == 1. Works great unless b is 0.
* DN: Except if b is 0, compiler gets to pick answer if specced as unspecified
* KG: Yea, but if you did branch ….
* DN: Yea.
* KG: One thing mentioned it’s our imperative that the API should be safe and it merely should have consistent behaviour as much as it can. A bunch of UV concerns if what if you branch on them, is it unsafe? As long as it’s unsafe it’s ok. Need to be careful for time of use check but a general branch that writes yellow or red depending on value technically fine if indeterminate.
* AB: Shader can still produce incorrect results. I agree
* KG: So can agree have things in spec where we say they are undefined values
* AB: Goes to question of where sandbox exists in implementation
* KG: And also, friction between Undefined Behaviour, can anything happen when you branch on a / b * b. Answer should be no, but could choose anything that could possibly happen.  By having undefined value shouldn't be able to cause behaviour that you couldn't get with a defined value of type T. Doesn’t matter the value any behaviour is valid and you don’t have to choose one but you can’t choose a new value because it’s undefined.
* AB: Agree that would be nice but sounds like each use of that variable must give same result. Feel like classic Undef in LLVM don’t respect, each use of undef before poison/freeze could give a different value so makeup whatever during code gen.
* KG: Kinda like indeterminate value. Trying to hard to label things? Think it’s important. What problem are we trying to solve? Draw line between UB that’s safe and UB that’s unsafe. If we say top down all behavior is safe we don’t have to talk about it
* DN: Safety is not a property of ??? but a property outside. Browser says you won't see anything outside. 3 categories. If UB then program can do _anything_. 2 the value is chosen by system at run time and question is if observation of value is same over multiple observations. Program no longer runs as written, multiple values of same type or same value of type even though indeterminate.
* KG: Could fit as UB, UV or indeterminate or non-specified. Or arbitrary value.
* DN: Yea
* KG: It’s a value, could call out similar to frozen.
* DN: Could write that intent. We record intent and let things catch up. Not fussy about specific words but think getting categories correct is important
* KG: Satisfied some fear leaving doors open to UB in spec. Don’t like when folks appear and say have undefined behaviour there unsafe API. Good point safety is more meta topic. Should check to see what we say from meta perspective. Meta intent is that it’s safe.
* DN: Filed issue 3.5 years ago to get to bottom will dig up.
* AB: API spec has security consideration section. Our goal is to maintain sandbox and be safe even if shader has bug
* KG: Even if WGSL spec had API spec pointer to security guarantees could be helpful
* JB: Think of C vs unix. C has undef behavriou but unix says if kernel crashes it’s our bug. Don’t care what c compiler does. We’re similar. We don’t promise what shader does but the API promises you don’t break browser sandbox.
* DN: When api launches pipelines and draw calls it’s giving resources to shader to operate in. Reading outside of those resources is what the browser is operating.
* JB: But shader isn’t going to corrupt JS world. There is a GPU boundary.
* KG: Helpful to say, maybe in api security considerations, if you get UB then it’s still safe but you won’t know what it will do
* GR: Not sure if folks will group for UB and never believe what you say. Maybe just don’t use that word which is easier then saying trust us. Pick another word and avoid problem.
* JB: Not concerned about trolls. Need to be clear and help implementors and users. Unreasonable people will be that way.
* AB: We don’t say UB a lot. One major place is referring another spec so would have to go dig to find it
* KG: Think i’m satisfied, will try to write a line or two.
* DN: One outcome is we get names for the 3 value categories
* AB: And have to go through and categories everything.
* KG: We can do that. Will assign to myself.


### [Uniformity analysis of local variables #2859](https://github.com/gpuweb/gpuweb/issues/2859)



* AB: JP implemented uniformity analysis in Tint and we had a lot of discussion about meanings. A lot focusing on if you ref a local variable it doesn’t say in the spec, it may mean a single node with errors to and from but if you override variable by putting a non-uniform it’s dirty’ed for the rest of the function. Could do data flow analysis and keep better track. That’s what Tint does as we thought it was right thing. Tried to jot down what we came up against and break down to making decisions before spec’ing to get consensus that’s how we should treat it . Should we treat values as having a data flow like model. If aggregate value, in spec if ref to variable alone, even into field, we think it’s OK to pollute everything. Not too many cases where you would be concerned. 2 builtin values that are uniform. Could add a note about splitting into separate input param where uniform ones are by themselves and analysis could handle. Easy enough to be split. Last one is how smart for data flow analysis. We haven’t defined it yet we have options for conservative or not. Some examples for each.
* JB: These are all matters of resolution. Granularity of analysis. Is it var related or assignment that we relate or do something more specific. Most concerned is that we write down and put into spec. Don’t think we should have different implementations. One gives lots of uniform values and other doesn’t. Don’t want to end up in that situation. Whatever we do it all has to be written down. How hard do we want to work, how much memory and time.
* AB: Agree
* JB: Analysis is what makes frontends slow/expensive.
* JP: When landed saw a 10-20% perf hit on frontend. No effort to make fast, just to make work.
* KG: LIke number of things, called out feels like generating SSA values. Like that polluting aggregates is a good place to stop trying to be smarter. Think polluting aggregates is fine and if folks want can do like said. Can override non-uniform with uniform uniformly and have  a deadzone of uniformity where it’s non-uniform and then uniform feels weirder. Wonder if it’s both simpler and tolerable to say if you want it uniform make a new variable.
* AB: That’s an option.
* KG: You already did the hard thing?
* AB: We did. With new thing didn’t want to phase in extra warnings and errors. Not as well spelled out so guessing as intent. We attempted to be as helpful as we could. Forcing to not-rename to a temp variable didn’t feel as helpful vs knowing what hte value is and what control flow nodes associated with. JP can comment on complexity. Didn’t seem terrible but not easiest thing to take away from spec, lots of text needed
* KG: And examples
* AB: Those would be helpful
* KG: Some probably omes down to implementation complexity. Don’t have a good feeling fro in Naga
* JB: Going to be a bit weird. Have identifiers for expressions, good place for metadata, for statements have tree. Rust doesn’t like pointing into other things. Wondering about that. We’re going to have to do it for other reasons, but change, but one of several. Not more concerned then about other things.
* KG: Would like to hear from apple also. Let’s not resolve until Apple weighs in. But sounds good. Modulo hearing from Apple, consensus is we can try to do this and see if we fall short or if it’s too hard.


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, May 24, **11am-noon** (America/Los_Angeles)
* [Special case uniformity of arrayLength](https://github.com/gpuweb/gpuweb/pull/2846#) PR#2846
* 