# WGSL 2024-04-16 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+is%3Aopen+milestone%3A%22Milestone+0%22), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+is%3Aopen+milestone%3A%22Milestone+1%22)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2024-03-26 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1aHozp5lH16ab7pVGJE8Rcn9TXAhkil9aKrXODH8T8Es) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send **dneto** a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Mike Wyrzykowski **
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * **Alan Baker**
    * **Antonio Maiorano**
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * **Kai Ninomiya**
    * **James Price**
    * Rahul Garg
    * Ryan Harrison
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
    * Greg Roth
    * Michael Dougherty
    * Rafael Cintron
    * Tex Riddell
* Mozilla
    * Erich Gubler
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * Teodor Tanasoaia
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* Unity
    * Brendan Duncan
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


# 📢 Announcements/Meta


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


### FYIs and Notable Offline Merges


### CG -> WG Transition



* See Corentin’s email about this group becoming a working group
* Contributors need to join the WG to continue contributions


---


# ⏳ Timeboxes (until XX:25)


### [wgsl: Add a carve out for AbstractFloat fract accuracy #4541](https://github.com/gpuweb/gpuweb/pull/4541) 



* DN: Ryan report using rule as in spec, every platform fails. Need pr to make all platforms work. Strongly recommend we land.
* KG: It's landing presently.


### [WGSL built in functions are very under specified · Issue #4527](https://github.com/gpuweb/gpuweb/issues/4527) 



* (untriaged)
* KG: 2765 is previous iteration, it's editorial and assigned to KG.
* DN: pow keeps coming up, did shallow dive. Brandon made PR with a rule. Matches GLSL, more strict then MSL, HLSL doesn't say anything. Go with Brandons PR and put that rule into spec. IEEE has 3 power functions this is aligned with inherited accuracy rule. In sense of POW we have an answer. Still editorial of going through list of exception cases in IEEE and writing what it means
* DS: One note, HLSL docs linked from bug has more then version we saw. May want to validate that table with our plan.
* KG: Previous consensus was various things. Would say we do what JS does except when we need to be faster. Later comments say based of IEEE. Assign to dneto?
* DN: Yes.


### [Compatibility mode: provoking vertex differs between OpenGL and WebGPU · Issue #4489](https://github.com/gpuweb/gpuweb/issues/4489)



* (untriaged)
* (this will mostly be bikeshedding)
* KG: two aspects, names which haven't changed that much from 3 weeks ago and related aspect of do we issue a warning for one argument flat. Shoudl talk name first and finish that. Folks are generally in favour of being `flat, either` and `flat,first`
* JB: This is syntax 2 in KG comment of 3 weeks ago.
* KG: for deprecation, things are more up in the air. Consensus seems to be that `flat` means `flat,first`. If that has consensus, not what i prefer, but think it's fine. Would add, if we did want to make ti a warning, deprecation doesnt' mean we'll removing it just telling people we recommend the other way and we could bring it back later. Given we have compat, don't want folks using 1 arg. Don't feel strongly, if we have consensus ot not warn and treat it as `flat,first` does that have consensus. Any objectives?
* JB: With just hte warning, in non-comapt, in compat …
* KG: Right, compat would say `flat` is error
* JB: And non-compat is warning?
* KG: Consensus is no warning. Would recommend implementations consider a warning, but up to group.
* DN: Would tend to defer to KN.
* KN: Don't feel strongly. Argument on one side is we end up with code in the wild when unshipping compat code uses 2nd arg when doesn't need it. Arg to show warning is to make sure folks put the thing when they write it instead of patching later. … both fine. It's a trade off. No easy answer.
* KG: Tolerable consens, move forward with no-warning and `flat,either`


### [clarify when const-exprs subexpressions are evaluated](https://github.com/gpuweb/gpuweb/issues/4558) #4558



* DN: Paired with 4551. Latest comment is on 4551 that shows examples to illustrate.
* JB: This seems simply an oversight. Intention is once you know denominator is zero, regardless of division itself being const, we call it an error. Making it not care if the division is run-time or const was the consensus.
* DN: That goes further then issue we've been discussing.
* JB: I'm sorry.
* DN: No, interesting and useful.
* DN: c1, false && 1. Should not type check and it's a const expression and it's short circuting. Do we typecheck short-circuted should resolve if that's checked. My intuition is type checking happens all over the place.
* BC: Jim is talking runtime / const expression.
* JB: Sounds like 4551 not 4558
* DN: Trying to conjoin the two.
* BC: Can be separated. One is eval through short-circuit, other is div by 0
* KG: These kinds of issues and complexities like if (false) { …. }. In that case the thing we're doing that we mandate we make a syntax(compile time) error not incredibly useful to devs. And is occasionally frustrating in that you get spurious compile errors because the code doesn't run. But because the code is bad in the branch we aren't running we fail to compile. Consider what we want is a signal to devs that we present the compile or warn? Same as with run-time expression it will compile and give you something
* AB: If we go down that road it adds a compilation to frontends to know what they can't emit in their backends because underlying backends have type checking rules that would fail to validate the program. There is a cascade of behaviours where const is most restrive, then pipeline, then runtime. Typechecking exists in the first one and always happens. You always know the type. Don't see the benefit where you write bad code and change a value and it starts/stops working. An array of -1 size just doesn't make sense. Could have a rule that types must always be known.
* KG: I think types should always be checked. Values I don't think should.
* BC: Debate is types and value expressions are intertwined so separating them isn't easy. Array types are different if sizes are different and sizes can be derived from const variables so a type check isnt' easy unless you draw line on what gets value computation. So need a ruleset of RHS on short-circut for things like array counts. While array counts is one of few things we have now, should consider user function templates and all other things that come with it.
* JB: Haven't finished reading issue, but has it been proposed that const-expressions as array lengs always be evaluated wherever they appear. 
* DN: That is one option, yes. It's completely consistent. It's consistent with my own reading of the spec but extra words could be used to clarify. Short circut section makes it sound liek you don't need to check at all. In my mind you always do type checking so always have to eval. And top-level const expressions are always evaluated. Could invent qualities of const-expr if in future versions. It isn't cutting us off from a future land of c++isms if we choose to do so.
* JB: Sounds like types are always check regardless of where they appear has consensus. And if so, that implies array length is always evaluated.
* KG: Yeah punt SFINAE in 2025?? :P
* JP: Interested in other implementers. For Naga, are you happy with having to walk down RHS for type checking and not eval?
* JB: Yes. we will always fully type check all of our expressions.
* KG: because of 4551 resolution no longer an issue.

Later discussion:



* JP: Looking at c0, should 1/0 be static rejected?
* JB: First example should compile without eror because we did not eval so no exception
* KG: If that's true, happy for that. Little weird you get better behaviour then in the if false case. but, eyes wide open that's weird, sure.
* JP: The reason we allow c0. Div by 0 is a validation rule. But we don't apply the val rule because we don't do the division.
* JB: I think it’s okay that `if false { let _ = 1/0; }` is an error, but `false && 1/0` is not. The way we’ve set up WGSL, constant evaluation is a thing that applies to expressions. `&&` is an expression, and `if` is not. If WGSL were an expression language, then it would make sense for `if` to also prevent the error, but WGSL is not an expression language.
* DN: Thumbs up.
* BC: Can extend argument to type
* JB: No. There’s no precedent for doing that. In dynamic languages like JavaScript or Python, things are checked if/when you get there, but WGSL is not a dynamic language, we’re statically typed. Standard ML provides the model here: there’s an “elaboration” phase where you do stuff that doesn’t require values, that comes before the “evaluation” phase. WGSL is the same. Type checking is part of the elaboration phase. WGSL’s constant evaluation and override evaluation are new phases we’re introducing between elaboration and evaluation. But except in the wildly dynamic languages like JS and Python, there is no language that uses values to determine which subexpression to type check.

    ```
    @compute
    fn main() {
      let foo = false && dpdx(1.0) == 0.0;
    }
    ```


* AB: Don't want to revisit type checking, want clarification on function call ones. Function calls are static call trees. So a derivative after false && is a validation error. I dont' think if this example is there. (See above example). dpdx is not a const function so would never be const-eval'd. But I think we should reject the dpdx there if it's in a compute shader tree. I think that was JPs point, we hadn't settled on that. Isn't the same as a value eval, it's a separate level of eval then what JB said was values.
* JB: Think this is what folks talk about for effects systems. derivatives are other level. They have types that we type check but an additional requirement they can only happen in certain contexts regardless of values.
* AB: Agree with that. \
JB: In deference to BC, C++ has that const if thing where they check based on stuff. Not entirely unprecedented but … awful.
* KG: Think situations where that is useful is subfields we aren't targeting with wgsl. Useful for template meta programming
* DN: Seen relevant cases where you have a fast pass for one architecture and one for a different one. Other level of consideration. Future we may want it, but design it when we get there.
* JB: No existing consensus for anything like const-expr-if c++ thing. Committee has not discussed it.


### [Clarify whether type checking occurs for short-circuited RHS expressions](https://github.com/gpuweb/gpuweb/issues/4551#top) #4551



* KG: Resolution is type checking always occurs.


### [Reduce byte-size limit for function and private address spaces · Issue #4545](https://github.com/gpuweb/gpuweb/issues/4545)



* JB: Same experience in Naga to blow stack before reaching nesting depth of statements.
* KG: Allowable to go beyond, consensus is to merge all of these. If impl wants to go further they can.
* JP: Fine with me. Only non-simple lower is array sizes. Do we want to also signal that hte limit is about the aggregation of the variables in a given address space. We say 8k for an array, but in reality you have to consider the size of all the private variables together.
* KG: Good distinction, worth noting. In line with how we give limits for other things.
* JB: Have option to declare an implementation restriction. If I make a jillion structs things go badly. To some extent, leaning on that anyway.
* Approved


### [Reduce limit for maximum number of elements in const-expression of array type · Issue #4546](https://github.com/gpuweb/gpuweb/issues/4546)



* Approved


### [Reduce limits for composite types · Issue #4547](https://github.com/gpuweb/gpuweb/issues/4547)



* Approved


### [Reduce limits for the number of case selector values in a switch statement · Issue #4548](https://github.com/gpuweb/gpuweb/issues/4548) 



* Approved


---


# ⚖️ Discussions


### [Feature request: textureQueryLod equivalent · Issue #4180](https://github.com/gpuweb/gpuweb/issues/4180) 



* DN: Did homework. Read math and have rough text to brain dump. Think we can go forward with basically previous consensus. Have much clearer picture and have text we can drop in and use.
* KG: Assigned to Oguz should we move to DN?
* MOD: David can handle it.


### [[high-level] Should we support aliasing predeclared enumerant values? (e.g. user-defined @location(pos) mapping to @location(position)) #4540](https://github.com/gpuweb/gpuweb/issues/4540)



* (AB/DN: This is actually coupled to #4539:  Deciding that those attributes are not shadowable, means we can’t really make aliases for these things. That’s because defining and using an alias requires identifier resolution to be operating, and that means we get back into the world where we’re going to accidentally shadow `position` again.)
    * DN: In later internal discussion, it was pointed out you could solve this by putting `position `in its own name resolution space.
* KG: In M3+, was a concern about it last time. If we wanted to do this, we could, but punt for now, yes?
* DN: Yes.
* KG: Resolved, M3+.


### [[high-level] Should predeclared enumerant values (e.g. position) should be shadowable by user variables? (e.g. let position =) #4539](https://github.com/gpuweb/gpuweb/issues/4539)



* AB: Added CTS that checks no name resolution occurs ([#3600](https://github.com/gpuweb/cts/pull/3600))
* AB: Last time, agreed they shouldn't have name resolution on builtin value names. Have CTS for that. Think we can close this as fixed. MOD had pr for normative change, doing editorial updates.
* KG: Pr was 4559.


### [Generalize attribute grammar #3911](https://github.com/gpuweb/gpuweb/issues/3911)



* Oguz: (in note, not call) I think now we’re covering this aspect with the following PR [https://github.com/gpuweb/gpuweb/pull/4554](https://github.com/gpuweb/gpuweb/pull/4554), thank you
* DS: Editorial, so editors can edit?
* AB: Yes, working through this now.
* MOD: Close to landing based on latest comments.


### [Attribute syntax should not specialize to built-in language attributes · Issue #3520 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/3520) 



* Oguz: (in note, not call) The PR for preceding, #3911, also should address this sufficiently. Thank you
* MOD: Previous PR should cover this as well.


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday April 23 2023, **11am-noon** (America/Los_Angeles)
* Requests
    * [#4568](https://github.com/gpuweb/gpuweb/issues/4568) Compat validation in createShaderModule vs create*Pipeline (punted from API meeting)
    * DS: Generally errors are generated as soon as possible. But currently Dawn is validating compat rules only at pipeline creation time. But these oppose each other.  If all the compat info is available at shader-creation time, then should those be shader-creation errors.
    * MW: Lean checking more things at shader-creation time, but will take a look.