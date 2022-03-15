# WGSL 2022-03-15 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️ Scribes:** KG, JB

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-03-08 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1FvXBSA3GSn1pAR1dsIBEZAk1z0bK3Eyi3tFpkzNUZRs)

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
    * David Neto
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



* North America just changed!
* Europe changes in two weeks!
* Australia and New Zealand change in three weeks, but the other direction!
* **We use Americas/Los_Angeles, and the Google Calendar invite should show correctly for you.**
* Just in case: [https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting&iso=20220315T11&p1=137](https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting&iso=20220315T11&p1=137) 


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



* BC: Is there an int-clamp table? (no)
* BC: Did you test mobile devices?
* MM: Apple mobile devices, but not others, not Android.
* BC: Worried about weird and wonderful Android driver implementations.
* ??: If you think it’s risky, don’t emit clamp, emit min(max()).
* KG: Maybe we could standardize clamp, and add fast_clamp later if needed.
* MM: Apple’s philosophy is generally to be portable first, and add speed later if needed. We could even not call it clamp at first, call it min_max().
* BC: I do think things need to be fast, and I think it’s a narrow usecase to worry about using clamp wrong.
* JB: Having a choice of two definitions might be acceptable.
* KG: if we accept both implementations in the spec and Metal always polyfills, is that portable enough?
* MM: we would be disincentized if it were undefined values
* AB: we could say it is either med3 or minmax
* JB: similar to OOB
* MM: think it differs because there is hardware to reduce the cost
* **Int-clamp to be consistent (min(max()), talk about floats next week.**


### [Workgroup Broadcast #2586](https://github.com/gpuweb/gpuweb/issues/2586)



* JP: Previously, we said that the only time a load could actually have non-uniform behavior was in the presence of a data race, and I think we should just accept that yeah, a data race makes things non-uniform (undefined behavior). The sticking point seems to be whether data races produce undefined behavior, versus undefined values.
* MM: It is within the rules for the metal shader compiler to look at your program, prove a data race, and do whatever it wants.
* 


### [FP16 Extension: Part 1 #2647](https://github.com/gpuweb/gpuweb/pull/2647)



* **KG: **intel wants to make a document, then a PR. We suggested that they just make a PR directly. Does anyone think there should not be a separate document at all, and they should just jump directly to a PR?
* AB: Does the document get updated to change the contents of the PR? There’s a place for a document describing an overview of the feature. There’s less utility in a document that just describes the diff that would be the PR.
* KG: I think the former. Then once it’s merged, we can get rid of the document
* MM: Intel is doing a lot of work, and we should let them work in whatever way is most effective for them
* AB: Making them write that twice is what we want to avoid, we should help them not do that.
* KG: It seems like that (write it twice) is what they’ve chosen to do, so we should let them approach it how they want.


### [Why have literal suffixes at all? #2664](https://github.com/gpuweb/gpuweb/issues/2664) 



* BC: I had some reservations, but then thought better of it. Some individuals have expressed a strong desire not to (be able to?) do any inference at all. I’ve tried to argue that suffixes address this problem, but it wasn’t persuasive.
* BC: There are many cases where you’d want to declare a var or let, say in a for loop, and it’s slightly more convenient to write `var i = 0u` instead of having to cast.
* BC: We spoke about this a bit within Google, and we have concerns about where to draw the line. At the moment, we have i, u, float16 support - should we have h? Hf? How does this end?
* JB: only one column - and only with support elsewhere so they’re rare
* MM: Apple strongly feels that it shouldn’t be easier to use f32 than f16. We feel very strongly about that.
* MM: Apple weakly feels that it’s easier to write a suffix at the end than to write out the name of the type. And coming from a C background, having the suffix be longer than the number you’re trying to write is unfortunate. 1u16 looks like 116 with a typo in the midst of it
* BC: re: putting the type at the end. I like that it means there’s no need for a mental map from letters to types. But there’s an asymmetry with type aliases: would you allow type aliases to appear as suffixes? Or is it restricted to the fundamental scalar types of WGSL?
* KG: In Rust, the type names are already the shortest thing you could imagine.
* BC: The strongest held opinions: the people who were worried about being explicitly were worried about code generation: it should be easy to know exactly what the type was you’re emitting, and not getting interference from inference. But, I feel that if you care about that, you should just emit verbose code that spells everything out.
* MM: Yeah, the language should just have clear rules, and then people should know them, if they’re emitting code.


---


# ⚖️ Discussions


### [Should WGSL support Unicode identifiers? #1160](https://github.com/gpuweb/gpuweb/issues/1160)



* [https://github.com/gpuweb/gpuweb/pull/2595](https://github.com/gpuweb/gpuweb/pull/2595)
* Previously:
    * DN: We got the feedback from our Google i18n experts. Came in our afternoon.  We need to review.
* BC: I’ve put our takeaways from our internal meetings on the thread. Recap: some people feel strongly that XID_START and _CONTINUE are too restrictive, so we lean towards having a codepoint set that includes things like emojis. We lean towards not normalizing WGSL in the language or at the API level. But to try to prevent people from making mistakes, we’d like to encourage implementations to warn if libraries are available to help them normalize easily. We’d like browsers to produce warnings about confusables that result from normalizations. But, the specified behavior should be that things resolve in a non-normalized way. So, consistent behavior across the board, but warnings.
* MM: From the perspective of unicode, those ‘clashes’ are not mistakes - it’s a feature. It’s part of how Unicode is designed to be used that these strings are *not* equivalent, and it’s counter to that purpose to warn about ‘clashes’.
* MOD: …
* MM: Yes, it is opt-in, but presumably, since Unicode bothered to specify this behavior, it’s because they think it should be used.
* KG: It feels strange, but the intent is that there are two ways of saying the same thing, and since our intent is to communicate with the compiler. The dream of Unicode is that you type amelie either way, composed or decomposed, and the compiler gets it.
* KG: There simply isn’t a way to guarantee perfect forward compatibility. But we’re trying to get the benefits of Unicode as a general idea, and that’s more important that avoiding minor compatibility issues.
* AB: That’s why we arrived at having a fixed set of rules. The unassigned code points cause us to lean towards avoiding normalization.
* MM: Unassigned code points should not be a problem, because they’re so rare.
* KG:
* AB: New code points may have normalization rules, which is why we want to avoid normalization.
* MOD:
* MM: Maybe there’s another compromise: instead of having the browser or the shader compiler normalize, we could reject non-normalized text
* BC: Our stance is that we either fully normalize within the compiler, or don’t at all. Expecting earlier stages to do it results in non-portability. We need to cover cases of offline tooling, and if there’s a discrepancy between cases where we allow non-normalized code and where we don’t, it will create portability problems.
* MM: JS could meet a normalization requirement by applying normalization just before calling into WebGPU
* KG: The complaints people would have about us normalizing things would already apply to String.normalize in JavaScript, which we already have and which people use.
* BC: I also listed reasons why depending on normalization in the browser is going to bite us, in terms of syncing to a particular Unicode version.
* KG: We should just accept the definition of whatever String.normalize does.
* MM: This is a case where the benefit of normalization outweighs the very minor risk of new code points.
* MOD: Will it be possible to ensure the closeness if we do XID_start and continue and close?
* MM: We have no opinions about how tokenization works. XID_start/XID_continue would be fine, and Immutable Identifiers would be fine.
* KG: What we want to avoid at all costs is divergence from what ECMAScript does. We should specify our behavior by reference to ECMAScript’s String.normalize.
* JB: I think it would be helpful to gather up a list of positions, and it would be easier to see what we’re disagreeing about.
* MM: One thing that we could do for now is just limit everything to ASCII. We can always extend later.
* RM: Along those lines, we could say that everything is ASCII except for comments.
* KG: That’s what WebGL does.


### [Literal unification, bottom-up #2227](https://github.com/gpuweb/gpuweb/pull/2227)



* 


### [Resolve numeric literal types through inference. #2651](https://github.com/gpuweb/gpuweb/pull/2651)



* 


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, March 22, 11am-noon (America/Los_Angeles)
* 