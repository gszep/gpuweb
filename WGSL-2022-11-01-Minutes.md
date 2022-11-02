# WGSL 2022-11-01 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** KG, DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** **(APAC!)** Tuesday **4-5pm **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-10-25 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1a0fVUmAlXX9JiTMlI1nr0Iuy9UjIeOBKWEMmnreSoOk) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * **Myles C. Maxfield**
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * Dan Sinclair
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
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * **Shaobo Yan**
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
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



* FYI:  Landed [Add draft text/wgsl media type registration. WGSL uses UTF-8 #3542](https://github.com/gpuweb/gpuweb/pull/3542)	[[wgsl] Encoding for shader text needs to be specified · Issue #565](https://github.com/gpuweb/gpuweb/issues/565#issuecomment-1297440745) 
    * Regarding the BOM:
    * David found RFC 3629 says that if only UTF-8 is allowed then the protocol SHOULD forbid the BOM.  David updated the PR to forbid the BOM. Oguz approved, and David landed
* FYI:  Prevent abstracts outside of const-expressions
    * PR: [#3547](https://github.com/gpuweb/gpuweb/pull/3547)
    * Previously had special rule for array indexing, but really should be a general rule that abstract can’t be involved in non-const-expressions
        * Oguz, Ben, Antonio, David approved.
        * David merged it.
* 


### APAC meetings during Nov and Dec



* We will be doing more APAC meetings during Nov and Dec
* **We are also moving APAC slot one hour earlier** (to 4pm Americas/Los_Angeles), though this will drift again in early November with USA’s timezone change.
* WGSL schedule:
    * November 01: APAC
    * (Meeting timezone change)
    * November 08: APAC
    * November 15: Non-APAC
    * _November 22: **Quorum Check: APAC? **_Yes, APAC
    * (US Thanksgiving Nov24, Thursday)
    * November 29: APAC
    * December 06: Non-APAC
    * December 13: APAC
    * _December 20: **Quorum Check: Non-APAC? **_Yes, Non-APAC
    * (Christmas Eve&Day Dec24&25, Saturday&Sunday)
    * _December 27: No Meeting_
    * January 04: APAC
        * DN: Suggest we cancel. Also, the 4th is a Wednesday
* Note: Based on our current issues remaining for v1 though, we are very likely to cancel meetings due to lack of discussion topics, so we can likely be liberal with skipping meetings.


---


# ⏳ Timeboxes (until 4:15)


### [wgsl: add way to get special values for a given numeric type (e.g. max, min, lowest) · Issue #3431](https://github.com/gpuweb/gpuweb/issues/3431)



* DN wrote and landed [https://github.com/gpuweb/gpuweb/pull/3561](https://github.com/gpuweb/gpuweb/pull/3561) which documents the extreme values for scalar concrete numeric types.  That’s the “let’s document this” solution without adding functionality.
* Proposal:
    * f32 maxNormal&lt;f32>();
    * f16 maxNormal&lt;f16>();
    * f32 minNormal&lt;f32>();
    * f16 minNormal&lt;f16>();
    * 
    * i32 max&lt;i32>();
    * u32 max&lt;u32>();
    * i32 min&lt;i32>();
    * u32 min&lt;u32>();
* KG: Looking at C++, min and max give the min and max for normal range, and you have to ask specially for the denormal value.  So that would be max&lt;f32>() to give the above.
* AB: Still in favor of David’s PR, to avoid templated parameters on function calls.
* KG: I would like to have these things.  Unless you find it onerous or actively gross.
* AB: I don’t like foot in the door for templates. Would rather bake it into the name.
* KG: Would be more tolerable if it were max_f32?
* AB: Yes, that’s more palatable
* DN: We do have bitcast which takes template-likes, fwiw. 
* MM: which of e.g. min() and min(x,y) are more common?
* DN: min(x,y)
* MM: How about we rename max&lt;i32>() to maximum&lt;i32>()?
* DN: That would help
* JB: Think there’s value to having something you can write.
* AB: Define your own const
* KG: Prefer to have a builtin.
* KG: minValue maxValue would be nicer for me, but don’t care that much.
* MM: That makes it harder to mess things up. Probably a better idea than “maximum”
* KG: confusing either way you slice it.  Verb vs. noun.
* MM: Ok, next iteration:
    * Functions named maxValue&lt;T>(), minValue&lt;T>().   We have precedence of the bitcast with type template.
* TR: Think it would also be tolerable to have max_f32(), min_i32(). (Without expressing a preference).
* MM: The reason I prefer angle brackets, it seems cleaner.
* AB: I can live with Myles’ 2nd iteration.
* _~~Resolved.~~_
* TR: It’s confusing for integer min to be a very negative thing, but min float being a small-magnitude positive value.
* KG: Historical precedent from C++.  There is “lowest” for the most negative value in float types.
* TR: Can always use sign bit.
* ZJ: Other names are “lowest” and “smallest”
* TR: It’s inconsistent with the min(x,y) function.
* KG: **Need to check, make sure min says what we think it is. Otherwise, we have consensus for maxValue&lt;T>().**


### [Runtime behavior of NaN-producing math functions · Issue #3365](https://github.com/gpuweb/gpuweb/issues/3365)



* DN: Now that we’ve landed indete	value, I think we’re done here. I don’t want special values to box us in for backwards compat. So I think we should let these be indeterminates.
* MM: Internally, we made a list function by function.I can post that here though? Though it is long
* KG: **Yes please post, and we’ll take a look**.


### [Accessing an "invalid memory reference": Trapping behavior · Issue #3272](https://github.com/gpuweb/gpuweb/issues/3272)



* MM: I thought we got close to a conclusion here, but months ago?
* KG: I think we are just waiting for PR
* DN: Interesting that these are paired together. I feel like this here is pushed by not wanting to have doubled shader size, but in the previous issue, we’re discussing adding bunches of more edgecases.
* MM: I think it’s partly about starting from a position that we can amend later.
* DN: I’m saying that if e.g. sqrt(-5) -> 0, we can’t later expand it to allow receiving NaN
* DN: Forward compat spec would be, return indet, and NaN is a valid value for it.
* 


---


# ⚖️ Discussions


### [Designing a uniformity opt-out #3554](https://github.com/gpuweb/gpuweb/issues/3554)



*  MM: What about a uniformity opt-in?   
* KG: Want developers to have to think about it first. Don’t want to start with the confusing behavior to start with.   Want the guardrails on by default.
* KG: would like to see a proposal for which functions would need to be marked with nonuniform options.  Eg. what entry points are modified.  Generally I like if annotations for builtins for cleanliness.
* DN: Ben made comment that one thing he doesn’t like with new names for builtins, is that they don’t actually do different things.
* DN: Also, in v1, we have just one level of scopes, but we expect to have more scopes in the future. The number of builtin names you’d need would multiply.
* MM: I believe it’s group opinion  that we need UAn for compute shaders for safety.
* DN: I do think that question is still open, e.g. other ways to clog the gpu
* JB: Does WebGL have this problem?
* KG: You can write very slow shaders in webgl
* MM:
* JB: I just wonder if we’re adding any new risks here or not, vs webgl?
* DN: 
* TR: Just an idea, could you have a “more trusted” mode?
* JB: One nice thing about AB’s thing, attributes on vars and argos, plus attribs on statements are both useful things. I could imagine you want both. We maybe don’t need to choose, it might be useful to do both. 
* KG: Whatever gets us consensus faster.
* JB: Yes. Also btw we do have a “collective “ section
* AB: Won’t always be builtin functions at fault, e.g. descriptor arrays (when we get to that feature).  Attributes are the lowest cost thing right now.  Gives you the freedom to modify your code in the smallest scope way.  E.g. putting it on a comparison, rather than the whole if-statement.  Once it’s a statement, you can’t get below a certain level of granularity without refactoring your code.  Attr
* DN: I added an example of a user experience where the user knows better than UAn, and diags tell them what’s wrong, and they say “no that’s fine actually” when they mark with an attrib.
* DN: Flipside is when an author is instead merely willing to accept bad/wrong derivatives, and annotates instead *acceptability* rather than *actually correct*. Like “pretend uniform”.
* AB: SPIR-V has prior art: has Uniform decoration and UniformID (for scoped). And for descriptor arrays it has a “non-uniform” control for descriptor arrays.
* KG: Have several options here.  My dream was how Rust has unsafe block and unsafe expressions, and also unsafe annotations on functions.  The MVP for what we need would be annotations on functions. What would make me super satisfied is function-level annotation. That’s tolerable and minimal.
* JB: Have a question. Talking about attributes.  But putting it things on stuff we don’t annotate now.  Types, return types, parameters, struct members. We don’t put them on expressions yet.  When someone says we put attributes I think of putting them on things we already have.  Are you suggesting putting on expressions.
* AB: Yes, that’s a possibility.
* JB: Seems to be roughly isomorphic between unsafe-expr and attr.
* JB: Surgical approach seems not acceptable to Unity.  Need to have “a” opt-out which can be used with cooperation with the code generator in the bulk way.  Would prefer surgical,...
* KG: In extrema could have unsafe-annotations on all functions.
* AB: Unity’s only requirement is that tooling can generate it for them.
* JB: But don’t want it strewn all over.
* KG: You only really need those when certain spots. If your tooling is smart enough about which have to be non-uniform calls, then it can emit more surgical annotations/wrappers as needed.
* TR: Could have subgroup operations assumeUniform and makeUniform (latter would pick first lane’s value), to make it actually uniform.
* TR: Don’t like mangling the functions. 
* KG: Most people don’t like those.
* EG: Think the premise of wider opt-out is needing a standard mechanism.  Ok… resetting understanding.
* MM: Have said before. Jury is still out on what the security requirements are.  It may actually be the case that both derivatives and barriers require the analysis.  That could be the outcome.
* AB: Touching on makeUniform. We have workgroupBroadcast proposal for compute shaders. But don’t think it’s clear for fragment.
* TR: For fragment, need quads to cooperate.
* AB: This is where we have the issue of whether we have the invocations in the quad actually getting together again to supply their values.
* MM: Is there a quad barrier?
* TR: No.
* AB: There is a vulkan extension for subgroup uniform control flow that moves the reconvergence requirement down to subgroup level rather than whole invocation group.  But that’s not universally available to where we’re targeting WebGPU.
    * VK_KHR_shader_subgroup_uniform_control_flow
* KG: You can kinda do certain transforms to incur total reconvergence but it’s not trivial or great.
*  
* DN: Concretely how about:
    * `@uniform` and `@fake_uniform`  (bikeshed name).  Applicable to various things, and captures programmer intent.
* EG: In Rust practice, you use a single ‘unsafe’ construct but document it as you go (in comment?)
* TR: ‘assume_uniform’ does the same thing.
* JB: Watch out for Rust’s unsafe. Rust unsafe functions and blocks were incorrectly unified in first versions.  Two different concepts actually.  Let’s be cautious about unsafe functions and unsafe blocks.
* KG: That’s why I want to look to someone else.  Otherwise we may fall into the same trap.  Like, they had troubles with this, but then they fixed it.
* AB: If nobody is allergic to using an attribute as the solution, then @assume_uniform can capture both cases. Then I can take this and make it concrete. Then the discussion is what things to annotate.  
* JB: Consider future direction.  The analysis can report an error whose cause threads back through several functions. When that occurs, we can provide detailed diagnostics. However, it will be annoying for users. Programmes know where they thought things were going to be uniform. They have no way to say their expectations about their program.  Because we can’t capture programmer intent at that granularity, we’ll cause difficulty for those difficult spots.  Think we’ll want to put annotations on function-level boundary. User will want to say how each function will behave. Produces a summary on each function of effect.  Will want the analysis to check their expectations.  That will be useful just for the sake of being able to debug.  Let’s keep that in mind, maybe not necessary for V1.
* KG: Namewise, I want the name to be negative and scary.  Really like “unsafe” for that reason.  Negative connotation is really important. Want people to not want to apply this opt-out.
* MM: +1 on what Jim said earlier. Historically expected an author to assert that a particular variable is uniform. Would cause an error. Good for cutting off the length of action-at-a-distance.
* JB: +1 Kelsey’s point about negative connotation.  People are swayed by connotations of ‘unsafe’ and ‘use strict’ in JavaScript.
* AB:  Understand that argument. Where you end up changing your code differs depending on positive or negative connotation.
* EG (in chat): take_off_kneeepads
* ZJ (in chat): ignore_non_uniform


### [Uniformity analysis is far too strict #3479](https://github.com/gpuweb/gpuweb/issues/3479)



* 


---


# 📆 Next Meeting Agenda Requests



* Next week: (**APAC**!) Tuesday, November 08, **4-5pm** (America/Los_Angeles)
* **Write any PRs you’ve promised!**
    * There are no other v1.0 issues outstanding (other than the ones on this agenda!) that are not blocked awaiting proposals.
* 