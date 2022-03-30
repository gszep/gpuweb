# WGSL 2022-03-29 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️ Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: ** [WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-03-22 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1UB_e1nJZ4fni5WkRaGmj3Ynj7Y61Mr9bJHOPz6j14oQ/edit#heading=h.x5y79724c083) 

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
    * Antonio Maiorano
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
    * Rafael Cintron
    * Tex Riddell
* Mozilla
    * Dzmitry Malyshau
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


### Shifting timezones!



* North America recently changed!
* Europe just changed this weekend!
* Australia and New Zealand change in a week, but the other direction!
* **We use Americas/Los_Angeles, and the Google Calendar invite should show correctly for you.**
* Just in case: [https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting&iso=20220329T11&p1=137](https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting&iso=20220329T11&p1=137)


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


---


# ⏳ Timeboxes


### [Make colon following switch case optional #2678](https://github.com/gpuweb/gpuweb/issues/2678)



* AB: Different from C; in C the colon serves a purpose. We have comma separation.
* MM: Can we remove fallthrough.
* AB: That’s different functionality.
* KG: We do require braces.
* MM: And that’s good.
* AB: Should we ban it?
* MM: Don’t do that.
* KG: Ok making it optional.
* RM: Ok with it optional.
* _Consensus: make it optional._


### [editorial: discuss parsing ambiguity: type construction, function call #2539](https://github.com/gpuweb/gpuweb/pull/2539)



* (Want Robin’s review)
* _Consensus to merge_
* MM: As aside, we might eventually want something unambiguous, but that it would still be possible with more work.
* DN: It shouldn’t be ambiguous per se, though a token might be need a lookup table to disambiguate its kind. (but the grammar would be sound still)
* 


### [Define edge case behaviour for f32 to f16 conversions · Issue #1201](https://github.com/gpuweb/gpuweb/issues/1201)



* (closed after discussion offline)


### [wgsl: Rename smoothStep to smoothstep #2175](https://github.com/gpuweb/gpuweb/pull/2175) 



* KG: wikipedia spells it uncapitalized.
* _Consensus to make this change._


### [context-aware tokenization: Sometimes >> is better parsed as two copies of > (similar for ]] ) · Issue #2092 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/2092) 



* MM: Spelled out 4 options in the issue
* AB: Like 1 better than 2.
* RM: Don’t like option 1.  C++ had option 2 for a long time then used option3 . Which is more usable. Both are fine.  Option 4 goes completely around the issue.  Option 1 reads weirdly.
* AB: Don’t think the grammar is the place to enforce clean-looking code.  If 1 is the easiest, then in favour of that.
* MM: 3 is what programmers are used to. Give them that if possible. 4 is also good.  Benefit is it makes order of operations explicit.
* DN: Really don’t like 2.  If I had to solve it quickly 1 is fine with me.  Somem time ago experimented with 3 and got stuck.
* AB: Given constexpr is coming up, want constexpr as array element count, and make sure it’s not ambiguous there.
* MM: There’s no ambiguity, just about layering.
* JB: How far ahead do you look to decide if it’s one or the other.  E.g. array length could be 4 >> 2.
    * Let a: array&lt;array&lt;f32,4>>2>> = ….;
* KG: Pushes me to 4.  Life gets a lot easier when you make your nesting syntax distinct from other syntax.
* JB: It’s fun to know operator precedence, but it’s probably not good practice to depend on it. [i.e. using a builtin function makes order explicit]
* RM: That example convinces me that 4 is good.
* MM: Make it symmetric: do all the shifts as functions.
* AB: If you make it words, you can be clear about sign extension or not.
* _Consensus: replace all shift and compound shift operators with explicit function builtins._
    * _Possibly named shiftRight, shiftLeft_
    * _Or rightShift, leftShift_
    * _Or shl shr_
    * _Or lshr ashr shl_


### [consider namespaces for WGSL · Issue #777](https://github.com/gpuweb/gpuweb/issues/777)



* KG: Given from 2426 we want unnamed spaces.
* DN: Not agreed to unnamed namespaces: leads to weird poking-through behaviours for entry points and overrides.


---


# ⚖️ Discussions


### [Literal unification, bottom-up #2227](https://github.com/gpuweb/gpuweb/pull/2227)



* Previously:
    * JB: Want one week to review. Major changes done since I last read it in detail.
    * KG: Let’s plan to revisit next week; bias to land. Give folks chance to give feedback
* JB: No comments
* RM: What I’ve seen seems ok.
* MOD: My review of mechanical thing is fixed by now.
* _MERGED._


### [Add creation-time expressions #2668](https://github.com/gpuweb/gpuweb/pull/2668)



* MM: const x = 5;  is the type AbstractInt, or in32.
* AB: In this PR, it’s i32. When you add ‘const’ as a declarator, then it becomes abstract
* MM: Glad there is an answer.  No strong opinion about what the answer should be.
* MM: Why have @const  when authors can’t write it.   Why not have an explicit list in the spec.
* AB: Thinking forward, we can add that feature later.  It’s easy to explain it this way.
* AB: Happy to bikeshed the name like ‘constfn’ vs `@const fn`.
* MM: Think user-defined constexpr functions is a big thing, and needs careful consideration about how it’s shaped. So for now want to not add a syntax for it.
* AB: I want a single place to look in the spec to know about a function, rather than look in two places. As long as it’s inline it’s important.
* MM: Inline is no problem.  Could be a column, or a sentence.
* KG: Agree with Alan somewhat.  When I look at documentation I often just look at a declaration. So it would be cool to see a ‘const’ there, or @const.  You could argue it’s just a table with no borders.
* MM: I view the whole spec as the thing you’re describing.  If someone made a list of everything in the std library that info would be in the list. 
* DN: The way AB has it is there’s an @const attrib, but users can’t use it, so this would be something we can harmlessly changed later with no user-visibility. Did we add a note that this is just for explaining, and not “real” syntax?
* MM: RIght, this is an editorial comment.
* MM: Confirm you can still make a let at internal function scope.
* AB: Yes.
* MM: Not blocking. If I wrote a PR to replace @const with something that is  inline next to the function, would that be a harm?
* AB: would need to see it.
* KG: Currently prefer @const.
* **MM: Ok, let’s go forward, and I can write a PR.**
* **AB: I’ll rebase, convert to non-draft.**
* **KG: I’ll review**
* AB: Call attention that some builtin functions now take abstract numeric types, see if you agree with the set.  And also abstract types can be for scalars, vectors, and matrices. Not arrays and structs.
* MM: I’m most interested in the math functions working on these abstract values, and they don’t operate on arrays and structs, generally.
* AB: An odd case is frexp and modf, which return structs.  Decided one way, and open to changing to the other.


### [Why have literal suffixes at all? · Issue #2664](https://github.com/gpuweb/gpuweb/issues/2664)



*  KG: Are generic literals enough?
* MM: Example, if I write floor(1.0)   floor(67000.5)
* DN: Even after adding f16 types, will still resolve let x =  floor(67000.5) to 32-bit type because backward compatibility.
* MM: So compelling to want to make it as easy as possible to use f16.  So argue for suffix for f16.  Fewer characters to write.
* AB: Don’t want precedent to add suffix for every type that comes along.
* MM: Care more about f16 than 64-bit types.
* JB: Are these cases rare anyway because generic literals take care of a lot.
* KG: The problem comes because generic literals don’t bridge across builtin functions.
* AB: The key transition is transitioning to concrete type, e.g. at var or let.  When you concretize that way, how easy is it to get to the f16 than f32.
* AB: The way #2227 is written, when there is no additional context, it goes to 32-bit.
* DN: I could see an annotation on a function declaration to opt into f16 bits within the scope of this function body.
* MM:  This is a long term desire. Want to move in that direction, as easy as possible to use half-precision vs. single-precision.
* MM: So motivate ‘h’ for half-precision.
* AB:Alan Baker
    * let x = 1h;
    * let x : f16 = 1h;   // or     =  1;
    * let x = f16(1);
* AB: Not a lot more characters to do the cast.
* MM: It’s for every time you write a literal.  Death by a thousand paper cuts.
* KG: f16 is an extension, so kind of weird to make it as easy as using f32.  So you need some indication some places. Want to drive it closer to zero, but can’t make it zero cost.
* KG: h is novel.  Seems weird.
* AB: When you have ‘const’, that can remain abstract, and that helps a lot.  
* KG:   let f: f16 = 2000000000/1000000000;  If you coerced those to f16 literals, you’ve blown your precision, and you get the “wrong” answer that could have survived an f32.
* MM: I see symmetry with u vs. h.   U is there for “not the default”.
* GR: Not sure HLSL is a model to follow.  Think  f followed by bitwidth is much less ambiguous.
* KG: That would be my preferred way to solve it.
* MM: Don’t know that’s better than colon-f16.
    * Let x: f16 = 1;
    * Let x = 1f16;
* KG: Helps with multi operands.  Eg..   max(1h, 2h)
* MM:  Not worried about one particular use case.  It’s globally across a large source file.
* GR: Yeah, 1f16 looks pretty ugly.  I have to think about 1h. Others will react smiilar. Will it dissipate over time?
* KG: At least with Rust have prior art to that way of doing things.
* JB: In C, C++ having one name for types and another name for suffix, and a third name for a printf formats.  It accumulates. Having one set of lexemes is nice.  Making sure you don’t have to write it very often is another goal.
* MM: MSL does support ‘h’ suffix. So that’s prior art.
* MM: GLSL has f F, L, h, H
* David found: [https://github.com/KhronosGroup/GLSL/blob/master/extensions/ext/GL_EXT_shader_explicit_arithmetic_types.txt](https://github.com/KhronosGroup/GLSL/blob/master/extensions/ext/GL_EXT_shader_explicit_arithmetic_types.txt) 
* BC: If we allow this everyone will want a suffix for every data type.  Will we want suffixes for i8, i16, u16.
* MM: Don’t have opinions about that. Just care about f16.  If others want to add i8 suffix they can go ahead.  Have no preference for there.
* KG: It’s the fear of having a tide of many others that causes us to not want the ‘h’ now. So trade is accept ‘h’ but write down intent to not take more suffixes in future.
* DN: Would be part of the f16 extension.
* AB: So is the consensus just add h suffix, as part of the f16 extension.
* _Yes._
* GR: What about 64 bit.
* MM: Don’t expect doubles to be common, do expect half to be common.  So no special suffix required for 64-bit types.


---


# 📆 Next Meeting Agenda



* Next week: (APAC!) Tuesday, April 05, 5pm-6 (America/Los_Angeles)
* Identity constructors for matrix types [Issue #2704](https://github.com/gpuweb/gpuweb/issues/2704) [PR #2714](https://github.com/gpuweb/gpuweb/pull/2714) 
* Creation-time constants
* 