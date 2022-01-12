# WGSL 2022-01-11 Minutes

**🪑 Chair:** Kelsey Gilbert

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
    * Jaebaek Seo
* Intel
    * Jiajia Qin
    * Jiawei Shao
    * Shaobo Yan
    * Yang Gu
    * Zhaoming Jiang
    * Yunchao He
    * Narifumi Iwamoto
    * Hao Li
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * **Dzmitry Malyshau**
    * **Kelsey Gilbert**
    * Jim Blandy
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
        * [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)


### Virtual F2F



* Google proposes Virtual F2F first week of February.
    * (See email “WGSL VF2F February 2022”)
    * Focus: remaining work to finalize V1
    * Proposed schedule:
        * Day 1:
            * State of the spec (dneto)
            * Literal unification
            * Constexpr
        * Day 2:
            * Uniformity analysis (and related issues)
            * Atomics
            * Smaller data types investigation (native vs emulation)
        * Day 3:
            * Air time for Google, Apple, Mozilla and anyone else to present features or changes they want for V1
                * Ideally each company could have some slides/materials prepared if there are still changes they wish to make to WGSL


---


# ⏳ Timeboxes


### [Storage variables shouldn't be required to be structs · Issue #2188](https://github.com/gpuweb/gpuweb/issues/2188)



* (PR to make it so: [https://github.com/gpuweb/gpuweb/pull/2401](https://github.com/gpuweb/gpuweb/pull/2401)) 
* DN: Removes constraints that used to be in the language. Some pushback against having e.g. a whole binding for one scalar, want to authors towards backing things.
* MM: It’s not like adding a second binding point is free. If an app had a single float, and then another, they have two ways to do it. Neither of those is strictly “free”, as one needs more code in the shader, and one needs more code in the api side. Also, you sort of need more code in the api side than you would in the shader size.
* DM: I think we want to push people towards packing into fewer bindings.
* MM: I think that that’s true already without forbidding it, where managing multiple bindings is sort of more work than packing.
* KG: Let’s deal with it with education.
* DM: I think it’s a poor api if it needs more education.
* BC: The argument is you don’t want to encourage people that don’t really know about separation of variables by binding update; you don’t want the mentality of GL uniform. If someone is likely to make that mistake, then they’ll likely update the entire buffers many times, just as much as separate bindings.  They’ll update one big fat structure very frequently (which is also bad). Either people know how to use bindings well, or not.  This change doesn’t make mistakes worse.  Seen lots of code where a single binding is a struct with a single thing in it, and that’s unnecessarily verbose.
* MM: Concrete example shader. Binding point with a single field in it.  In Metal it’s common to have arg-buffers with gigs worth of data; and those are relatively static. But a separate argument  / bindpoint which is a time-variable parameter, that is updated very often. That’s a correct, valid, good-style shader that is permitted by this change.
* DM: If you have another frequently changing thing, where do you put it.
* MM: Either make a third binding point, or join it with the time-parameter.
* KG: How much do we care for v1, how much implementation work, how much can we disincentivize/ discourage people to funnel them to the “right” course.
* DM: I think if we provide this feature, we probably shouldn’t warn about it.
* MM: DM, do you think we should never add this? Like, not even after V1?
* DM: I think what we already require is fine/tolerable forever, and is all we need, yes.
* BC: From implementation perspective, it’s more work to enable arrays but not others.  Because target languages that only accept a struct will need work, but then additional work to carve out the array case.
* DN: Google’s consensus is that we should accept the PR, and remove these restrictions.
* MM: I wonder if a UA could watch the bindings, and report console messages if applications consistently update the same set of bindings at the same time. “Hey, you might want to coalesce these…”
* DM: I am weakly against, but if there is consensus otherwise I will not block this
* **DN to land**


### [Select operator arguments are in a non-intuitive order · Issue #2242](https://github.com/gpuweb/gpuweb/issues/2242)



* GR:
* BC: WGSL is not the outlier here vs. other existing shader languages. In issue, linked to a table comparing the prior art.  SPIR-V is the only on that’s different.
    * [on Jul 16, 2021](https://github.com/gpuweb/gpuweb/pull/1957#issue-946062961) ⇐= this is the table
* GR: Reject mix and lerp as being “the same” as select.
* KN:  Before “select” existed, folks would have to use mix to polyfill the behaviour.
* DN: In C and I think GLSL, the ternary operator is short-circuiting, which is different in my mind.
* GR: HLSL’s select() operator is not short-circuiting. HLSL’s ternary operator is.
* **No consensus to change. Leave spec as-is.**


### [Allow float modulo for scalar-vector and vector-scalar #2450](https://github.com/gpuweb/gpuweb/issues/2450)



* DM: Didn’t we have a reason for mod-float being different?
* DN: We *had* concerns but we addressed those already.
* MM: Our preference for consistency.
* **Spec needed**


### [wsgl: Add bit-finding functions. #2467](https://github.com/gpuweb/gpuweb/pull/2467)



* MOD: IDK if there is appetite but I think in the final spec it could be nice if there was an "Also Known As" section for functions with alternative names so that search engines can pick on it when searched with WGSL + alternative name
* Agree to add them, but AB and BC don’t like the “first bit low”, mistaking it as “find the least significant 0 bit”
* DN: “firstBitLow” is the close analog to the HLSL function and DXBC instruction “firstbit_lo”. That’s the audience for this function.
* **Approved**


### [wgsl: Add extractBits, insertBits #2466](https://github.com/gpuweb/gpuweb/pull/2466)



* KG: And we can polyfill this? (msl &lt;1.2)
* DM: Yes.
* DN: The tricky one was signed case for extractbits.  [https://github.com/gpuweb/gpuweb/issues/2129#issuecomment-1010267624](https://github.com/gpuweb/gpuweb/issues/2129#issuecomment-1010267624) 
* MM: Care mainly that the polyfill is branchless.
* DN: Yes, it was simpler than I thought, and branchless.
* **Approved**


### [Uniformity analysis #1571](https://github.com/gpuweb/gpuweb/pull/1571) (timebox suggestion)



    * Can we get consensus on major blockers to landing a first draft? e.g.
        * Uniformity level
        * Initial set of uniform things
    * Timelines
* RM: Still responding to comments. I think the main thing we still need is to enumerate uniformity for standard library funcs.
* AB: We think it’s fundamentally sound, with bugs to address and things to fill in. The real test of this is to implement and run against shared corpuses.
* KG: Can we split any of that up for other to help with?
* RM: Maybe, I’ll get back to you in office hours.


---


# ⚖️ Discussions


### [Literal unification, bottom-up #2227](https://github.com/gpuweb/gpuweb/pull/2227)



* (Also [Typing of literals #2306](https://github.com/gpuweb/gpuweb/pull/2306))
* MM: Twitter and internal webkit poll
* DN: Worth noting that shader math has reassociation, so you can end up with two different answers, whereas with double math, there’s only one answer.
    * Programmers generally have a poor intuition for floating point math.
* RM: I think people who…
* DM: I think that because we have different math in shader and in compiler, we will probably end up with different answers anyway.
* RM: We allow reassociation, but don’t require it. If you have IEEE both places, you could very well end up with the same results. 
* BC: I tried to compare the proposals for the edge cases/examples we’ve gathered. There was general consistency across implementations, with the exception of MTL. We have at least one backend that matches what we want to do precisely.
    * [https://github.com/gpuweb/gpuweb/issues/2200#issuecomment-989848073](https://github.com/gpuweb/gpuweb/issues/2200#issuecomment-989848073)
    * MTL compiler seems to only go up to 32bits?
* DN: Tex (HLSL team) reported that HLSL compilers use 64bit types for literal computations. [https://github.com/gpuweb/gpuweb/pull/2306#issuecomment-988251893](https://github.com/gpuweb/gpuweb/pull/2306#issuecomment-988251893) 
* RM: Not that happy with this special type, but if it’s been working for years for HLSL, I can concede that it must be ok enough, even if it’s a little gross to me.
* DM: Is that consensus?
* **Consensus: just use 64bit math (DN to take)**
* MM: On type inference for consts, I was surprised to hear that it “flows through” variables.
* BC: We can debate whether types are baked on const or not later.


### [Should unreachable-code be an error or warning? #2378](https://github.com/gpuweb/gpuweb/issues/2378) again



* (During office hours, we came to believe that we misunderstood each other when reaching for consensus)
* RM: I think it would be ok if it were a warning that users could choose to make fatal/an error.
* AB: I really really don’t like spec-mandated warnings
* DN: “SHOULD” says do it unless you have a reason not to, so that might match well for this.
* MM: I think we’d be ok with SHOULD instead of MUST
* MM: If there’s a uniformity error in dead code, is that an error?
* DM: Can there be? We would realize it’s not run, right?
* DN: We will post an example. It’s a subtle case.
* **Consensus: SHOULD warn on dead code**


---


# 📆 Next Meeting Agenda



* Next week’s meeting: 2022-01-18 5pm Los Angeles time (APAC)
* APAC by request!
    * Calendar invite has already been moved
* (RM will not attend, so no uniformity decisions there)