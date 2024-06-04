# WGSL 2024-06-04 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** JB, DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial++-label%3Acopyediting+no%3Amilestone+), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+0%22+), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+1%22+)

**Todos doc:** [WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2024-05-14 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1pYJI6W5iNWU2cRGtOOUDXVu-zj2Wt7m-Hnol-65USvA) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send **dneto** a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Mike Wyrzykowski** 
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * **Alan Baker**
    * **Antonio Maiorano**
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * **James Price**
    * Kai Ninomiya
    * **Natalie Chouinard**
    * Rahul Garg
    * **Ryan Harrison**
    * Stephen White
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
    * Ashley Hale
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
* Myles C. Maxfield


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



* 
* 


---


# ⏳ Timeboxes (until XX:25)


### [Adjusts subgroup built-in name verbs to match atomics operations #4627](https://github.com/gpuweb/gpuweb/pull/4627)



* AB: We don’t have a strong opinion. Two languages did it “sum” and “product” and one language did “add” and “mul”.  HLSL uses InterlockedAdd.
* **RESOLVED: ACCEPTED**


### [Adjusts subgroup built-in function names to match GLSL #4528](https://github.com/gpuweb/gpuweb/pull/4528)



* KG: Given that we accepted #4627, the only remaining change here is the one to “Quad”
* DN: I don’t like this change. “Subgroup” seems like an overly fussy qualification, people are thinking about their Quad, not about the fact that it’s a particular kind of Subgroup operation.
* KG: People are discouraged by long names
* AB: Confusing to have multiple scopes in the name: subgroup and quad. Just have one.
* DN: Renaming X and Y to Horizonal and Vertical is just too much typing - we’re in graphics, we know what our axes are
* **RESOLVED: NOT ACCEPTED**


### [Add optional feature dual_source_blending #4621](https://github.com/gpuweb/gpuweb/pull/4621)



* (KG: Are we happy with the WGSL half of the PR?)
* DN: Was okay with this. Requested to restrict `blend-source` so that’s only usable when you have a location to use with it. Let’s make a rule to make sure people don’t think they’re getting blend-source when they can’t: forbid it when it’s not meaningful.
* AB: That’s restating rules that appear later on in the entry points section, but I don’t mind the rules appearing in the attribute section too. Otherwise I signed off on this after some review
* KG: Teo approved this too.
* DS: So are we going to land?
* JB: Ok to land from our perspective
* DS: Need CTS?
* KG: Nice to have CTS, but don’t block on it. (Expect it won’t change due to CTS).
* **MARK AS WGSL RESOLVED. LAND after API side happy with it.**


### [Clarification regarding double negation (--) operator? #4669](https://github.com/gpuweb/gpuweb/issues/4669)



* MW: Confusing to users who either unintentionally type this. Difference from existing languages is unexpected. Don’t think WGSL is in the business of correcting mistakes in other languages.
* KG: One of the things mentioned on the issue is “ignoring LALR(1) lookahead rules…”. To me it’s at the core of it.  This case is accidental fallout from our higher level choices.  If we were to address this we’d add supplemental rules to identify and forbid this case.
* JB: I added some context in the issue.  [https://github.com/gpuweb/gpuweb/issues/4669#issuecomment-2148076888](https://github.com/gpuweb/gpuweb/issues/4669#issuecomment-2148076888) When we took our grammar and made sure treesitter could handle it, changing it to recursive descent, then we found this apparent ambiguity. We found a case (put in the comment) that can get confusing.
* JB: Classic greedy tokenization will misparse this as if `>=` is a single token.  At the time there was concern with `>>`, which resolved later as template discovery.  The original thing was resolve before template discovery.  This rule really did address the problems we came across. `--` was not talked about.  It’s not right to say this is unprecedented. For a long time C++ needed a space between `> >` at the end of a template. There was rejoicing when that got fixed.  Don’t look to JS for precedent on parsing ambiguity.   Rust is relevant precedent and Rust allows `>>` as two separate tokens depending on context.
* MW: Given this is a JS API we think JS is a highly relevant precedent.  If this is the only issue like this then we think this is not a big issue. If we start to see more than just this one between JS and shading languages, then it would be more of a concern. If this is the only one then it’s not a large issue.
* JB: That’s fair. JS is relevant for the domain. Another way to say what I wanted is … (?)
* KG: It’s not about whether we’re following one or another, it’s a sliding scale of weight. We agreed JS wasn’t the end-all authority. It has more than zero weight.
* MW: If this is the only place this comes up, then we can accep the current behaviour.  There’s benefit aligning with the existing rule.  We feel generally it would be a problem if there were valid things in HLSL or GLSL that were different behaviour in WGSL.
* KG: Recommend warning about this in your implementation.
* **RESOLVED:** **Keep this <code>minus-minus</code> as allowed.</strong>
* AB: Are you looking for more cases like this?  It would be hard to change things like this down the road. I’m unaware of a systematic attempt to find these.
* MW: We ran across this accidentally when gettitng CTS running,.
* JB: converting the grammar for treesitter was a kind of systematic analysis.
* KG: We also have mandatory parentheses rules.  That’s a possible third option.


---


# ⚖️ Discussions


### [Do pipeline-creation errors encompass the whole module or just the GPUProgrammableStage? #4648](https://github.com/gpuweb/gpuweb/issues/4648)



* AB: This was prompted by other discussion: setting overrides unused by the stage.  Realized we should be clear what is compiled as part of the shader stage.  Our implementation and intuition is it’s _only_ what’s needed to compile the stage. Thing outside that are not considered; has knock-on effects (e.g. diagnostic in WGSL for set-but-unused-overrides if they’re stripped away).  Discussion prompted examples in the issue. [https://github.com/gpuweb/gpuweb/issues/4648#issuecomment-2145774057](https://github.com/gpuweb/gpuweb/issues/4648#issuecomment-2145774057) 
    * 

        ```
Consider the following:

override a : i32 = 0;
override b = 1 / a;
override c = a / 0;



c should always result in a shader-creation error because of the integer division rules.
For b there are a few cases:
b is statically used by the GPUProgrammableStage shader:
If b is overridden, no error
If a is overridden to non-zero, no error
If a is 0 and b is not overridden, pipeline-creation error
b is not statically used by the GPUProgrammableStage shader, no error
It is possible the spec requires more clarification in override expressions to clarify the substitution of values happens before any expression evaluation and that it might cause some expressions to not be evaluated.

```


    * Also have to clarify how and when constants-value substitution occurs, and when validation occurs w.r.t. To that.
    * We realize there is some missing CTS as well.
* JB: I partly proposed #... we were told it’s very common to set a whole packet of overrides whether or not the value is used in the stage. So the diagnostic may have marginal use.  I haven’t read through 4648. I don’t feel I understand all the issue. If 4643 is the motivating concern.
* AB: 4648 is an issue on its own . The spec is not clear what is being included.
* JB: Ask for more time to review.
* KG: Gamedevs may want the lax behavior. Think there’s another community of users who want the stricter behaviour.  
* JB: I would turn it on.
* DS: Question for 4648 is how does validation interact with overrides that are not used. If you didn’t use an override but would ordinarily be an error (if it were used), then should it be?
* AB: Careful about shader-creation vs. pipeline-creation error.


### [Guarantee that OOB writes to storage textures will be ignored #4194](https://github.com/gpuweb/gpuweb/pull/4194)



* AB: This fell off our radar. We want to get there. We’d like to change Tint first.
    * Proposed staging order:  Update CTS to exercise this. Then update Tint. Then land the spec change.
* KG: Count as resolved.
* AB: Would like WGSL Resolved, but not land just yet.
* MW: Are out-of-bound texture stores guaranteed to be no-ops. I’d need to check.
* AB: According to the MSL spec, it should.
* MW: Need to double check implementation behavior.
* KG:  Count as investigation phase, pending testing.
* AB: We’ll put a draft PR for CTS .
* KG: I’m not going to mark this as wgsl-resolved just yet, since we want to do a couple double-checking/investigation things still, but we do have consensus for *trying* to move this way.


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday June 18 2023, **11a-noon** (America/Los_Angeles)
* 