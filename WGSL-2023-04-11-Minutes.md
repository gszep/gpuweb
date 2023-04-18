# WGSL 2023-04-11 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** JB, DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed/non-APAC**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-04-04 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1-DENn4GoVvVk77pC855n46zqaGzT1PGlDs0PVfC6v5E) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Mike Wyrzykowski **
    * **Myles C. Maxfield**
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
    * Kai Ninomiya
    * James Price
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
* **Dominic Cerisano**
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


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


### “Atlantic-timed” instead of “non-APAC”



* FYI!


### FYIs and Notable Offline Merges



* 


---


# ⏳ Timeboxes (until XX:20)


### [wgsl: unpack2x16snorm exhibits 3ULP error on AMD GPU on macOS Ventura #3991](https://github.com/gpuweb/gpuweb/issues/3991)



* With PR: [https://github.com/gpuweb/gpuweb/pull/3992](https://github.com/gpuweb/gpuweb/pull/3992) 
* (KG: Can we have a weak consensus to just accept the 3ulp?)
* **Consensus to merge**
* 


### [Support for gl_PrimitiveID / SV_PrimitiveID -like construct? #1786](https://github.com/gpuweb/gpuweb/issues/1786)



* MM: At least from our perspective, a natural win. If there’s support and people want it, let’s open it up. But only in the fragment shader. In the vertex shader there’s no such thing as a primitive
* KG: Need a PR to precipitate some investigation. Seems like an easy win, but, it didn’t ship with the V1. What’s the compatibility story?
* DN: This hasn’t been on our radar, so we don’t have an opinion right now (bad audio) need more time
* AB: Let’s defer
* **DEFERRED**


### [No abstract type overloads for zero value builtins · Issue #4019](https://github.com/gpuweb/gpuweb/issues/4019)



* JB: We don’t say what vec2() is.  Seems it should be the same as vec2(0,0), etc.  Not clear what it should be for matrices, since zero matrix is not particularly useful.
* BC: It doesn’t exist.  Semi-intentionally. HIstory is we started type constructors with explicit type. We added aliases, and sugar forms. Added the infer-the-type form from the arguments.  In this case, there are no expressions to infer the type from. It’s fair as a _new_ feature request.
* JB: We have abstract-typed vectors.
* AB: You have to spell the zero.  You have to name something.
* KG: The feature is vec2() is the same as vec2(0,0).
* BC: Want to make clear it’s not a mistake; the ‘inferred type’ constructors are short-hand when you have the argument expressions.
* JB: I want the abstractness to propagate further.
* MM: Want to touch on matrices.
* KG: Zero matrices are not useful on their one, but it’s useful to be the starting point to make a useful matrix from it.
* AB: It would be wrong to make the matrix non-zero, for consistency sake.  Would need a different name for the constructor.
* BC: Have a way to make implicit zeroes. If you want an identity, make an identity.
* MM: Need _some_ way to make identity matrix.
* KG: Suggest add identity builtins. …
* AB: Got stuck before: which identity matrix you want; we don’t do templated functions. So naming the type is fiddly.  Had dropped from v1.
* MM: Shall I open a new issue for matrices? (square matrices)
* KG: Affine translation matrices make sense.
* **_Consensus to add vec2() mean vec2(0,0), and also vec3() vec4()_**
* **_File new issue for matrix spin offs._**


### [Add component-wise conversion constructors? · Issue #4021](https://github.com/gpuweb/gpuweb/issues/4021)



* AB: I was porting some shaders from GLSL and I was converting integer coordinates to floating, and it was annoying. We have whole vector conversions and individual conversions, but no component-wise conversions. This would be a convenience.
* MM: Historically, this group has been opposed to automatic conversions, but we’ve been sliding towards having them more and more. Maybe we should just add a feature for automatic conversions.
* AB: This proposal isn’t going that far, because you’re naming the types - you’re telling it the component type and the size. But argument promotion for user-defined functions is more opaque, because the naming isn’t explicit
* BC: I’ve walked into this multiple times writing shaders. We did agree that the texture sampling overloads would allow different integer types, and it’s a slippery slope to add more and more accommodations. If we permit it on builtin functions, why not user functions? It’s late in the day.
* DN: An argument against allowing implicitconversions all over the place is things like integers that map to the same floating-point value implicitly. Secondly, as you add new types, the outcomes are different, you’re going to get different conversions than you’d expected, it’s too easy to walk into bad behaviors. I think not allowing willy-nilly implicit conversions, the decision we made in the past, is still the right one
* KG: It seems tolerable not to fix this.
* MM: I think DN’s arguments are valid. The counter-argument is, it’s elegant if constructors and functions are not actually different - they just behave the same way. So if a constructor takes an input, the overloading and conversion works the same as if you’d called a user-defined function. The fact that you spelled out the type in angle brackets shouldn’t matter.
* BC: We could have it in the spec that every combination of types is permitted, as we did for the sampling functions. The vector constructors could take a separate type argument for each of the arguments.
* KG: So, how strongly do we want this original proposal? (see example in proposal) My weak vote is to just leave it, it’s not painful enough to fix
* MM: I think the proposal is actually not just `vec2f`, it’s that aggregate type constructors that have inputs, every input is a separate templated type, as we’ve done for the sampling function arguments. I don’t think we really have an opinion. It’s important to make sure things fit into future language features
* BC: We’re expecting an enormous amount of feedback very soon, when we ship in Chrome 113. If people start complaining then, then we could revisit this
* KG: Right - when I propose closing an issue, I’m always thinking that we can come back to it if there’s a lot of feedback
* KG: **Let’s think about this some more, revisit**
* Dan: We only have limited type conversions - would this mean that we’d need more conversions?
* AB: If it’s a scalar conversion, that would be okay. Otherwise it wouldn’t be a valid overload.
* MM: So you could pass an int and an unsigned as different inputs to a vec2f constructor


---


# ⚖️ Discussions


### [Filterable diagnostic names should be checked, but also permit third-party diagnostics · Issue #3989 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/3989)



* JB: I’d like to close this issue in this meeting. We have consensus on the most important things. Have one last question of the microsyntax rule. Feel like we’ve made our arguments clear.  Mine is using the pun of the namespace in the language is a production-tested design that has shipped and has proved helpful, and has not caused remarkable confusion.  Look for opinions from other members.
* BC: I dumped my thoughts in the issue.  Most of my crankiness is that the diagnostic attribute now is very special. It’s an inconsistency; it stands out vividly vs. all our other work to make things identifier-lookups.  That is upsetting to me on a fundamental level. If we go down member accesses as a tactic. When we add namespaces, it raises … I wrote a (contrived) example.  Think it’s minor.  In reflection, would have liked a string notation for the name.
* JB: So if we take the pun now, it would be weird in future.
* BC: My objection is we have things resolve differently in this one weird place. It’s inconsistent. It will look more weird by having something that looks like an expression that resolves differently
* KG: From an author’s standpoint it doesn’t seem too mystifying.  Understand from an implementors/ language lawyers standpoint.  If you asked an author to write it as a string literal, then they might say “why can’t you just know that and put the string marks in for me”.
* BC: May be my unfamiliar with Rust practice.  For someone unfamiliar with WGSL, how would they know it’s not resolved like other identifiers.
* DS: In my Rust experience I see a thing I don’t understand (‘diagnostic’) and then I go an look it up, and *that* tells me what it is and I’m ok.
* KG: Right, it is an inconsistency but tolerable.
* BC: Would lean against making it look like an expression. Weak preference. Won’t fight it.
* DN: My weak preference is tipped by Rust’s existing practice. We can reference that as prior practice.
* MM: Remind me what the feature is (rather than spelling it).
* go/
* MM: Concretely, e.g. spaces around equal sign.  Would have to be `webkit.spaces_around_equals`.  Because it’s not standardized.
* MM: Don’t want to stand in the way, but the trend has been the opposite. Trend has been to remove browser prefixes.
* JB: CSS is constrained by the practice to make sure …
* MM: Constrained is a little strong. 
* KG: Need a consensus.  Propose Jim’s consensus.
* BC: **Let’s go with Jim’s.**
* _Thanks everyone._


### [Shader extensions are redundant · Issue #3961](https://github.com/gpuweb/gpuweb/issues/3961)



* MM: I wrote a really long post where I tried to go throught the arguments and give context, and suggest a path to compromise - but that was just before the meeting. This probably doesn’t have to be done for V1, since it only makes more programs valid. So we could say, let’s take a week to read the issue and then talk about it next week.
* KG: I’d like a chance to read it offline
* **Tabled for next week**


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday April 18, 2023, **11am-noon **Americas/Los_Angeles
* 

 
