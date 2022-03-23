# WGSL 2022-03-22 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️ Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-03-15 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1kaqGfJRgIfbEr2tCiptvV-OBtThGzmcNPZeO_o2n6I0) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Myles C. Maxfield
    * **Robin Morisset**
* Google
    * **Alan Baker**
    * **Antonio Maiorano**
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
    * Greg Roth
    * Michael Dougherty
    * **Rafael Cintron**
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
* Europe changes this weekend!
* Australia and New Zealand change in two weeks, but the other direction!
* **We use Americas/Los_Angeles, and the Google Calendar invite should show correctly for you.**
* Just in case: [https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting&iso=20220322T11&p1=137](https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting&iso=20220322T11&p1=137)


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


### [Should clamp be only defined for low &lt;= high? #2557](https://github.com/gpuweb/gpuweb/issues/2557)



* Previously
    * Int-clamp to be consistent (min(max()), talk about floats [this] week.
* RM: What is the expense of the polyfill on those GPUs.
* KG: So looking for data or appeal to authority. HLSL?
* DN: The current Microsoft HLSL compilers do. (FXC and DXC targeting D3D).
* KG: Propose tying our ship to Microsoft: they can tweak and we can follow.
* DN: Google’s position was to standardize as either median-of-3 or min/max. AMD’s implementation as median-of-three is the appeal to authority.
* KG: Can let the market decide, and narrow it down in the future. If we think there is a material benefit to portability.
* RM: Generally we aim for portability when perf data is not there. Can see the argument for the two-options..
* KG: Propose concretely on one-of-these-two-behaviours. And keep it in timebox.
* KG:  Making this choice lets us narrow it down later (even in time for “v1”)
* **Needs spec: either of the two behaviours allowed.**
* _DN: I’ll write it up that way._
* 


---


# ⚖️ Discussions


### [Resolve numeric literal types through inference. #2651](https://github.com/gpuweb/gpuweb/pull/2651)



* JB: Got feedback from David and Alan. Not time to reply yet.
* JB: Think the parenthesis topic can be addressed.
* JB: Top-down vs. bottom-up. If I understand the concern, there are nice algorithms for treating these as unresolved, then use unification to produce the types for the entire tree in one pass.  If it’s algorithmic, then I think we’re fine.  If instead it’s influence of one subtree on another, then yes, it’s a characteristic of this design.  Having worked with languages that work this way, have felt it hasn’t been a problem.  Bottom up is fine for computers, but humans think in terms of coherence.  I work with this in Rust, Haskell, ML.  The problems folks complain about in those languages are much more complex. Don’t think I’m worried about it here.
* JB: Alan also wanted a default to i32. Can be accommodated.  Change the “if there are many, then error” to “if there are many and one of those options is i32, then it’s i32, otherwise error”.
* JB: Want to understand what we’re aiming for.  Is it stability, etc.
* AB: Think we’re agreement on goals of representability.  Disagreement comes from editorial.  Happy to keep working on overall language. Think David’s PR is done. Suggest we go ahead with his PR.  My feedback from Jim’s is it’s a little vague from “... and then you get the right type”. In David’s PR it’s clear how every step works. Don’t think we disagree on intent. Agree on substance.
* JB: Intent with the PR is to offer an alternative design.  Should not hold back progress in the standard. Will try to catch up with things this coming week. Think makes sense to move forward with David’s PR makes sense.
* **JB to address reviews**
* **JB to split out improved language passages**


### [Literal unification, bottom-up #2227](https://github.com/gpuweb/gpuweb/pull/2227)



* 
* JB: **Want one week to review**. Major changes done since I last read it in detail.
* KG: **Let’s plan to revisit next week; bias to land. Give folks chance to give feedback**
* AB: And I have a PR built on david’s to do constexpr. Discuss next week? (yes)
* 


### [Should WGSL support Unicode identifiers? #1160](https://github.com/gpuweb/gpuweb/issues/1160)



* MOD: Last week there was a request to show table of options. I [posted it](https://github.com/gpuweb/gpuweb/issues/1160#issuecomment-1068359215). There are 5. I support option 3. “Nonnormalized immutable identifiers”.
* JB: I found blog posts warning folks to be aware of normalization problems with CSS selectors.  Different tools produced different forms. So it does come up.  Would be nice if we could reach a decision: is normalization useful. Set aside codepoint stability, implementation code size.  Is normalization a boon to developers or a distraction.  The majority of projects that intend to be broad and open, are in English.  E.g. many projects require the documentation to be written in English.  The audience of people we’re aiming at are relatively small: a deliberate community of coders that work together.
* JB: Given we’re designing for a small subset of developers, do we even see normalization as something that is helpful to them or not. My sense is it’s a valuable thing to do. But many members of the committee seem to think it’s not valuable to that subset of the community.
* DN: Think normalization is useful, at the same level as a group deciding something like “our code and documentation is written in language XYZ”.  It goes along with code formatting, goes along with other coding guidelines.  Think we can write a note in the spec that there is a hazard here, and you have to watch out for it.  Get the help of tooling. Feel it’s outside the scope of the language. It’s at a higher level.
* MOD: Agree
* KG: Don’t like code looking the same but being different. Bad to have to process the text somehow to get that normalization. Think the “ideal” WGSL program is normalized.  What we’re debating is can we have a WGSL program that is not normalized. Warning can live with it (?)
* MOD: NFC solves it for one corner of unicode and not the rest. Warning case accommodates one kind of user, and not the rest of unicode.
* KG: Am trying to not think too hard about normalization. Think other people have already tried to solve this and have come up with solutions that are demonstrably ok on the web.  Don’t want to solve this differently.  At least seems we’re making the same choices that others have made. Don’t want to try to decide are the details of normalization good enough for enough people. Given that normalization is a process that tries to make these things better, and that it’s open ended and will change in the future, I sort of want to punt all our choices and constraints to someone else’s solution.  (normalize utility in JS).  In light of that the strongest argument is, since we’re tied to JS, nobody could fault us too terribly to follow JS, and warn in any supplementary case.
* MOD: JS did decide not to normalize. Historical / backward compatibility.  WebAssembly also decided to not normalize.
* DN: Think we need to think of Wasm as almost equal footing as JS.  Don’t over index too much on JS.
* JB: Wasm thinks of itself as just a linker.
* RM: Can we stick to just ASCII. Do programmers want emoji.
* GR: Think that’s overly dismissive.
* KG: I understand ASCII is good enough for me and can ship products. Understand folks want more. Can think emojis in code is a non-goal.  Neither advantage or disadvantage. Think non-latin chars and accented latin chars. Hard requirement for comments. Think comments should support in any language.
* MR: Think we don’t have normalization concerns in comments.  All the issues come from identifiers.
* JB: Anybody on this committe hope to use non-ASCII?
* MOD: Me: Chinese, Turkish, and Old Turkic. 
* JB: Turkic uses accented forms, so normalization matters?
* MOD: As far as I know equipment always emits composed characters.
* JB: This discussion is hard because many of us aren’t target users for the feature. Trying to empathize and stretch.
* KG: That’s why I’m concerned about a case of trying to debug other people’s code where what you see isn’t easy to discern the same way as the compiler.
* KG: Ideally, all WGSL code I end up seeing would not be ambiguous that way.  At least if we warn, then the compiler will catch it.  It’s because I’m a reader, not a writer.
* JB: If we can trust an implementation to normalize, it’s the same problem to normalize for warnings.
* DN: Distinction between debugging a single program in a single moment in time. The bugs in normalization at that point in time have a low impact. The long term stability concern is raised when embedding it in the language.
* KG: Think the risk is small. Think the instability argument is technically true but not compelling.  Could go with a consensus to not normalize provided we urge to warn.
* JB: Think instability argument is unpersuasive.
* JB: Think the emerging consensus is option 3 with strong urge to warn.
* BC: So emerging consensus: immutable codpoint set (from unicode standard), no normalization anywhere. And unexpected urge to warn when two identifiers are different but normalize to the same thing, then warn.
* JB: And what makes it immutable is it’s not going to be changed in future versions of unicode spec.
* MOD: Option 3 is also most interoperable with CSS and other web languages.
* KG: I’m split between staying close to what JS did, and that JS didn’t have better tools in the past.
* MOD: JS had normalization option, because when they thought of doing it they could refer to the unicode normalization standard.  They had the option.
* KG: They had different pressures because they were working with a language that already existed.  And avoid backward incompatibility.
* JB: Argument of JS as the language is very extrapolatory; we’re trying to guess what a different people would do.  Stronger argument is string.normalize is out there and people use it without deep concern.
* KG: Want more time to think about this. When I go through the table and eliminate columns that I think shouldn’t be decision criteria, then I end up with no rows left. Have to think about this.
* MOD: Comments already support unicode.
* JB: Think we definitely should have a table that includes axes that Kelsey thinks are important.
* KG: Think we have a bunch of weak arguments
* MOD: Can we land row 3, then argue more later. Get some unicode landed.
* KG: What are the only forward compatible rows?  ASCII and … ?
* KG: And if I’m sanguine about NFC not being a big problem, then I should live by it.
* BC: Think we should land a unicode thing. Worst of all is leaving only ASCII.
* KG: My favourite is option 1.  Think XID_start, XID_continue is targeted to what we’re trying to use them for. Can we later expand to immutable.? Can we go from 1 to 3?
* MOD: Greg didn’t like.  For …reasons.
* KG: Don’t think they were strong.
* BC: Greg argued Japanese culture uses emoji quite strongly.
* JB: I have insight. Am totally unconvinced.
* **BC: So XID, no normalization, strong urge to warn when normalization would lead to identifier collisions.**
* MOD: I’ll adjust my PR to account for this.


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, March 29, 11am-noon (America/Los_Angeles)
* Timebox:
    * [https://github.com/gpuweb/gpuweb/issues/2678](https://github.com/gpuweb/gpuweb/issues/2678) Colon at end of case value list should be optional.
* Discussion
    * [Literal unification, bottom-up #2227](https://github.com/gpuweb/gpuweb/pull/2227)
    * Const expr
        * Draft proposal: [https://github.com/gpuweb/gpuweb/pull/2668](https://github.com/gpuweb/gpuweb/pull/2668)