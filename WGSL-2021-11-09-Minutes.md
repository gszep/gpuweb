# WGSL 2021-11-09 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribes:** DN, JG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon Americas/Los Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Myles C. Maxfield**
    * **Robin Morisset**
* Google
    * Alan Baker
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
* Intel
    * Jiajia Qin
    * Jiawei Shao
    * Shaobo Yan
    * Yang Gu
    * Zhaoming Jiang
    * Yunchao He
    * Narifumi Iwamoto
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * Rafael Cintron
    * Tex Riddell
* Mozilla
    * Dzmitry Malyshau
    * **Jeff Gilbert**
    * **Jim Blandy**
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Dominic Cerisano
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


### Memory Model Consistency Testing Presentation

Researchers at UC Santa Cruz would like to present to the group on . Can we reserve the date? Expect to use the entire meeting time for the presentation and Q&A.

JG: Concern about amount of meeting time. Keep progress momentum.

DN: We’ll need to discuss at some point anyway, so now or later.


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)


---


# ⏳ Timeboxes


### [Allow f suffix for float literals #2210](https://github.com/gpuweb/gpuweb/issues/2210)



* JG: Understand this goes along with literal unification.  But now, ask, do we want this. Independently.
* RM: Think it’s useful. Especially as we expect to add half and double precision.
* MM: No reason not to do it.
* BC: If we infer the type from a floating point literal, and f maps to float32, then what do we expect a non-suffixed float to map to.
* RM: Today, it depends what we pick for literal unification. In general, no change in outcome while we have only single width floats.
* BC: If the implicit conversion is to float 32, what does the ‘f’ give you.
* MM: It’s there for symmetry.
* GR: Becomes important when using literals as arguments to overloaded builtins.
* BC: Right now, there is no use apart from symmetry.
* JG: Feels same as ‘u’ suffix.
* BC: ‘u’ suffix changes the implicitly determined type.
* MM: Valid. But authors do write the ‘f’, carrying over experience from C++.
* BC: Does have a function if you are able to write 1f
* MM: Benefit is small, but the cost is even smaller.
* **_Consensus:  Allow f as suffix, and 1f to also be a float. (don’t require the exponent or decimal point.)_**


### [wgsl: Require [[location]] for [[interpolate]] #2251](https://github.com/gpuweb/gpuweb/pull/2251)



* JP: Spec allows it but we don’t need it.
* **_Consensus to merge._**


### [Are double brackets necessary for attributes? #2243](https://github.com/gpuweb/gpuweb/issues/2243) 



* MOD: Did feasibility study on parser. And were no issues. Some languages successful without them.
* MM: Agree with the goal. Concern with using single brackets specifically as the solution. Correct no ambiguities today but could be tomorrow.  Already use single brackets for array access.
* DN: Don’t want to spend the innovation tokens here. 
* JG: Can follow Rust. (which has something)
* JB: C# has single square brackets.
* MM: Scenario of possible future where we might drop semi-colons.  So can get an ambiguities when features collide, particularly around lexing.
* BC: Haven’t seen the Rust thing before.  
* BC: Hitting the same key twice is low burden.
* JB: JS implementers hate automatic semicolon insertion (JS devs ignore it).
* JG: Seems no great energy.
* JB: We should state that we feel like it shouldn’t be something being written often enough to truly be a hassle.
* **Consensus: No change right now.**


### [should WGSL handle unicode bidirectional overrides specially? · Issue #2245](https://github.com/gpuweb/gpuweb/issues/2245)



* BC: Big splash a few weeks ago.  Unicode lets you change direction of text within a single line.  It’s visually indistinguishable from having written it a different way.  Hear Rust has ruled out the offending patterns.  Could recommend erroring out.
* MM: This has nothing to do with control characters and overrides.  A native right-to-left speaker can write comments that will do this.  Burden on the editor understand what a comment is and display it reasonably.  Two paths forward, aside.
    * 1. Your editor should make comments displayed intelligently with knowledge of the unicode bidi. 
    * 2.?
    * Neither requires a language change
* RM: Useful for an implementation to issue warning. Rust compiler found no false positives across open source codebases.  So tooling can help. Since warnings are implementation dependent, there’s no reason to change.
* BC: Github now has tooling for calling this out visually.  So maybe we don’t need to do anything within the language spec.
* MM: Reasonable to have a warning when one of these characters appears.  Can appear in good-faith written text. But the case study that found no nefarious uses.
* JG: So recommend **warnings but no language change.**


### [WGSL should specify the execution order · Issue #2261](https://github.com/gpuweb/gpuweb/issues/2261)



* RM:  C languages allow permuting, but don’t know of study showing perf wins.
* Web has greater portability goals.
* BC: Discussed internally.  Think it’s worth pursuing. Massaging code to ensure ordering in various backends can be tricky.  E.g. decompose into a sequence of a let-declarations, then the actual statement. Fine until you get something like a for-loop.
    * E.g. condition has a few function calls nested.  But backend doesn’t allow ordering of arguments.   So you explode the evaluation to a bunch of lets.  But where do you put those lets.
* RM: ‘for’ is just sugar for ‘loop’.
* BC: Yes.  Tint has that transform for the hard cases. Want to hear from Mozilla.
* JB: One goal is to have the output shader be legible.  This would go against.  Understand MSL has weak guarantees like C++.
* JB: We can do this. Our complexity should not hold it back.
* MM: Competing desires: legibility of an output shader vs. portability for users.  Portability clearly win.
* DN: Had thought briefly about how this plays with floating point reassociation.  But it’s independent because you can still force evaluation of primary inputs first, then evaluate the combinations.
* JB: Naga treats all function calls as statements, so we may already be forcing the order in these cases.
* **Consensus: Robin to write PR.**


### [Go from attribute_list * to attribute_list ? in the grammar #2263](https://github.com/gpuweb/gpuweb/pull/2263)



* **(tabled due to time)**


### [Restrict the lhs of assignments to fix a grammar conflict #2265](https://github.com/gpuweb/gpuweb/pull/2265)



* RM: When you see f(, you don’t know in the grammar if it’s a function call or function call followed by equals.  You have to look ahead quite far. Propose grammar change accepts the same language, but removes one of the 4 conflicts in the grammar.
* DN: Haven’t reviewed yet.  In principle it’s ok.
* JG: Would like to allow future growth. But address it then.
* **_Agree to merge (after mechanical review)_**


### [Remove obsolete attribute_list in variable_ident_decl #2266](https://github.com/gpuweb/gpuweb/pull/2266)



* RM: Remove a thing that was added for a thing we took out.  Causes a conflict.  
* DN: Agree.
* **_Consensus to land._**


### [Add complex assignments (e.g. +=) #2241](https://github.com/gpuweb/gpuweb/pull/2241)



* (RM: Cancelled: it is not trivial sugar after all, so I am delaying until after V1.)
* DN: James wants to revive.
* **JP: Will take it over.**


### [Update operator precedence rules #2267](https://github.com/gpuweb/gpuweb/pull/2267) 



* (RM: The content of the changes was already decided by the group one year ago, this is just updating the spec to catch up to our past decision)
* **(Was reviewed by KN and merged)**


### [Should WGSL expose the concept of coherent buffers? · Issue #1621](https://github.com/gpuweb/gpuweb/issues/1621)



* Related to #2055?
* **(tabled due to time)**


---


# ⚖️ Discussions


### [[proposal] Changes to overridable constants · Issue #2238](https://github.com/gpuweb/gpuweb/issues/2238)



* MM: Previously we punted constexpr post-v1, but can we move that back to v1-target?
* DN: We’d like that too.
* DN: Do we need to litigate which of the standard lib builtins can be constexpr? (some, yes, probably)
* JB: **Can we defer until DM is back next week? (yes)**
* DN: There’s also some bikeshedding on attribute vs keyword for locations?
* MM: No opinion on that, but you wanted `const` rather than `constexpr` right?
* DN: Yes
* MM: There’s a choice whether constexpr-ness can be inferred by the compiler, or must be explicit. There seems to be general (but not universal) agreement to require explicit tagging.


### [Consider replacing elseif with else if #2212](https://github.com/gpuweb/gpuweb/issues/2212)



* (David: request to approve this and close “consider not requiring braces in some situations [#2211](https://github.com/gpuweb/gpuweb/issues/2211))
* MM: If Apple had to choose between optional-curly-braces or else-space-if, we choose else-space-if.
* DN: Fine by me.
* RM: If we keep the curly-braces, we have some paths forward, like omitting parens around conditionals, and also the potential for if-expressions.
* JB: For Rust, there is an ambiguity when a construction expression like `Type{...}` appears in an `if` condition: is `if S {` a simple condition `S`, or the beginning of a construction expression in the condition?
* JG: If I choose between optional-curly-braces or else-space-if, I would choose  optional-curly-braces, but I don’t think the consensus is with me.
* **Resolved: Change to else-space-if**
* PR for review: [https://github.com/gpuweb/gpuweb/pull/2239](https://github.com/gpuweb/gpuweb/pull/2239)
* 


### [Literal unification, bottom-up #2227](https://github.com/gpuweb/gpuweb/pull/2227)



* Previously
    * Resolved: Wait on counter-proposal.
* **(RM: counter-proposal is in the middle of being written, requests an extra week to finalize it)**


---


# 📆 Next Meeting Agenda



* Next meeting: 2021-11-16 11am Los Angeles time
* [https://github.com/gpuweb/gpuweb/pull/2282](https://github.com/gpuweb/gpuweb/pull/2282)  Proposal: No other characters count as blankspace. (removes an “issue” inserted when rewriting the tokens section)
* ~~Describe type aliases: [https://github.com/gpuweb/gpuweb/pull/2276](https://github.com/gpuweb/gpuweb/pull/2276)~~