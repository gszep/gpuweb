# WGSL 2022-03-01 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️ Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday ***5pm-6*** Americas/Los Angeles (APAC)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-02-22 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1DBiWJJZjZOJaZqZ6K85Y0dNCY8uF28rtwY382Hwtr1w) 

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
    * **Hao Li**
    * Jiajia Qin
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
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


### [Add acosh, asinh, atanh builtin functions #2581](https://github.com/gpuweb/gpuweb/pull/2581)



* To discuss: What to specify when math says there is no answer. (These are inverse functions, and some inverses don’t exist.)
    * Dzmitry said he wanted some principles.
    * No doubt there are other lurking cases of undefined results for math functions.
* AB: Difference from integer division:  There’s a different expectation. It’s looser for acosh, etc.
* KG: These come from the “sharp tools” bin.  If you cut yourself here, it’s your fault.
* MM: These functions are slow enough that folks won’t notice the select/if.  The condition in the selection/if would have to match what the underlying platform.  If the argument is numerically close then it might blow up anyway..
* DN: The regions are well defined, like x&lt;1.   Don’t think there’s a periodicity problem. Hyperbolic.
* KG: Convinced by the slowness. Can pick a value for the undefined region.
* _Consensus: Make it zero in the specified region._


### [Should unreachable-code be an error or warning? #2378](https://github.com/gpuweb/gpuweb/issues/2378)



* Alan’s proposal: [https://github.com/gpuweb/gpuweb/pull/2622](https://github.com/gpuweb/gpuweb/pull/2622) 
* Previously:
    * RM: Current state.  I was supposed to write a PR to show possible spec text.  Let’s delay.
* AB: There were actions for me and Robin.  I wrote my proposal, in a PR as draft, for review. Think it looks quite minimal in:
    * Non-uniformity problem in unreachable code is not an error. That’s what Google desires.
* MM: Robin is preparing a PR.  Let’s come back to this.
* KG: Will take time to do homework.
* RM: Looked at Alan’s PR.  Nice surprise that it’s smaller than expected. Don’t think I can make mine simpler or nicer. Ok with landing his PR.
* KG: Did you have different goal. 
* RM: I did, but realized I was in minority.
* RM: Also, David noted in an issue is that there is already a “should” obligation to issue a warning for the existence of the dead code.  Don’t think it’s too bad to not get uniformity errors in that dead code. Implementation won’t be *completely* silent. Programmer will get notification to look closer.
* KG: **Ok, have consensus direction**.  I need to review.
* MM:  This program:   ​​  fn foo() { return; break; }    is valid.  But has a “should” obligation to issue warning.


### [Should clamp be only defined for low &lt;= high? #2557](https://github.com/gpuweb/gpuweb/issues/2557)



* DN: What’s in the spec now is not great; because.
* MM: Proposed defining two functions.
* AB: Against defining both.  When do you tell people to use one and not the other. Preference is to accept both answers. Not a huge portability concer
* MM: Q1. Should we polyfill (Alan says no), Q2: If polyfill, which one.
* MM: (posted a table of results in the issue).
* MM: Can poll the device to see which implementation it has, and adjust the compilation.
* MM: Want to err on the side of being well defined.
* AB: Many more vendors that I care about.
* KG: Could say result is one of these two results.  Median or the min/max composition.
* AB: They will never disagree when they use parameters in the wrong places.  And it’s a small price to pay to get highest performance.
* DN: The function does what we say it does.  Like Kelsey’s suggestion.
* **_Let’s think about this._**
* GR: What API did the numbers come from.
* MM: Metal.
* GR: HLSL’s two compilers do the min/max polyfill.
* AB: Expect a driver will reconstitute the clamp.
* MM: The fact that HLSL always uses the polyfill is a good evidence that it’s fast enough.
* MM: Another thing that would convince me is data showing a significant slowdown.
* GR: Note that I didn’t check spir-v backend btw.
    * (After the meeting, DN verified the SPIR-V backend to DXC produces the Clamp instruction, not the min/max composition).


### [wgsl: Reserve words from common programming languages #2617](https://github.com/gpuweb/gpuweb/pull/2617) 



* Reserved all keywords from a bunch of languages.
* Did not reserve:
    * anything special from MSL (apart from C++20), and 
    * C preprocessor words (eg. ifdef)
* **Resolved: land, Myles to propose some further refinement later**


### [Correct the definition of fragment-stage position builtin #2625](https://github.com/gpuweb/gpuweb/pull/2625)



* **Approved.**
* **Waiting for IPR** .  Kelsey will track that down


---


# ⚖️ Discussions


### [Proposal of FP16 WGSL extension #2512](https://github.com/gpuweb/gpuweb/issues/2512#issuecomment-1050680075) 



* Intel
* ZJ: Have slides to present. Updates
    * Name the extension “fp16-in-shader-and-buffer”
    * WGSL source requires “enable f16”.  If used with unsupported device, produces shader-creation-error.
    * Q: When to raise the error:  shader-creation time, pipeline-creation time.
    * Define special structus for return value of frexp and modf; follow the pattern seen before.
    * Data-packing and unpacking builtins.
        * For unpacking need a new name to avoid having overload that differs only in return type.
    * No literal suffix specific to f16. Rely on type-construction form:   f16(1.0) when needed.
    * Bitcast:
        * No bitcast between f16 and other scalars
        * Add bitcast vec2&lt;f16> with 32-bit scalars,  vec4&lt;16> and vec2 of 32-bit types
    * Idea: maybe add raw-bit types like raw32, raw64, raw128.
* AB: For packing/unpacking, the SPIR-V versions always use the fp32 for intermediates, so there’s no profit.  Think we can drop the packing/unpacking additions.
* MM: The packing doesn’t need to be in the extension at all. It’s bit manipulation.
* ZJ: The slide deck is not shared.  Everything is in the comments, will put link to the comment.
* KG: In that comment is the hackMD document.
* AB: Suggest take the hackMD and make it into a PR.
* KG: directly into the main spec.
* ZJ: The packing functions. If we don’t change the name of the unpack functions, then we have the same function name and input types, but two different return types.
* AB: The point I was trying to make. In the SPIR-V backend, we would have to insert a conversion from fp32 to fp16. Don’t think there is too much cost for the user to write that conversion themselves.  
* ZJ: Agree with just keeping unpack built-in functions that return f32.
* _Discussion about whether one form same as a bitcast_
* AB: Yes, for one of them. But snorm and unorm need ALU.
* MM: Questions is whether to have (a) part in core part optional (b) all options .  Dividing line is whether you can store fp16 scalars.
* AB: Our stance is that should be a single feature, and optional_._
* AB: We don’t want to emulate fp16 arithmetic. So don’t add that behind the scenes.  Prefer a single optional feature for all of it.
* ZJ: Difficulty in supporting f16 on DXIL-based. Not supported on all devices.
* AB: We’re confused on what “core” means. The feature is in the main spec but not supported by a device.
* MM: In the option where half the feature is in core, the operations on f16 would be polyfilled on devices that don’t have native HW support for that. 
* AB: f16 can get infinities in intermediate. Introduces nonportabilities.
* AB: Also, user expects higher density ALU operation. Don’t pull the rug out from them.
* KG: Don’t want to make f32 pretend to have lower precision.
* AB: Would consider separate feature for “relaxed precision”. 
* MM: There’s memory use, and ALU density.
* AB: Experience in Vulkan: it’s a mistake to split the feature. It’s split between storage and arithmetic.
* AB: Important to signal to the author that they have to apply vectorization etc.  Give the user the info they need to make it fast.
* KG: Propose we land it as all in optional single feature.   If there is anything in the optional feature that we want in the core, address that later.
* KG: We know there’s _a_ feature that defines some of this functionality.
* AB: Google does not want any of this functionality in core-as-required.


### [Should WGSL support Unicode identifiers? #1160](https://github.com/gpuweb/gpuweb/issues/1160)



* [https://github.com/gpuweb/gpuweb/pull/2595](https://github.com/gpuweb/gpuweb/pull/2595)
* DN: We got the feedback from our Google i18n experts. Came in our afternoon.  We need to review.
* KG: Does Intel have any input?
* ZJ: No special input.
* MOD: Google’s feedback had “immutable” optional.  Immutable codepoint is not supported in existing regex engines.  Would be hard to support, when coming from a system that only has support for the XIDStart style.
* MM: Think both options from Mark Davis sound good.  Don’t think they will satisfy Greg. 
* AB: We’ll discuss with Greg, and resolve with him.
* MOD: If the first option is the one selected, then the PR already does that.
* DN: Even if option 1 is taken, still a question on how to phrase it.  Does normalization occur in the browser or in the compiler.  Usage in context of the browser is unaffected by this choice.  But WGSL could end up saying “WGSL source is normalized UTF-8”.  Can relieve an obligation for language tooling when used outside the browser.
* MOD: (Just a tiny addition that existing PR text does normalization in terms of canonical equivalence of identifiers, following the existing programming languages that utilize UAX15.)


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, March 08, 11am-noon (America/Los_Angeles)
* 