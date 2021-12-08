# WGSL 2021-12-07 (APAC) Minutes

**🪑 Chair:** Kelsey (was Jeff) Gilbert

**⌨️ Scribes:** DN

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
    * Ryan Harrison
    * Sarah Mashayekhi
* Intel
    * Jiajia Qin
    * **Jiawei Shao**
    * **Shaobo Yan**
    * **Yang Gu**
    * **Zhaoming Jiang**
    * Yunchao He
    * Narifumi Iwamoto
    * **Hao Li**
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * Dzmitry Malyshau
    * **Kelsey Gilbert**
    * **Jim Blandy**
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
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
        * [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)


---


# ⏳ Timeboxes


### [wgsl: Add vector and matrix constructor overloads that infer component type #2305](https://github.com/gpuweb/gpuweb/pull/2305)



* Previously
    * JG: I’d appreciate the time to think about this for another week
    * RESOLVED: defer until next week
* MM: Related to literals.?
* BC: Whatever builtin overload resolution works, this would match it.
* MM: They’re just functions.
* **_Approved_**


### [Add git version info to wgsl spec #2362](https://github.com/gpuweb/gpuweb/pull/2362)



* DN: Injects commit info with a sed script.
* MM: This is how we can get specific info.
* MM: Sed exists for windows. Shouldn’t be a problem.
* **_Approved to land_**
* MOD: If there’s an issue with the CI github generation, I’ll fix it.


### [Allow arrays with pipeline-overridable element count, as store type for workgroup variable #2347](https://github.com/gpuweb/gpuweb/pull/2347)



* Implements the consensus from #2024
* DN: Builds on the Block removal. Refines the “fixed footprint variable” concept.
* **Approved to land**
* KG: Has conflicts. Land after resolving.
* [https://github.com/gpuweb/gpuweb/issues/2188](https://github.com/gpuweb/gpuweb/issues/2188) is the topic about allowing storage buffers to not be required to hold a struct


### ~~[Remove Next from switch condition when body doesn't have Next #2382](https://github.com/gpuweb/gpuweb/pull/2382)~~



* ~~(RM: LGTM)~~
* Landed


---


# ⚖️ Discussions


### [​​Should unreachable-code be an error or warning? #2378](https://github.com/gpuweb/gpuweb/issues/2378)



* BC: Complaints from developers wanting to do an early return.  Implementing in Tint, and found some cases that used to be required one way are now an error. So implemented as a warning.  Modified behavior analysis to say “unreachable statements don’t contribute to the overall behavior”.  To make the change for a warning would require some thought to spec it.
* MM: I think we’d be ok with warnings. I would like this to be a “normative warning” (which is new) where we’d *require* this to generate a warning.
* DN: I think we can implement this in CTS, and at least show that this does generate a warning.
* AB: I don’t like it being a required warning. If it should always be raised, it should be an error, otherwise just a warning. If an implementation wants, it can, though.
* KG: Common workflow; there are languages that complain about unreachable code. Done naively. Can do ‘if (1) return;’ as an escape hatch.
* BC: Yes, in current rules in the spec you can use if (true){return;}
* KG: Ton of prior art for this kind of thing.  Often says “you have code after a return”.
* BC: Another reason folks have asked for this as a warning: code generation. Behavior analysis has to be replicated in the code generator. 
* MM: Generator may have to know about behavior analysis anyway.  May already have to know about it due to uniformity.
* BC: Case in point is spirv reader in Tint produces WGSL.  Was nontrivial enough to that I wrote a transform to strip from the AST, instead of modifying the construction in thef r
* KG: Phrasing:  “statements after control flow” is what’s made problematic.
* AB: If we add constexpr at some point, then could expect to see through “if (true)” to be analyzed.
* RM: Are you suggesting a new different analysis for this warning?
* KG: It’s purely a lexical analysis.  /look for stuff after continue, break, return. A nonlocal exit.
* RM: I’d be fine with warning rather than error. Developers can choose to pay attention to warnings, and then elect to treat that as an error. There is no security or portability issue that we are guarding against.  Folks writing code generators can use the “if true return” trick.
* AB: I want there to be a future with constexpr that we’ll evaluate and pay attention to the condition.  I view ‘if (true)’ trick as short term.
* MM: As long as the dead code elimination is run after the constexpr analysis.
* AB: Run the behaviour analysis after the constexpr.
* KG: I want to see protection against the tripping hazard.
* MM: That matches my thoughts.  We should guarantee that compilers will do something in response to the kind of bug that caused the goto fail problem. Don’t have opinion error vs. warning. Should guarantee the programmer will be notified somehow.
* KG: I like status quo.    Error required, and you can work around it with if(true) trick.
* GR: Precedent?
* KG: “most languages”.
* **Resolved: Continue to have an error.**


### [Should WGSL have different types of extensions? · Issue #2310](https://github.com/gpuweb/gpuweb/issues/2310)



* (process, policy for extensions / enables)
* DN: W3C has opinions on certain things. I think we should probably officially have no extensions within W3C, and we would have no official extension registry, except for the features that would make it into the spec.
* MM: I think it’s not quite accurate to say that W3C is not concerned with features still-in-flight. (e.g. WebGPU is still in-flight) But for example if someone wanted to write a raytracing feature, we could prototype it, even if it’s not ready or has limited support preliminarily. However the W3C isn’t interested in speccing things which have no aspirations to be supported in many places/backends.
* AB: How do we draw the line of reach. (e.g. 70% of some denominator). How important is broad reach. Defining new things vs. stuff that’s not everywhere yet.
* MM: Intentionally didn’t use a metric of number of devices. There are lots of browsers with much smaller numbers of users. Those browsers count in considering W3C spec. Don’t use market share.
* AB: I think we should, to an extent. Let’s say NVIDIA (they like to run ahead) manages to get a feature on all (denominator?). It meets the bar of wide, but should not be considered broad adoption.
* MM: Proposed 3 criteria
    * Multiple browsers
    * Multiple APIs
    * Multiple device vendors. 
* MM: So an NVIDIA-only extension would not be enough. But if they got together with AMD to make something common, then that would be enough.
* AB: Would like mobile included.  Mobile vs. Desktop should be considered.
* KG: That’s something we’ve wrestled with a lot in Web(GL?GPU). Strong split between shader texel buffer fetch. Am much more comfortable with Myles’ set of criteria.  Would like to set a aspire to bridge the portability gap between desktop and mobile, but don’t think we can make that strict criteria.
* MM: Apple has same arch on mobile and desktop
* KG: another way to slice / proxy is discrete, integrated
* AB: Come from context where very wide portability is very important.  When thinking of adding a feature, have to think hard about when something is blessed by W3C.  In GLSL’ism there are EXTs that typically are groups of vendors, but not really “everywhere”.
* MM: Feel if something is optional it’s likely to be optional forever. Can see extension packs. But still optional.
* AB: Think there’s value in different levels of blessing.  “This thing is something you should think about taking advantage of, expected to be in majority of hardware &lt;blah criteria>”  Being able to talk about those buckets is valuable.
* KG: Let’s focus on what this group needs to do.  E.g. develop dev-only features.  Kind of exists outside this spec. But should be aware of what we’re going to be asked to be guaranteed here. E.g. for WebGL, we have checks to make sure that you don’t offer more than you’re allowed to offer. E.g. extensions which were not official extensions (in our registry), that could be a problem.  Have a nebulous bar about what’s included in the registry. Have mechanism for “draft extension” which are not exposed, and community-ratified extension.  We do track extensions that are draft,
* GR: Is there an experimental/developer mode to allow experimentation.
* KG: All browsers do.  Chrome has command line flag.  Firefox has a preference.  about://flags
* RC: Chromium has command flags and about://flags and more of the former than latter.
* MM: Safari has a menu: a big list of checkboxes.
* GR: Somem pretty security concerns about enabling an extension willy nilly.
* RC: All these mechanisms, the end user has to take action to enable them.
* GR: All those vendor extensions should be enable-able behind a flag.
* MM: Yes, behind a flag is fine.
* KG: What matters is what we test in our test suite.  Have to test that you don’t get any more features than you’re allowed. Can do 
* DN: Can we reserve a prefix that we promise W3C won’t bless one that starts with underscore.
* KG: Do we test that. Get the extensions and require none of them to start with an underscore.
* MM: Sounds smelly, but ok. (?)
* MM: Should test with as close to final implementation. So should exclude underscore-pattern extension.
* KG: It’s up to us what negative tests to add. We have list of known extensions. And test only those come back.
* MM: The audience of the policy is us.  We can change if needed.  Can accept the strict policy for now, and relax in future if needed.
* DN: Does the API allow you to enumerate the extensions?
* MM: Yes, there’s a vector of strings and an enum.
* AB: Are all WGSL extensions tied to an API extension? Or can you have WGSL-only extensions.
* MM: Propose that all extensions should go through WebGPU. If you want to enable a WGSL feature, you have to enable it at WebGPU device creation time.
* AB: Think that’s reasonable.
* GR: Think that’s reasonable.
* GR: DirectX officially has no extensions, but NV and others have done so for decades.  Folks will find a way.
* KG: We can’t prevent folks who are very determined.
* MM: Can that happen on the web? (an extra JavaScript API to WebGL)
* KG: You’d have to do something ugly, like populating a signature string in a buffer. (Can’t do it in the shader transpiler.)
* KG: What do we have here.  Some criteria for “broad support”
* MM: Audience is different from shader authors.  Should be on wiki?
* KG: Put it in the spec.
* MM: I can send a PR. Don’t want to touch the desktop/mobile divide.
* AB: Thanks for the discussion.  We’ll work through policy/process when we try to add the first extension.


### Intel topics?

Intel: Today we don’t have special topics, but have learned from the discussion about extensions.


### Shoutout

BC: I implemented the behavior analysis this week.  It was painless.  The analysis was well done. Thanks and kudos to Robin.


---


# 📆 Next Meeting Agenda



* Next week’s meeting: 2021-12-14 11am Los Angeles time (non-APAC)
* 