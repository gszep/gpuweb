# WGSL 2023-02-07 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** (APAC) Tuesday **4-5pm **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:**  

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * Mike Wyrzykowski 
    * **Myles C. Maxfield**
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
    * dan sinclair
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * **James Price**
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
    * Zhaoming Jiang
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Microsoft
    * Damyan Pepper
    * Greg Roth
    * Michael Dougherty
    * **Rafael Cintron**
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
* Mehmet Oguz Derin
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


### FYIs and Notable Offline Merges



* F2F at Apple Park, Feb 16-17.
    * There will be videoconferencing (WebEx) for people who wish to attend remotely.
    * MM: I’m setting up a group dinner on Feb16. Send me preferences or ideas for restaurants!
        * Have tentative slots at 3 restaurants.  Will send email asking for preferences.
    * KG: Will send out time slots.
    * AB: Consideration of agenda time slots for APAC members? PST  Morning may be difficult.
* Call for topics for F2F!
    * [WIP Agenda](https://docs.google.com/document/d/1bbwoAu7NZ5GAWAE12xUNZi08fRuSRDfYLzHxV6yFDF8/edit)
* [Add diagnostic filtering; range diagnostics apply to control flow constructs and compound statements #3713](https://github.com/gpuweb/gpuweb/pull/3713)
    * **Ready for review.  ** Has all the fixes and tweaks needed. Matches Tint’s algorithm closely enough.
* FYI.  [wgsl: vec2 shouldn't be a keyword; disambiguate parsing of type construction expr vec2&lt;i32>(0,1) #3739](https://github.com/gpuweb/gpuweb/issues/3739) 
    * Already agreed to the the lookahead, *and* have type-defining words be keyword.
    * Status update: 
        * 3784 landed, making i32, etc, and ‘quat’ keywords.
        * Ben and David collaborating to land the lookahead algorithm.
            * Have the grammar update and treesitter parsing working, _for the case where the type-defining words are *not* keywords._
            * TODO:  Roll that to make type-defining words keywords again.
            * TODO:  Write prose describing the disambiguation step.


---


# ⏳ Timeboxes (until XX:25)


### [Realizing CG Charter Element for WGSL #3778](https://github.com/gpuweb/gpuweb/issues/3778)



    * I.e. Add another repo to gpuweb, to hold the Treesitter grammar and parser as an always-updated repo.  Better for downstream consumers.
    * From last time:  KG: was going to think about it.


### [abs(INT_MIN) should be well-defined on integral types](https://github.com/gpuweb/gpuweb/issues/3785) [wgsl](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+milestone%3AV1.0+label%3Awgsl)  #3785



* MM: It is covered. Let’s close.
* 


### [Are padding bytes between matrix columns preserved for storage buffer writes? #3753](https://github.com/gpuweb/gpuweb/issues/3753)



* JP:  We are tentatively going to propose to preserving padding bytes between matrix columns and at end of vec3.  In analogy with arrays and structs.  Tint implementation would *not* have to scalarize all the writes.
* JB: So the padding there is the same as we’re handling for structs. 
* JP : We have to decompose writes into column writes. 
* MM: Is there anything to do?
* JP: Spec only talks about padding bytes in structs and arrays, silent on matrices.    So spec does need an update.
* **_Resolved needs spec._**
* JP: I’ll shout if something goes wrong implementation-wise.


---


# ⚖️ Discussions



* [wgsl: language evolution: managing rollout of core (essentially sugar) language features #3149](https://github.com/gpuweb/gpuweb/issues/3149)
    * PR [#3792](https://github.com/gpuweb/gpuweb/pull/3792) Add wgsl_version_at_least builtin function
    * Discussion points:
        * Issue 0:  Get the *actual* version numbers instead of a builtin to compare “at least”.
        * Issue 1:  If you declare version N, can you ship some but not all features from N+1
        * Issue 2:  Major version number
* KG: Discussion around putting a constant with the actual number, and use it or don’t use it.
* KG: Also,  reassure to try to not have a ton of versions. Exercise discretion.  Aim to not increment a major version (compatibility break); strong desire.
* MM: Two issues: Philosophical how does it evolve. 2. Do we want to expose a version number. Related but not the same.
* KG: Let’s talk about scalar version number.
* MM: No opinion on number or function.  If we choose a scalar then can’t make breaking changes.
* KG: Not sure it’s exactly right.
* MM: Code that does a version check against a constant would be wrong if we added another axis.
* MM: Sounds like 1 vote for single scalar value.
* AB: We proposed a (major, minor); it’s well understood model.  Having a single scalar meas _if _ we have breaking change we’d have to change the form.
* RC: How does version number interact with the API side.
* MM: WebGPU the API does not have a distinct version number.  Just modify the API.  There will be a function in the API to expose the WGSL version number. That will match what WGSL will say.  There’s one field of information exposed in two places. As convenience for the author.
* RC: When would someone not check the API’s report of the shader version number.  Is it try and fail loop.
* AB: We expect use with const_assert.  For someone intentionally setting a bar “at least this specific version”, instead of trying to debug compiler errors.  And offline tooling that makes use of that can take advantage.
* RC: How do you use it.
* KG: Don’t have to declare it at all. Can use with const_assert anywhere.
* MM: One issue: one version number or two.  Second issue: Should be exposed as just constants, or have “at least” comparison.
* KG: Think it’s fine to have one version, slightly better to have two.
* AB: We have preference to use a builtin function.
* KG: Don’t want a situation where you try to reverse engineer with a comparison function.
* MM: counterargument is comparing complicated version numbers is nontrivial.
* BC: Why do you think people would want to single out a specific earlier version, or range of versions.
* KG: Because we fail sometimes. I’ve had to do this kind of thing in the past and it was annoying.
* MM: What do you mean “we fail sometimes”.  
* KG: Sometimes we fail at being exactly backward compatibility.  (Deliberately or by accident).  We do this in WebGL. If you don’t have a way to deal with that, then you have to be perfect.
* MM: Scenario:  V1 has no FOO, v2 has FOO but it’s broken, v3 has FOO that is fixed.
* DN: Want to add friction to doing the “bad” thing which is *equality* to a specific version.
* MM: Synthesizing:  **One function that takes one argument (an integer) that returns a bool.**
* KG: Think it’s a bad idea, but can agree.
* JB: To get on the record.  WGSL is not like JS.  JS is late binding, but WGSL is not late binding. You can’t translate the shader if there are any references to a function that is not defined (for example).  Tactics in JS are not going to work in WGSL.  Our plans for having things evolve smoothly may not be sufficient, or needs more mechanism than in JS.  May have to have structural/conditional.
* AB: JS is our preprocessor. We’re exposing the version in the API.  We’re going to do our best to maintain backward compat. Hopefully we can do a good job in forward compatibility. If you need to generate a different shader, you should not encode it in the same string that’s passed to createShaderModule. Hand it the right version of shader.
* MM: We can tell that to authors, as a start.  
* MM: Jim’s stated dis-analogy is worthy to think about.   Will think about it.  True for std library; not true for language features.  JS ‘classes’ for example, is a hard cliff; will cause entire source fail.
* JB: Think using JS to glue source code / munge it is fine. 
* AB: Also want to surface another point from Myle’s feedback on the PR.  One item: all the features that come with a core WGSL version should be exposed in a browser at the same time.  Would be hard to write CTS otherwise.  To allay concerns about delivering features in different bundles, can be addressed with other procedural aspects.
* MM: The 4th piece of feedback that we gave.  Example hypothetical.  Say all browsers have WGSL v3.  WGSL v4 is specified to have 17 new features in it.  WebKit would like to be able to ship some but not all of those 17 features, even if we don’t bump the version number to WGSL v4.  If we implement that feature in v4, don’t want to wait to ship it.  Don’t want to wait to implement unrelated things.
* AB: We don’t know how to write CTS for that.  You write CTS negative test, then shipping some new features will cause that CTS to fail.  If you want to handle that scenario, use an enable for the purpose.  An explicit opt-in “i’m looking for more than what’s necessarily advertised”.  E.g. enable future_stuff_v4;
* MM: Interesting idea.  Thinking off top is to flip the default; explicit opt-in to *not* have the new stuff.  Second idea. Is same basics, but make it an opt-in to not get the additional future features.
* Bc: the point about having a monotonic version is look at the spec and know what the browser will be able to do.  Pulling in future features means that mapping (and number) doesn’t mean much anymore.   Like Alan says, CTS is nearly impossible to write; must handle negative tests.
* JB: Thought the strategy was “we’ll implement stuff”. Premise is that we won’t break things for them.  The reason we have shadowing of builtins is to allow us to grow it.
* AB: Motivating example, we agreed to future feature of better pointers for params.  And CTS has tests that make sure certain advanced uses will fail.  How do you write those CTS tests?
* JB: Myles’ example is introducing new builtins.  Are we going to test future things don’t exist?
* AB: The riskier thing is when you expand an existing feature.
* JB: That’s different from what I recall.  Thought we’d allow implementing new stuff.  Should not have negative tests in CTS.
* KG: Using “can_i_use” is fine. It’s totally workable and fine.
* BC: Following up Jim’s comment. The CTS argument is not that you can’t add new things. CTS can check revisions. 
* AB: the version_at_least is to guarantee something will work because shader only uses features up to that version.
* MM: Hesitant to make forks of CTS for every version number. Procedurally.  Also not really concerned about the CTS.  Care about shipping software to users.  The fact that CTS can spurious failures that happen is not a big deal.  If your shipping criteria is 100% then you’re free to hold back partial versions.
* JB: CTS should not have tests that check for absence of features. It’s not a promise that we make.  That’s not a useful property.  It’s ok to say “please don’t let me use after this point”, and then honor that.
    * Myles agrees
    * Kelsey disagrees
* BC: If your browser says “implement 1.0”, and we have CTS tests that passing uniform and storage buffers is an error.
* KG: This is what I am worried about. I see your line of logic, and you end up making this choice. I worry this is large divergence from WebGL.  WebGL did an incredible job making portale what was not portable before.  I’m concerned about portability picture.  “Works in Chrome”. In WebGL we had much stronger portability.
* AB: I’m surprised by the suggestion there should not be some form of negative tests.  You want the spec says something and we test another.  We’re just going for some random stuff.  Flies in the face of engineering.  If a browser can implement anything at anytime, then version number makes no sense.  Won’t be useful to users.  We thought we’d have a number and you’d have features land together in a bundle. Don’t think the can-i-iuse and try-and-fail model is a good developer experience.  Don’t think we’re going to add negative test for a possible future function.  Say we should have tests that check when the spec says “X doesn’t work” then that X doesn’t work.
* MM: Say WGSL V2 has full pointers.  And we want to write CTS for full pointers.  What would CTS look like. 
* AB: All tests for v1.0 “just work”. V2 tests queries the API saying “i an run v2”, and then try the new functionality.
* MM: What about those negative tests.
* AB: You can conditionalize the tests.
* JB: You can write all the tests you want with the “don’t let me use more than v1”.  When you give it a shader which doesn’t make that declaration, then it should be able to work.
* JB: It’s a little exaggerated to say we’re not doing engineering.  
* AB: When you’re saying the spec says “you can’t do X”, then you should have a test saying “X doesn’t work”.  Don’t want to leave different browsers in the lurch.  That’s where portability problems creep in.   Spec is the agreement of what a particular versio is.
* JB: When you test for the absence of a feature, those tests are the ones that should be self-limiting.
* AB: When you add the new feature, or the break, you have to update the CTS existing tests and new tests.
* MM: Key distinction is whether a feature goes through the full process to define it.  Browsers agree with how it behaves, but not on timeline.
* 


---


# 📆 Next Meeting Agenda Requests



* Next week: F2F