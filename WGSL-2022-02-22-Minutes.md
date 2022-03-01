# WGSL 2022-02-22 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️ Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon Americas/Los Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-02-08,09 - WGSL/WebGPU F2F - Agenda / Minutes](https://docs.google.com/document/d/1r5QbKsqwZxZzD2CqbpteTGJ2bQwTHje_coZ8UCIAyik) 

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


### [Rules about "break" inside "continuing" are strange · Issue #1867](https://github.com/gpuweb/gpuweb/issues/1867) 



* DN: Don’t need parentheses.
* MM: So just `break if` in `continuing`?
* JB: Or at the end of a `loop` with no `continuing`
* MM: But if it’s supposed to just be for `continuing`
* JB: SPIR-V requires a `continuing`, but we let wgsl omit it.
* **Resolve: `break if` allowed as final statement in `continuing`, but not elsewhere.**


### [wgsl: Describe experimental vs. complete extensions #2609](https://github.com/gpuweb/gpuweb/pull/2609) 



* (Closed, see linked comment)
* DN: Hinged on whether an extension would be merged into the main spec while the extension is not yet ready or complete, i.e. it is “in progress”. If extension is included only when it’s “final”, then there is no need to make a distinction in the main spec about its readiness state.
* MM: I think this is contrary to what we agreed. I think we agreed that we’d use external documents for larger exts, but smaller exts can be merged directly.
* MM: If we want to have all in-progress exts live in separate docs.
* AB: to me, “in progress” things don’t live in the main spec.  Merge it all at once in a PR. It goes in when it’s ready.
* KG: Don’t think we need too much process here.  Having an outside-of-spec doc works great for big things.
* **DN: So “Do we ever merge a PR for an extension that’s still in progress?”**
* **RM: I think “no”.**
* DN: Ok, then we probably don’t need any additional grading/labeling in the spec.
* MM: Just be aware that this could cause rebase collisions
* JB: If we had everything in-spec, then editors have to, so someone always has to.
* **RESOLVED**: In-progress extensions do not get merged to the main spec (in the main branch)


### [Update override examples #2614](https://github.com/gpuweb/gpuweb/pull/2614)



* KG: Seems ready to land.  Has failure. Looks like host-side API building.
* _Try building it again._


### [Consider reserving keywords for a module system #2607](https://github.com/gpuweb/gpuweb/issues/2607)



* Resolved: DN to make PR reserving _all the keywords!_

Rust, C++, Smalltalk, JavaScript, TypeScript


### [Literal types #2227](https://github.com/gpuweb/gpuweb/pull/2227) 



* (Mozilla wants more time to investigate an alternative approach)
* JB: Will try starting from Rust, and see if it can be spec’d well and more simply.
* DN: I shared collected samples that should be made to work.


### [Binding model in WGSL does not respect encapsulation #2426](https://github.com/gpuweb/gpuweb/issues/2426) 



* (v1?)
* MM: Situation I’m worried about is if it’s optional, but then developers don’t use it because it’s not incore.
* MM: Think the cost is low.  Important to encourage authors to put as much inside a single module as possible.
* DN: The optionality is an argument that this isn’t necessary for v1.
* MM: Minimal proposal is braces at top-level (unnamed namespaces). Introduces scoping.
* DN: What about things nameable by the API.
* MM: Kai’s answer: rule is that anything nameable by the API (entry point name, and overrides), are not allowed to collide.  So they’re like their at a top-level namespace. 
* AB: Does this solve people’s problem.  Are entry point collisions a problem.  This only solves the problem for helper functions, resource bindings, and module-scope var and let.  What is the extent that is being envisioned here that is being combined in one module. The only name collisions you’re saving here are independent bindings.
* MM: Named namespaces was the original idea.  Think they would be better.  Current idea is the direction from the issue discussion.  Current idea can be extended to named namespaces. 
* AB: Why is it more complicated to prefix just the entry point, and not everything.
* KG: Your concatenator is much simpler if it only has to rename the entry point and overrides.
* JB: Find Alan’s points persuasive. It’s only a partial solution. Think a real solution is the named namespaces. Think we should spec and land named namespaces first.
* KG: Let’s work on unnamed spaces.
* **MM to write PR**


---


# ⚖️ Discussions


### [Should unreachable-code be an error or warning? #2378](https://github.com/gpuweb/gpuweb/issues/2378)



* RM: Current state.  I was supposed to write a PR to show possible spec text.  Let’s delay.


### [Should WGSL support Unicode identifiers? #1160](https://github.com/gpuweb/gpuweb/issues/1160)



* [https://github.com/gpuweb/gpuweb/pull/2595](https://github.com/gpuweb/gpuweb/pull/2595)
* MOD: The question is whether to normalize or not.
* KG: Should included Gregman (Greg Tavares)
* BC: Most of Google’s stance is we should not normalize.  Discussion about normalizing at the API level, and leaving normalization out of the WGSL spec.  We are reaching out to internal internationalization teams for their feedback.  
* MM: There aren’t only two options.  A third option is no unicode in identifiers.
* GR: FXC does not support unicode identifiers. DXC support unicode is WIP. Depends on the platform OS encoding.
* BC: We agreed before to focus on UTF-8
* DN: Have an intent to register MIME type text/wgsl requiring UTF-8.
* MM: Why add unicode to DXC?
* GR: Dropped support for FXC, and DXC codebase supports it so we say “why not”.
* BC: I implemented the proposal XID_start XID_continue, excluding normalization.  The XID unicode sets are very easy to implement, along with UTF-8 encoding. Minimal codesize and speed impact. I think what we have now, excluding normalization, is quite feasible.
* KG: Addressing Myles three options, think we have consensus to allow unicode identifiers.  So it’s normalize vs. not.  Just because we are debating normalize vs. not, doesn’t mean we should back out of unicode altogether.
* KG: We’re weighing benefits of normalization vs. the tradeoffs of normalization.
* RM: Even if we want unicode identifiers in the future, we can defer unicode altogether.  Many programming languages survive with ASCII for a long time.
* DN: Stipulate that normalization is good for people Had discussed at the F2F:  the API layer and compiler stack are viewed as combined, from the user/author/developer perspective.  So it doesn’t matter where the normalization occurs, just that it happens in one spot.  So can we say that WGSL assumes the source text is normalized, and that createShaderModule API call says it will normalize the text for the user.
* MM: The argument for that is diagnostics messages will need translation.
* DN: Haven’t standardized how to count lines and codepoints.  In offline discussion, have discussed counting code units (i.e. bytes in the case of UTF-8).  And diagnostics can be retranslated by the API side.
* BC: Think that retranslation is Have to remap message text locations anyway. Because Tint handles utf-8, but the web string is UTF-16 (??)
* JB: Naga is the same.
* …
* KG: Would be sad in 2022 if we release something without unicode identifiers.
* BC: We have our ongoing discussion with our i18n team, but think we can get agreement no normalization at WGSL level.
* MOD: Can agree non-normalization.
* KG: Can’t go back once we decide.  Will we learn anything in the next 3 years that could change our minds.
* JB: Those google conversations are in flight. That could change our mind.
* KG: Let’s wait for that.
* MOD: Anything other than NFC gets muddy
* JB: The K-forms of normalization clearly change the meanings of things.
* MM: Agree. Don’t use the compatibility forms.


---


# 📆 Next Meeting Agenda



* Next week: Tuesday March 01 **APAC 5pm-6 (America/Los_Angeles)**
* ~~[PR #2618](https://github.com/gpuweb/gpuweb/pull/2618) Add optional break-if at end of continuing block. ~~ Landed
* [PR #2581](https://github.com/gpuweb/gpuweb/pull/2581) Add acosh, asinh, atanh builtin functions
    * Discussion: What to specify when math says there is no answer. (These are inverse functions, and some inverses don’t exist.)
    * Dzmitry said he wanted some principles.
    * No doubt there are other lurking cases of undefined results for math functions.
* Issue [#2378,](https://github.com/gpuweb/gpuweb/issues/2378) How to analyze unreachable code.
    * Alan’s proposal: [https://github.com/gpuweb/gpuweb/pull/2622](https://github.com/gpuweb/gpuweb/pull/2622) 
* [#2557](https://github.com/gpuweb/gpuweb/issues/2557) Should clamp(x, low, high) be only defined for low &lt;= high.
* Reserve keywords: [https://github.com/gpuweb/gpuweb/pull/2617](https://github.com/gpuweb/gpuweb/pull/2617) 
    * Reserved all keywords from a bunch of languages.
    * Did not reserve:
        * anything special from MSL (apart from C++20), and 
        * C preprocessor words (eg. ifdef)