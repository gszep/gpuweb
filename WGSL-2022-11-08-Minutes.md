# WGSL 2022-11-08 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** JB

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** **(APAC!)** Tuesday **4-5pm **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-11-01 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1f2c5pgiSovyJsU82r0B8PR450Q2kzNVClXtvatxvjIE) 

 

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
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * Dan Sinclair
    * David Neto
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Price
    * Rahul Garg
    * Ryan Harrison
* Intel
    * **Hao Li**
    * Jia A Chen
    * Jiajia Qin
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * **Shaobo Yan**
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Microsoft
    * Damyan Pepper
    * Greg Roth
    * Michael Dougherty
    * **Rafael Cintron**
    * **Tex Riddell**
* Mozilla
    * Erich Gubler
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
* Mehmet Oguz Derin
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson


---


# 📢 Announcements


### Double-check your timezones!



* Meeting is still anchored to America/Los_Angeles, but America/Los_Angeles switched from its Daylight Time to its Standard Time this past weekend.
* Please refer to the times listed here: [https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting&iso=20221108T16&p1=137&ah=1](https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting&iso=20221108T16&p1=137&ah=1) 
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



* Landed [https://github.com/gpuweb/gpuweb/pull/3577](https://github.com/gpuweb/gpuweb/pull/3577) Allow slightly more expressions for pointer arguments #3577
    * Allow using intermediate let-declaration of a pointer. Still the same expressive power, but makes it more regular/consistent.
* 


---


# ⏳ Timeboxes (until 4:15)


### [wgsl: add way to get special values for a given numeric type (e.g. max, min, lowest) · Issue #3431](https://github.com/gpuweb/gpuweb/issues/3431)



* KG
* BC: min vs lowest is tricky and really does mix people up. Rust’s min is different than c++’s min. 
* AB: Not fond of the :: syntax, since we don’t use that anywhere else in the language
* KG: We kind of had consensus on the syntax, so my proposal was more about the names, fit into whatever syntax was agreed upon (e.g. max&lt;T>, min&lt;T>, and minPositive&lt;T>)
* AB: Template style works for bitcast because it’s a keyword - it doesn’t work for other identifiers that could be redefined
* KG: We had a rough consensus on using brackets - did that break?
* AB: Yes, I think so - [since last meeting,] DN thought it was going to require too much lookahead to use angle brackets on ordinary identifiers
* (DG: Does the IEEE standard give these values names?)
* KG:** Tabled for more discussion.**


### [Runtime behavior of NaN-producing math functions · Issue #3365](https://github.com/gpuweb/gpuweb/issues/3365)



* KG: Mozilla’s consensus was, let’s take in the direction of using the things called “indeterminate values’
* AB: Google is also happy with that
* &lt;TENSE SILENCE>
* KG: unknown
* KG: **Let’s use “indeterminate values”,** and if that is not the consensus. . .
* DG: Apple would be happy with that also
* AB: Okay, **we’ll back out the acosh or whatever, and just make everything say the right stuff**
* DG: sounds okay


### [Accessing an "invalid memory reference": Trapping behavior · Issue #3272](https://github.com/gpuweb/gpuweb/issues/3272)



* KG: “trapping behavior” is a slightly misleading name
* KG: MM didn’t have proposals put together before he left - does Apple have anyone to pick up the baton?
* DG: **Let’s table for now**


### [wgsl: consider renaming frexp result fields #3569](https://github.com/gpuweb/gpuweb/issues/3569)



    * Rename { .sig, .exp } -> { .frac, .exp }  
    * Prompted by the fact we can document the mnemonic “fraction and exponent”, and it really is a fraction.
* JB, BC, AB: +1
* DG: Do we have an established practice around spelling things out versus using abbreviated forms?
* KG: I like using the two parts of the ‘frexp’ name, ‘fr’ and ‘exp’, because if you know how to use the function, then you know what the parts are names
* AB: We have another struct with ‘fract’ and ‘whole’
* KG: ‘fract’ and ‘exp’?
* AB: fine
* KG: Let’s be consistent with our previous decision, and use consistent names between all our structs
* DG: Was this an HLSL vs GLSL distinction?
* AB: Most other languages use a pointer output argument
* KG: HLSL is ‘frac’, GLSL is ‘fract’
* Current practice in the spec is ‘fract’, so we should use that consistently
* **Consensus: { .fract, .exp }**
* **If anyone wants to s/fract/frac/, file a new issue.**


### [Clarify mathematical value of float literal #3565](https://github.com/gpuweb/gpuweb/pull/3565)



* DN: I copied the ECMAscript algorithm for decimal floats: 20 significant digits. (~66.4 bits preserved)
* DN: For hex floats (not in ECMAscript), I used 16 significant hex digits. (64 bits preserved)
    * The key review question: is 16 appropriate here. I think so.  A double only has 53 bits of mantissa (if you include the hidden leading 1 for normal numbers)
* No opposition: **Consensus: take**


---


# ⚖️ Discussions


### [Designing a uniformity opt-out #3554](https://github.com/gpuweb/gpuweb/issues/3554)



* KG: What I really like about using the negative term instead of the positive term is that it suggests that things are normally safe but we’re using an escape hatch. Flags code for extra-careful review.
* KG: When reviewing an unsafe block, the goal is to become sure that what’s in the unsafe block is indeed safe - it’s just that the compiler can’t see that
* AB: If we were to write an attribute-based proposal with a placeholder name, would that be all right? Would that let us move forward on substantive questions like, where can this be used? Separately from naming
* DG: re: attribute: In one of the issues, someone was porting rendering and there was a variable input to the shader that was uniform, but the analysis had no way of knowing that. I support the uniform if it supplies additional information about the program that the compiler can’t determine.
* KG:  And if that’s violated, we say, “that’s on you”?
* DG: yes
* DG: Unless people think we can get away without the ability to mark knowledge that the programmer has
* KG: We should go as far as we can in that direction
* TR: I made a long comment on the issue, but I recently found out about Vulkan’s “dynamic uniformity” requirement for barriers - they’re not quite the same as what I expected. This kind of broke the idea that you could simply attribute values and get a guarantee of uniformity in control flow for barriers. Marking individual values with attributes wouldn’t be sufficient, but you can create control flow that can’t be marked up sufficiently.
* AB: It depends on the use case. If you could put an attribute on an expression, you could put it on the condition of an if, or put it on a let and then use that in the if
* TR: It could be non-uniform when false, but uniform when true. You could have two if statements, and in the … (See [comment](https://github.com/gpuweb/gpuweb/issues/3554#issuecomment-1308056266) on issue)
* AB: If you could put the attribute on the function call, then you could put it on the builtin itself, avoiding a proliferation of attributes on builtins. This gives it a lot of flexibility
* TR: There’s a difference between a value being uniform, and the control flow operating on that value being uniform
* AB: The key for the analysis is that what causes a problem is a collective operation called from non-uniform control flow. That could depend on the uniformity of a value. But with an attribute on expressions, you can always make it work.
* TR: But also uniformity scope matters. So I don’t know if we can capture everything we need. We need a simple escape hatch for v1.
* AB: The bare minimum is to put an attribute on function calls. That would solve everyone’s problems, it might not always be idea but it would suffice
* KG: For 1.0, messy suffixes might not be so bad
* AB: That would also suffice
* KG: Nobody prefers it, but it’s shippable
* BC: We would never be able to get rid of it
* KG: We would have to support them forever, but we could discourage people from using them once we have something better. And they’re not hard to implement
* BC: Is that preferable to module scope?
* KG: Yes, preferable, even for v1. Module scope will become something people slap on as a habit, without thinking about it.
* BC: I prefer function-scope annotations to littering the language with functions we don’t want to keep. That would just turn off errors on a particular function
* TR: How would that affect the rest of the analysis, using the results of those functions?
* BC: You could proceed with the analysis, but simply silence the warnings. Or, you could avoid propagating the non-uniformity
* AB: If I branch on a non-uniform value in a flagged function and use a barrier, would I get an error?
* ZJ: Is this an annotation that works within a function, or does it affect the function’s callers as well?
* KG: If you mark a function, you’re saying, just pretend this is uniform. So you would not get errors if you used its value
* ZJ: Maybe if we are going to have function-scope opt-out, where we put an attribute on the whole function, then does that mean that any expressions in the body of that function will not produce errors?
* KG: yes
* TR: It makes much more sense to preserve the analysis within the function, but mute the errors
* BC: I’m worried about the implementation complexity of determining where to place the attributes, if you wanted to avoid placing them unnecessarily. Right now, we only return one error and then bail, so having to identify all the places where it’s needed would be a major effort
* AB: Having an attribute on a function call would clearly work just as well as having variant functions with ugly names. Unity’s feedback is, they don’t care how we do it - just as long as the tool does it for them
* BC: **Let’s discuss internally**
* KG: I do like the equivalence between the attribute on a function call and calling a function that has a suffix, it’s nice to have those two equivalents. I don’t quite understand the implementation complexity concerns around collecting multiple errors
* KG: Can we do other things to the language, like the uniformRead primitive (#2321), that would make these annotations less necessary in the first place?


### [Workgroup Broadcast · Issue #2586](https://github.com/gpuweb/gpuweb/issues/2586) 



* AB: How about #2586: There could be two useful variants of this function: One would help Raph out with his barriers in his compute shader; and one that would be a general easy-to-handle tool for people that need to make something uniform, where the compiler would allocate the workgroup memory for you. There are reasonable arguments for both variants.
* KG: Is either of these a “more primitive” operation that you could implement the other in terms of?
* AB: Yes, the first one is more primitive
* KG: If we can provide a primitive that satisfies the concern, then we should provide something low-level that people can use themselves
* AB: They have different tradeoffs in flexibility. The second one doesn’t need two barriers; the first does. But, if you already have to store to workgroup memory anyway, then maybe the first one is all you need
* KG: Just give them different names?
* AB: Maybe, but could be confusing
* KG: The first one feels less like a broadcast and more like a read, whereas the second one seems more like a broadcast
* AB: I’m not too particular about the naming
* KG: I could write that up as a proposal
* AB: Just the one version, or rename and have both versions?
* KG: Have both versions
* **Decision: continue to discuss offline**


### [Uniformity analysis is far too strict #3479](https://github.com/gpuweb/gpuweb/issues/3479)



* KG: The other low-hanging fruit were already solved, we’re just waiting for implementation updates, right?
* AB: We also discussed whether the analysis should look at constant expressions in terms of access into composite values. The analysis right now stops at any aggregate member access, but we could make it see further when the access was simple. If we have the opt-out, making the analysis more precise may not matter
* KG: People can always just not use aggregates if they want things analyzed separately. Within a shader you can always just scalarize things yourself. But perhaps that could be post-v1?
* AB: I don’t think that would suffice on its own - if people were going to use the opt-out, doing finer-grained aggregate analysis isn’t going to make them not use it


---


# 📆 Next Meeting Agenda Requests



* Next week: (**Non-APAC**!) Tuesday, November 15, **11am-noon** (America/Los_Angeles)
* **Write any PRs you’ve promised!**
    * There are no other v1.0 issues outstanding (other than the ones on this agenda!) that are not blocked awaiting proposals.
* WGSL schedule:
    * November 15: Non-APAC
    * November 22: APAC
    * (US Thanksgiving Nov24, Thursday)
    * November 29: APAC
    * December 06: Non-APAC
    * December 13: APAC
    * December 20: Non-APAC
    * (Christmas Eve&Day Dec24&25, Saturday&Sunday)
    * _December 27: No Meeting_
    * January 03: APAC
        * DN: Suggest we cancel
    * Note: Based on our current issues remaining for v1 though, we are very likely to cancel meetings due to lack of discussion topics, so we can likely be liberal with skipping meetings.
* 