# WGSL 2022-01-18 (APAC) Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️ Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** (APAC) Tuesday 5pm-6 Americas/Los Angeles

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
    * Robin Morisset
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * James Price
    * Rahul Garg
    * Ryan Harrison
    * Sarah Mashayekhi
    * Jaebaek Seo
* Intel
    * **Jiajia Qin**
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

**1st week of Feb is Chinese New Year, let’s delay a week at least.**


---


# ⏳ Timeboxes


### [wgsl: OpXMod -> OpXRem for integers #2187](https://github.com/gpuweb/gpuweb/pull/2187) 



* (This is what we asked for, right?)
* David (offline):  This aspect is already covered by [PR #1830](https://github.com/gpuweb/gpuweb/pull/1830) “detailed semantics of integer division and remainder”. 
    * That PR does:
        * Defines all the cases for negative divisor and dividend
        * Handles overflow case for division ( MIN_INT  / -1 ).
        * Says division by 0 is a “dynamic error”
    * Further discussion suggested a polyfill that used ‘select’ to avoid divide by zero.
    * PR 1830 now specifies definite result for integer division and % with zero.
        * [Commit](https://github.com/gpuweb/gpuweb/pull/1830/commits/79b616f0cd26fd7f5c882d455f1825202d39ce2b)
* Dzmitry on Matrix chat said
    * “I know integer division is slow already, and we can make them a tiny bit slower
    * but if I have to use it for some reason, I'd be very pissed off by those 2 extra conditions on every division.”
* **For now, take 2187, then revisit 1830.**


### [struct decl should not have to be followed by a semicolon #2492](https://github.com/gpuweb/gpuweb/issues/2492)



* PR [#2499](https://github.com/gpuweb/gpuweb/pull/2499)
* **Agree to merge.**


### [wgsl: float to integer conversion saturates #2434](https://github.com/gpuweb/gpuweb/pull/2434)



* (Removes a TODO, and polyfills to avoid undef behaviour in several underlying targets. But the saturation is a bit weird, PTAL at the specific limits)
* JB: The weirdness happens because the clamp occurs in the float domain. Doing this way avoids multiple branches/ conditions.
* MM: Need to solve the undefined behaviour problem, as long as it doesn’t involve branches.
* AB: This is better than a bunch of selects.
* KG: Like DM want this behind a function call.
* AB: So you don’t like a type constructor from another type.
* KG: Example: clamp_to_int.
* MM: But if you really want the function name to spell out the behaviour, have to say clamp_then_truncate.
* AB: Not opposed to extra aliases if you think that helps. But like having the type constructor form.
* AB: Consensus this is the behavior for any conversion. But are discussing the name for it.  Suggest Kelsey or Dzmitry to open a PR.
* (Agree to land this.)
* 


### [remove stride attribute #2493](https://github.com/gpuweb/gpuweb/issues/2493)



* [PR #2503](https://github.com/gpuweb/gpuweb/pull/2503)
* Agree to land.


### [Are double brackets necessary for attributes? · Issue #2243](https://github.com/gpuweb/gpuweb/issues/2243)



*  MOD: Less wordy. Was raised again. Helps parsing.
* KG: Seems enough weakly opinionated agreement.
* AB: Also, every attribute is individual. No combined syntax anymore.
* MM: Seems fine.  Had talked about combining makes it more difficult for code generators.
* _Agreed on @-based syntax._
* MOD: I will do the PR.
    * [https://github.com/gpuweb/gpuweb/pull/2517](https://github.com/gpuweb/gpuweb/pull/2517) 


---


# ⚖️ Discussions


### [Proposal of FP16 WGSL extension · Issue #2512](https://github.com/gpuweb/gpuweb/issues/2512)



* a much more detailed design doc is proposed at[ https://docs.google.com/document/d/1TdHVV8NgQYANAq967L_rRHdJ7Uq_L8CGvq7_VY6G4OQ](https://docs.google.com/document/d/1TdHVV8NgQYANAq967L_rRHdJ7Uq_L8CGvq7_VY6G4OQ).
* GR: The data from gpuinfo.org is by ratios of #skus rather than # of units in the field.  So take the stats with a “grain of salt”.
* MM: Question about D3D f16 coverage?
* GR: Has been wanting this it the default in D3D land, but have not been able to make it so. Assuming f16 would simplify (.... D3D support ?) 
* GR: Browsers collect this info. May have metrics.
* AB: It’s not pervasive enough to make it core in the Android space.  So don’t think it should be core required feature.
* GR: Very much dislike precision-ambiguous types.  Would be 32-bit on some machines, 16 on others.  Affects interfaces, which is hard to debug.
    * [dn: I think min10float, min16loat in HLSL]
* AB: The analog in GLSL is lowp, mediump, highp.
* ZJ: We propose only supporting the extension where it’s natively supported, rather than adding flexible-precision types.
* GR: Great!
* ZJ: Native 16bit implementation only. Basically fp16 anywhere fp32 is except atomics. Not sure about uniform or pipeline IO.
* GR: floats are default to f16?
* ZJ: no, just a new type, f16
* MM: two questions: proposing webgpu feature to corresponds to all the vulkan extensions?
* ZJ: to be discussed. At least shaderFloat16 and storageBuffer16Bit. Allows all but uniform and shader IO (and push constant). Could emulate them.
* MM: tested 5 gpus: 11x regression, 12x, 20x, 40x, 120x. Tested the polyfill of atomic xor for writes.
* AB: We think the way Vulkan designed this was rather unfortunate. Don’t want to repeat that complexity. Want basically a single feature.   We think it’s ok to emulate f16 in pipeline IO and uniform.  It’s better than dividing it into a bunch of confusing and interacting features.
* KG: The lowest coverage is on input-output.  For input-output, those seem like we could polyfill.
* AB: using f16 on input output, you’re not saving any IO locations, so a valid implementation is use f32 on underlying pipeline, and quantize in ALUs on read and on write. It should be very cheap, relatively.
* ZJ: For uniform, since it is readonly, then there is no need to polyfill with atomics. So it’s just a data conversion (and figure out which half-word to read).
* ZJ: Think the key feature to turn on this feature in WGSL is whether `storageBuffer16BitAccess` is supported and also shaderFloat16.
* AB: W3C can’t force that choice on the implementation. But I agree that’s a very good place to start.
* DN: There is no atomic&lt;f32> anyway..
* MM: This is only about f16, not integral types.
* ZJ: Yes.
* MM: Don’t think integers are fundamentally more difficult.
* AB: Google also interested in integral.  May want another optional feature.
* MM: f16 is really important to us.  They are very much faster and smaller.  Integers don’t have all those extra benefits (in magnitude.)  Integral is good for ML.
* GR: How is the feature modeled on Vulkan
* AB: Storage feature bits are same between 16 bit types (float and integral).  8bit integer is its own independent feature.
* GR: Similar in D3D:  16bit types feature are grouped.
* MM: Metrics that would be helpful.  Correlation of features, e.g. if there would be different extensions that would have the same user base.
* KG: Kai reported (via email or an issue) worked with Sascha Willems, to get the data out under Creative Commons, with a more sophisticated query. Should be able to support compound queries.  It’s still SKUs but useful.
* KG: Want to get telemetry into Firefox to gather this.
* DN: Want to be clear about which builtin functions work with f16.  Also need to have table of accuracies.  Don’t care that it’s tight bounds, but that only that they are written down and testable.
* AB: E.g. derivatives. Also sampling may be interesting.
* … may be easy to polyfill
* AB: When we compiled OpenCL to Vulkan SPIR-V we did polyfills.
* KG: May add non-normative notes to indicate when the polyfill is going to be likely and therefore slower than using f32 directly.
* GR: Let’s discuss the suffix on literal.
* DN: I don’t like ‘hf’.  (put some notes on the issue).  I hope that with literal typing consensus, we would have very much less need to use these suffixes.  Would like to segregate this discussion if necessary.
* KG: Like Rust’s pattern where you use the intended type as the suffix.
* DN: I like that because it generalizes very clearly.
* GR: Like that explicitness.
* MM: Where it will become important is when we want overloads for user-defined functions. Our attitude is f16 is great and should be used as much as possible.
* AB: The overload selected is based on the arguments, and you will have at least one non-literal argument in almost all cases.  eg. Even for Clamp.

F2F agenda



* Don’t schedule over Chinese new year.
* AB: Agree the dates are still to be determined.  Let’s finalize that next week.


---


# 📆 Next Meeting Agenda



* Next week’s meeting: 2022-01-25 11am Los Angeles time (non-APAC)
* Detailed semantics of integer division and % [https://github.com/gpuweb/gpuweb/pull/1830](https://github.com/gpuweb/gpuweb/pull/1830) 
* With RM back: [Uniformity analysis #1571](https://github.com/gpuweb/gpuweb/pull/1571)
* 