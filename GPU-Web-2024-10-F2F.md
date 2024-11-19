

# GPU Web WG 2024-10-29/30 Mountain  View F2F

Chair: CW and KG

Scribe: DN, DS, CW, KR, KN, EG, …

Location: 900 Alta Avenue, Mountain View, CA 94303, and Google Meet


## Logistics

Both days will use the same Google Meet link: [meet.google.com/fwc-ziyu-wrv](http://meet.google.com/fwc-ziyu-wrv)


### In-person Logistics

Please tell [kbr@google.com](mailto:kbr@google.com) [cwallez@google.com](mailto:cwallez@google.com) [bajones@google.com](mailto:bajones@google.com) [kainino@google.com](mailto:kainino@google.com) if you plan to attend in person.

The meeting is at 900 Alta Ave, Mountain View, CA 94043. Enter via the main entrance at the center of the building, on the street side. You can park freely in the building parking lot during the day. (The lower level of the parking garage across the street should also be open if necessary.)

Both of us will plan to be available starting no later than 8:30am, to bring people in for breakfast (our cafe menu: eggs, potatoes, breakfast meats, oatmeal/congee, yogurt, and bread/baked goods).

We will try to start the meeting approximately on time at 9:00am. We aim to have 10-15 minute breaks every ~hour. Lunch break will start at noon and meetings will start again at 1:15PM.



* Day 1 is – ([click here for your time zone](https://www.timeanddate.com/worldclock/fixedtime.html?iso=20241029T09&p1=137&ah=8)).
* Dinner on Day 1 is  at [Doppio Zero Mountain View](https://maps.app.goo.gl/ridGpS485yQ4w5SW6) (a short drive from 900 Alta) - cost will be covered by Google
* Day 2 is – ([click here for your time zone](https://www.timeanddate.com/worldclock/fixedtime.html?iso=20241030T09&p1=137&ah=8)).

Dinner photo:



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.jpg). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.jpg "image_tooltip")



## Tentative agenda

Schedule:



* Intro
* Status updates from implementers, spec editors and CTS
    * WebKit status (Mike)
    * Firefox status (Jim)
    * Chromium status ([slides](https://docs.google.com/presentation/d/18Z9fdr-p8JZfVtquEhQp5MtXU5EINAJXRFsqKLVeVao)) (Corentin)
    * WebGPU/WGSL spec update (Kai, David),  ([slides](https://docs.google.com/presentation/d/1o-t4vJhNcz3BKnpPdNQ4_9O0_Sya_nARAHyjYNRc5Rg/edit#slide=id.p))
    * State of the CTS Update ([slides](https://docs.google.com/presentation/d/1_EyeaCm0Tu-xuAyKnbgQe8DZnNg9n4ObCrEJgC5Mxu4/edit?usp=sharing)) (Ryan)
* Cool demos

Tentatively 10:15 - 10:30 Break



* Make the specs ready for CR:
    * Milestone 0 PRs:
        * Rename image copies -> texel copies [#4838](https://github.com/gpuweb/gpuweb/pull/4838)
        * Allow featureLevel: "compatibility", which does nothing [#4897](https://github.com/gpuweb/gpuweb/pull/4897)
        * Remove textureGather on integer textures [#4904](https://github.com/gpuweb/gpuweb/pull/4904)
        * Add note about toneMapping mode in configure [#4922](https://github.com/gpuweb/gpuweb/pull/4922) 
    * Milestone 0 issues
        * Fingerprinting Surface [#3101](https://github.com/gpuweb/gpuweb/issues/3101)
        * textureGather with integer texture formats doesn't work on AMD devices [#4841](https://github.com/gpuweb/gpuweb/issues/4841)
            * The problem is that textureGather on stencil can cause hangs, see [#4857 (comment)](https://github.com/gpuweb/gpuweb/issues/4857#issuecomment-2355920741)

Tentatively noon - 1:15PM lunch



* Administrivia
    * Approve of the rechartering
    * Agree to take a CR snapshot and go for CR.
    * Reschedule Atlantic meetings earlier?
* Other status updates / non-feature presentations
    * Subgroups experience update (Alan) ([slides](https://docs.google.com/presentation/d/1JHnBG0R2PxhdxWp26QrjDls-fpNvfqWQNbrDWI0VyxQ/edit?usp=sharing))
    * WESL and improvements for WGSL libraries (Lee Mighdoll and Stefan Brandmair) ([slides](https://docs.google.com/presentation/d/1rBSSPT6X67IJQogS_GNd6niQEt65dytN49Punv3p8RQ/edit#slide=id.p))
    * Presentation / discussion about Slang and its WGSL backend (Nvidia) (30m) ([slides](https://docs.google.com/presentation/d/1tYhc0uJE2ja6iQuylOlMxDTPP3jjipyXAtR63-JIncc/edit#slide=id.p)) [Request EDT-friendly time; EET-friendly time even better but not critical - also request **not** 2-2:30pm Pacific]
        * Related: Tint’s Vulkan SPIR-V to WGSL converter (David)
    * Canvas2D WebGPU access [proposal](https://github.com/fserb/canvas2D/blob/master/spec/webgpu.md) (Fernando, EDT)
    * **Your update here!**

Tentatively 2:45 - 3PM break

Order below is more in flux



* Burndown a number of [compat issues](https://github.com/gpuweb/gpuweb/labels/compat). Timeboxed.
    * Allow featureLevel: "compatibility", which does nothing [#4897](https://github.com/gpuweb/gpuweb/pull/4897) (Kai)
    * Reconsider “renderable float16, float32 textures” [#4417](https://github.com/gpuweb/gpuweb/issues/4417) based on new data from the [comment in #4721](https://github.com/gpuweb/gpuweb/issues/4721#issuecomment-2237577058)?
    * Store buffers in fragment shader limits? (minor issue: CTS tests may require this and need re-writing)
    * Reconsider [#4513](https://github.com/gpuweb/gpuweb/issues/4513) (“Compat: Disallow texture*() of a texture_depth_2d_array with an offset”) in favour of workaround/polyfill
    * More …
* Fuzzing lessons from wasmtime
* Update and questions on WebXR/WebGPU interop ([slides](https://docs.google.com/presentation/d/1XEzrBrxJ_p9XTEUojibG2Ct2Ft1f7ul4ggzulCu-jpg/edit?usp=sharing))
    * Querying XRCompatible adapters
    * Patterns for efficiently rendering multiple views 
* New features:
    * Support for arrays of textures [#822](https://github.com/gpuweb/gpuweb/issues/822) [#4940](https://github.com/gpuweb/gpuweb/pull/4940) (Corentin)
    * Bindless ([draft proposal](https://hackmd.io/PCwnjLyVSqmLfTRSqH0viA?view)) (Corentin)
    * Dealing with holes in the pipeline layout. [#2043](https://github.com/gpuweb/gpuweb/issues/2043) (Corentin)
    * UMA buffer mapping [#2388](https://github.com/gpuweb/gpuweb/issues/2388) (Corentin)
        * Interested: Mike Wyrzykowski 
    * Texel buffers. [PR #4912](https://github.com/gpuweb/gpuweb/pull/4912) ([slides](https://docs.google.com/presentation/d/1XR6TbSuJpZ04UA6Q7cwllJNabSagpUE7lZC3dJAkx-0/edit#slide=id.g30fa23859c2_0_0)) (James - EDT)
    * Allow getting adapter information from GPUDevice [#4810](https://github.com/gpuweb/gpuweb/issues/4810) (Intel)
    * [late] Proposal: formats-tier-1 extension [#3837](https://github.com/gpuweb/gpuweb/issues/3837) (Kai/Teo?)
* Agenda for next meeting


## Attendance



* Apple
    * **Mike Wyrzykowski**
* ByteDance
    * **Qinchuan Yang**
    * **Yunchao He**
* Google
    * **Alan Baker**
    * **Antonio Maiorano (EDT)**
    * **Brandon Jones**
    * **Corentin Wallez**
    * **dan sinclair**
    * **David Neto**
    * **Francois Beaufort**
    * **Geoff Lang**
    * **Gregg Tavares**
    * **James Price (EDT)**
    * **Kai Ninomiya**
    * **Ken Russell**
    * **Loko Kung**
    * **Peter McNeeley**
    * **Ryan Harrison (EDT)**
    * **Shrek Shao**
    * **Stephen White**
    * **Victor Miura**
* Microsoft
    * **Rafael Cintron**
* Mozilla
    * **Erich Gubler**
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * **Teodor Tanasoaia**
* Nvidia
    * **Shannon Woods**
    * **Theresa Foley**
* Unity
    * **Brendan Duncan**
* **Albin Bernhardsson (ARM)**
* **Francois Daoust**
* **James Moir**
* **Jasper St. Pierre**
* **Lee Mighdoll**
* **Mathis Brossier**
* **Mehmet Oguz Derin**
* **Rob Conde**
* **Stefan Brandmair**


# Day 1 Notes


## Logistics



* CW: Meetings 9-5, hour+ lunch block at noon PDT, US has not changed clocks yet. Discussions 1hr to 1hr 15 then breaks for 15ish minutes. Agenda in doc. This morning:
    * Round table
    * Status updates
    * Hash out M0 issues by noon, hopefully arrive at CR candidate.
    * After lunch: administrivia
    * presentations and discussions on non-status update things
* CW: Suggestions for agenda ordering, please raise them
* Intros ….
* CW: WebGPU is a W3C working group which has a bit more IP commitments if committing to the spec. Not to say if you aren't a WG member you can't say anything, but if there are super detailed design discussion, then might call out that it's getting into contribution territory and if not a WG member please refrain.


## WebKit status updates



* MW: Functionally, largely complete. In Safari preview for several months. No comment on ship date. Functionally, there are a few missing CTS features we haven't seen used (like importing texture formattings is known not to work). HDR support on WebKit main is added, expect to see in preview soon. Otherwise, looking into iOS memory usage issues and various security issues. Hope to ship soon.
* CW: Saw parts of WebGPU on Safari is in Swift, is there a plan to rewrite for memory safety?
* MW: Yes, commits are being made in repo, WebGPU serves as a good test environment. WebKit uses some Swift, but not a lot. WebGPU is a good test case if it can be added, what might prevent and what benefits. Compiled off by default, early experimental phase. Not sure when it will ship.
* KR: Mentioned some parts of CTS not functionally used, comment on pass rate?
* MW: `api,execution,shader,execution` at some point last sprint mostly passing, new tests added and don't pass them all. Worst at `webplatform` category of tests. Some interop between canvas, importing textures, external textures. Formats seen in wild have been implemented, and a few others, but have not done all formats. Some videos as external texture will not render correctly. Would like to fix bugs, but consider lower priority bug fixes, as we haven't seen it used. If we see it used, or a working group sample uses it (e.g. transparent canvas) which we want to fix but have not seen it used outside sample.
* JP: Have you done uniformity analysis
* MW: We have not, completely non-passing. Difficult to justify from implementation standpoint. Especially with some requests to disable it. If disabled, then not as useful we feel. Plan to implement as required by spec but not gotten around to it.


## Firefox status updates



* JB: Enabled in nightly, not in beta or release, targeting public windows in h1 2025 with Linux and mac to follow soon after. Have CTS largely under control. When pulling in new WGPU we get unexpected passes and have system to pass. Will not comment on pass percentage.
* EG: Not as high as we'd like.
* JB: Current focus on compat and fixing missing WGSL features which aren't implemented. API side much closer to spec. Mostly work in Naga. We have curated list of high impact things to focus on, then applications and demos which are high profile to make work. Will focus on that. Then will look at CTS results and see which ones we intend to fix and which we'll ship without fixing. Won't wait for 100% CTS to ship. Biggest area of concern is around security. As everyone knows, GPU device drives are not security boundaries. That's our job. Don’t think we have enough fuzz coverage, great team, but need to look at what WASM Time has done. Can take fuzzing input and produce valid WASM modules from that. So, fuzzers can get into code gen and exercise sandbox. Much better coverage then naive approach. Need to do at least that. Have a serious responsibility here. Have strategies we want to pursue and will chaise them until we're comfortable.
* JP: Is uniformity analysis you'll address before shipping?
* JB: Think we will do before shipping. Have an overly conservative analysis from before uniformity was in the spec. Expect we'll update that before ship. Understanding is that is necessary for compute shader barrier soundness. It is an interesting algorithm, and understand why you're curious.


## Chrome updates [slides](https://docs.google.com/presentation/d/18Z9fdr-p8JZfVtquEhQp5MtXU5EINAJXRFsqKLVeVao)



* CW: We shipped WebGPU along with a few more features to the spec. WGSL features, shader f16, various formats, WebCodecs integration. Shipped on Android on some devices.  Tint moved from AST centric view of compilation to an IR based, much faster. More maintainable. Shipped DXC which triggered a number of security issues. Not designed to be a secure compiler.
* DN: Rate has dropped very far off. Fuzzers could get through WGSL and trigger DXC issues.
* KR: Antonio figured out issues with Asserts
* AM: More than half of issues would trigger DXC assertions. We added the ability to trigger asserts in Release and that massively slowed down bug rate.
* CW: Experimenting with subgroups to get feedback. Trying to drive to 0 CTS validation failures, as that's under our control. Close. CTS execution failures are much higher because of drivers.
* CW: IR is much simpler to do transforms. There is also an IR fuzzer to fuzz at that boundary. Lot of work to layer Chromium on Dawn. Skia next gen rendering is layered on Dawn in chromium. In process of shipping on macOS. Testing on Windows D3D11 and android. Showed need more 2D rendering extensions, pixel local storage, etc. Can make proposals if interested. Will have a GLES 3 and 2 backend, won't be advertised, internal for Skia. People satisfied but needs more for 2D work.
* JS: For render pass optimization, do you mean subpasses?
* CW: This is more for things where you can load multisampled data from simple sampled texture. To reduce memory traffic and having renderer smaller than render pass so you only touch certain styles.
* JS: Does scissor not work?
* CW: Scissor is in render pass. Maybe some backend drivers do things, but scissors you can change in the render pass.
* CW: Work in progress to make Angle run on WebGPU. Early days. Eventually take WebGL GPU process code and move to render process. Reduces attack surface as it's much more sandboxed. May cause WebGL regressions, but hopefully more content will have moved to WebGPU. Hope to have a Media/Dawn GPU process and everything else talks through interfaces. Not sure if practical. Lots of ML using Dawn. Lots of other WebGPU launches, compat mode is working to lower CTS failure rate and fixup implementation. Working on WebXR/WebGPU interop.  Push constants.
* CW: Also doing ecosystem work. Spec, CTS, Node.js bindings that were borrowed by dawn. Trying to stabilize webgpu.h to be a common header between rust and dawn implementations. Also in WebKit source tree. Trying to stabilize so we have v1 of JS spec and v1 of C header, so applications targeting in C will not have churn. Started on Kotlin bindings.
* YH: Dawn is for hardware rendering of Skia. For low-end devices like ES 2 do we have the requirements to support? instead of falling back to software
* CW: Devices are low-end devices, GPU is slow, and the CPU is slower. Really want to use the GPU as Chromium does not support software rendering of Skia on android.
* JB: The flow diagram for tint looks almost exactly like the one for Naga including the hidden AST and the SPIR-V IR. We do not have backend specific IRs and we suffer for not having those.


## API / WGSL spec updates [slides](https://docs.google.com/presentation/d/1o-t4vJhNcz3BKnpPdNQ4_9O0_Sya_nARAHyjYNRc5Rg/edit#slide=id.p)



* KN: What is left in M0 is CR tracing issue, PING fingerprinting, 1 API+WGSL issue (`textureGather`). 3 things open against API, feature detection, feature-level compat to have spot for later and the rename thing. 0. 
* KN:M1 has plenty of copy editing issues, plenty of feature requests. Nothing super urgent. CR should be ready by the end of the F2F.
* DN: Very few issues in M1, one issue (`textureGather` in M0). M2 is "programming in large", things like sugar (so things like WESL).  Hardware features. M3 has 92 items. M0 is basically done. M1 is very near term and very much under control. M2 is features. Compat has a bunch of M1 issues but not for CR. Basically under control.
* DN: Themes from M0, most drive by conformance testing. Lots of floating-point issues. Things like overflow and floating-point exceptions when in const-expr shall be a shader creation error, so really have to validate it. Drives a lot of edge cases: when are expressions actually evaluated, what if they’re specified in the module but not used in pipeline. Lots of compat mode things which were shader-creation errors, but then moved to pipeline-validation. Lots of detailed stuff only caught because of detailed CTS tests. In my opinion, ready for CR. 528 closed issues which were M0. Often getting bugs saying things aren't covered or correct but is already in spec and just have to point to spec locations. I try to avoid redundancy but dan [sinclair] makes me add them. Spec is not a tutorial, so doesn't teach about floating point. Thanks to Oguz and Alan who moved mountains.
* CW: Yaa for CR (hopefully)


## State of CTS [slides](https://docs.google.com/presentation/d/1_EyeaCm0Tu-xuAyKnbgQe8DZnNg9n4ObCrEJgC5Mxu4/edit?usp=sharing)



* RH: Went through big CTS push and what's remaining. 
* `Who Am I` slide
* `What does the CTS cover?` Slide
    * RH: What does CTS cover, everything in spec should have test. If you don't test it will deviate. Allows interoperability. Two broad categories, execution tests which are supposed to run, like floating-point math. When do addition or cos the value returned is within spec intervals. Other is validation, should this run at all. If floating-point values go out of bounds in const-eval, that is shader creation error. If in GPU, it’s a different behaviour. Makes sure implementation does the right thing. 4 high level dirs: `api`, `compat`, `shader` and `webplatform`.
* `What doesn't the CTS cover?` Slide
    * RH Some things it doesn't cover, no perf tests. Tests expected to be small and quick. No timing anywhere, and no performance tests. There can be timeouts, but that's due to volume. No security testing or hardening. Not doing fuzzing. There are implicit security tests as the spec is designed to be secure. Can be used as a base for sanitizers as it covers the entire surface of the spec. This could be a good fuzzer test suite to start with. Underspecified/unspecified, CTS only covers when codified and landed in spec. Don’t add tests beforehand. Don't get into implementation specific details. Unless spec says how to do it the expectation of the test will be broad. Assumption the loosest implementation of the operations and flush to zero. Whatever is the least accurate is handled, more accurate is great  not dictated. Some FP edge cases that are hard to test in detail. There is an fp_primer in the docs repo if you're really interested. If the spec doesn't mention something, or it's vague, or unclear, we file spec bugs. If you don't understand how to test in CTS, it’s a good thing to file a spec bug for. Concept of undefined value, if running on value if this goes out of bounds you can return anything, still tested in CTS, just tested that something returns.
* `I noticed a bunch of commits to CTS in Q1/Q2…` slide
    * RH: Pile of commits in Q1, Q2, Dawn, Tint and Intel did a bunch of work for spec coverage. Various people put in a week or two and a few dedicated sprints. Took about 3 months. I read the entire CTS spec, and made line items for everything (not everything was weighted). The number of items increased as things got broken down. Things like textures is much larger than int initialization. Idea was to get things done, as looking at years of WGSL work. In Jan, 17%-ish done of WGSL spec, three months later stopped sprint and was ~95% done. Sitting at 97.5% now (although that 2.5% is large). Lots of spec feedback as we did this for things that were ambiguous, or needed details or GPUs didn’t work as we expected.
* `A fancy graph of CTS Sprint progress` slide
    * RH: [lost]
* `What remains to be done?` slide
    * RH: API side needs to converge [for] completion. Keeping in sync with spec changes. Compat, driving a lot of compat mode through CTS. Trying to get passing CTS and then seeing it's failing and determining if we need to change compat, cts or API. WGSL, texture built-ins need to be complete (mostly done, most common textures are tested). Bring up on real devices. Some tests will need to be loosened due to real hardware. Gradient mipmap testing. Invariant and a few built-ins need coverage. General correctness and perf improvements. Try to be selective on what we test. But a large amount of things to run. Making tests run 1% faster can have a substantial impact on running suite. Recent changes for congestion of tests. Lots of feedback from folks on the values produced. There may be bugs in CTS, if something doesn't look right the CTS may need to be fixed.
* `Call to action` slide
    * RH: Please run CTS early and often. From Dawn/Tint helped us understand what we needed to work on. Running early shows real hardware isn’t doing what you expect, or there are CTS issues or spec issues. Finding early gives time to work on them. If you think a test is wrong, there is a change it is. FP math is hard. Chrome is addressing test failures, Tint/Dawn are not 100% passing CTS.
* Q&A
    * JB: Talked about reviewing WGSL spec item by item. Was it difficult to identify the items and keep track of them as the spec got edited. Vulkan VUIDs are kind of unwieldy and are ugly but have merit of referring to claims in the text which are stable. We wish those existed in some form. Do you have suggestions? What are you using to make references to individual pieces of the spec?
        * RH: For CTS sprint, basically took a snapshot of what spec said at the time, and spent a week reading through and made a spreadsheet for each section had a sheet. Then worked through with hard links into that version of the spec. Wasn't intended to be long-term maintainable living doc. Sitting at a point in Jan. where we didn’t know what remaining work we had. Was a DIY system. Lot of TBD came from not understanding that part of the spec. Knew others were, so gave tag that someone else could fill out. Wasn't intended to be a formal living thing. If we could anchor to the spec that would be good. Could be painful with the spec changing as would need to reflect somewhere else. Didn't need to write this at a level for anyone outside the WG.
        * JB: SO a one-shot thing
        * CW: For VUID, an idea was raised 4 years ago at the time we decided not to add error IDs. One concern was over constraining implementations on API side. Validation of WGSL could be tractable to make CPU cycles to make error. For API side was strong concern about preventing implementation smart-ness by forcing errors to be handled in a specific way.
        * KN: Didn't want API to expose error code as requires validation happens in specific order. For this purpose don't think we'd need that. Think it could be valuable in spec that are just for human references.
        * JB: Just want comments in the code.
        * KN: We've done it with links into the spec. On APItry to keep old anchors into spec. Which works but is looser.
        * JB: Think if there was a way to make links to validation bullet points that folks tried to keep stable, that would be the 80% solution.
        * CW: Would help searchability of errors
        * DN: Given this was 1 shot, this was the 3rd attempt. What worked was that it was a breadth first search through the table of contents and stopping. Whoever picked it up broke it down more. Having breath first give us a thing we could count. For evolution. Each spec change I have a column in a spreadsheet for CTS getting filed for things.
        * JB: Kind of like going through git log.
        * AB: The WGSL side we've been diligent that things link to the specific type of validation error. Every link off of the spec was a thing to validate. Can't capture everything but that's the way to do it in wgsl.
        * RH: Process developed over time to link into the github repo where the tests were added so you could go and look at them. Convenient if can't remember where something lives, good to go back to the sheet to find them.

**Demos:**



* CW: I realize I didn’t make a call for demos before, so if anybody has a demo *right now*, then great, let’s do it, otherwise, let’s proceed to a break.
    * [no takers]
    * Alright, let’s go on break.


## Milestone 0 burndown


### Milestone 0 PRs



* Rename image copies -> texel copies [#4838](https://github.com/gpuweb/gpuweb/pull/4838)
    * KN: Nothing additional to say.  What remains is naming proposals.  
    * KG: Posted a comment.  Some aspect of bikeshedding. Corentin had a point that these names are the same that C headers use.  In C headers you’re much more likely to have type them a lot.  JS specs are less sensitive to that because of defaulting.  Need more care in the C world.  My suggestions are at [https://github.com/gpuweb/gpuweb/pull/4838#issuecomment-2444931287](https://github.com/gpuweb/gpuweb/pull/4838#issuecomment-2444931287)  [ the ones that end in `Desc`
        * In Web IDL we often use `Source` suffixes when doing a typedef union.  Would be nice to retain that.
        * Like to draw a strong distinction between *data* and *objects.  *Use `Dict` for dictionary, `Desc` for descriptor.  I like `Dict` because in JS [Web IDL] these are dictionaries.
        * Usually shorten “destination” to `Dest` even if we don’t shorten “source” to `src`.
    * Jasper: I like `Dict`, but not in the C header.  Seems wrong for C++ and C and Rust. Because they are `struct`s, not dictionaries. Pick up the pattern used elsewhere.
    * KG: Think we get `Desc` from Vulkan.
    * Jasper: `Desc` is used everywhere.
    * CW: You have to type these out. They are a mouthful. That’s why I’m partial to the previous names. “GPU texel copy buffer descriptor” is a lot. That’s why I’m worried about this rename.
    * KG: Buttons-pressed wise are longer, but not much longer.  It’s a balancing act.
    * KG: The reason we desire these names is the distinction between the designating the object, or the data describing the object (e.g. layout and size).
    * Jasper: I am fine with the bold names.
    * CW: The bold names are Kai’s.
    * KG:  my recommendation is the ones that end in `Desc`
    * CW: I don’t want to block this. Go with the ones end in `Desc`
    * KN: How do you feel about `Info`.
    * CW: Descriptor is not shortened anywhere else.
    * KG: It’s good that it’s different from all the other “Descriptor” uses in the web API.
    * Jasper: I like the bold names… ending in `Data`.
    * KG: But you’re not copying `Data`.  Often have to clarify with folks with the difference between the data and a description of data.
    * MW: outside of `GPU` we don’t have a lot of other abbreviations.
    * CW: The only other ones we have are `Dest` and `Src` for various things.
    * KG: In the field *that* is very common.
    * KG: I’m proposing using `Dest`.
    * KG: I’ll post a PR .
        * I could be ok with Info. Seems it has the fewest objections to that?
    * LokoK: Said “Shape” (but got vetoed on basis of not communicating enough)
    * KG: I will post a PR 
        * with `Info` it’s least objectionable.
        * `SourceInfo` and `DestInfo` feel natural.
        * Leave the typedef as external image `Source`.
    * `GPUTexelCopyBufferLayout` \
`GPUTexelCopyBufferInfo` \
`GPUTexelCopyTextureInfo` \
`GPUCopyExternalImageSourceInfo` \
`GPUCopyExternalImageSource` \
`GPUCopyExternalImageDestInfo`
* Allow `featureLevel: "compatibility"`, which does nothing [#4897](https://github.com/gpuweb/gpuweb/pull/4897)
    * KN:  currently spec will reject any string it does not know.  To allow compatibility to be ignored later, we have to allow it now and let it do nothing for now.  So we have to 1. Decide on a name, and 2. Allow its behaviour to be upgraded later.
    * JB: The ability to do silent upgrades has been part of the discussion of compat since the beginning, so don’t object now.
    * KG: What worries me is saying “feature level” here, and we might do something else later. I’d be more comfortable if it said “min feature level”, and allow it to be upgraded. That would have less risk.
    * Jasper: are feature levels always to be backward compatible.
    * KN: Yes. backward compatibility, but not necessarily always automatically upgrading.
    * KN: We might say FL1 is not automatically upgradable to FL2 just for compatibility reasons.
    * Jasper: WebGL 1 to 2 tried to be compatible, it was sometimes painful.
    * KN: we could call it `compatibility_or_core `to be explicitly super clear.
    * KG: D3D lets you give an array of feature levels that your application could work with, and the implementation selects. That works. Might even be priority order. So make it an array and you get something that you asked for.
    * CW: We’re kind of meta on this. We don’t have to set it in stone right now; we think we have good will among browsers for this.  You have to reject on a string that is unknown, and included `compat` in that list.  I’m saying we don’t need to decide firmly now. Push this back when we actually ship compat.  And have the browsers implement the agreed.
    * KN: The risk is browsers that don’t support this string will be out there for a while. But not that long.
    * KR: We have all the browser vendors in the room.
    * JB: Firefox will get this done quickly.  I think compatibility_or_core as the string addresses the upgradability story. When folks write that they know what they’re getting.
    * CW: We’re trying to define how compat will work before compat is defined.  We don’t need to do that now.  I’m here we’re uncomfortable specifying this now.
    * KG: we added the featureLevel member.  Something that gets us past M0 is to remove that for now and push that to M1. Then we can sort it out there.  Consider it a prerequisite for landing compat as a feature. 
    * KR: Feels like we’re being too cautious. We know we need compat to get onto GLES 3.1 Android devices. We spec’d out the compat mode to work with that. I view this as relatively minor thing. It’s the only feature level we intend to auto-upgrade.
    * CW: uh..
    * KR: If “just” add this now we will get browsers out there that support it in not too long a time. It will get out there before it’s used in earnest.  If we don’t put this in now it will be a longer period of WebGPU content in the web which breaks in one or two browsers because they either don’t support `featureLevel` member or `compat` as an `enum` value.
    * KG: Concrete proposal: there is a table of feature levels in the spec. It says if you say `compat` you get either `compat` or `core`.  And if you say `core` you get `core`.  That’s it for now.  That feels safer than arguing upgrade paths.  No implied comparability between things.
    * CW: If that sounds good to you then that’s workable for compat on the Chromium side.
    * JB: To restate, change the type from `DOMString` to an `enum`, and explain in the spec you either get `compat` or `core` when the application specifies `compat`.
    * CW: Ok with Apple, Mike?
    * MW: As long as we have the auto upgradability, WebKit will be happy with any reasonable approach.
    * KN: Do we want to add the table now? Because compat doesn’t exist.  
        * KG: Put the table in today.
        * JB: Both entries should say the same thing.
    * KN: Do we give core a name?
        * JB: call it `core`.
    * Jasper: Does changing to `enum` mean it still has the same behaviours w.r.t. Defaulting in the dictionary?  [Yes]
    * Jasper: when i get an adapter info [object], do I read out of it what feature level is supported?
    * KN: Proposal for compat will deal with those details.  Not yet.
    * CW: The TAG will be interested in this.
    * KN: Remember now. The reason we made it `DOMString` not `enum` is to avoid erroring out at the IDL level if the enum is not known.
    * KG: Can we make it `DOMString` | `enum` ?
    * KN: IIUC not allowed in IDL, the types overlap.  Need to make it a `DOMString`
    * <code><em>featureLevel member will remain a DOMString, with two strings defined in a table in the spec. core and compatibility will mean the same thing for now.</em></code>
* Remove <code>textureGather</code> on integer textures [#4904](https://github.com/gpuweb/gpuweb/pull/4904)
    * CW: Very broken on AMD. We thought we were going to remove this, but someone (Jasmine/@JMS55) on Bevy said it’s used in an SSAO shader.  But then they said if it’s broken (which it is), and broken on Intel (always returns 0) then it’s fine to remove the feature.
    * Jasper: Is this stencil depth, 4 component, 2 component; what’s the bounds on the error.
    * GT: All formats. On Metal AMD.  Always returns the top left pixel.   …something else… always returns the first layer.
    * Jasper: Gather is used for shadows. … 
    * Jasper: I’m not opposed to removing it. Hopefully we can reach out to IHVs.
    * CW: On this one, it is a hardware bug. In the Mesa source code you see nasty workarounds.  Don’t think we can ever get a fix on Mac AMD.
    * KG: There are other options than removing texture gather for all platforms. It’s useful when drivers implement it correctly. If they don’t implement it correctly, we can implement it correctly for them at performance cost.
    * GT: There’s no texture load for cube maps.  
    * Jasper: you could rebind the texture as an array, and sample yourself.  I imagine shadow testing use u8norm floaty-kind of thing.  
    * GT: To emulate it you have to pass in all the sampler state. Including wrapping….
    * Jasper: I think we remove for now, and try to bring it back later.
    * MW: Have we considered making this an optional extension for the platforms that it works on.
    * CW: First step is to remove it.
    * MW: That’s fine. 
    * KG: I would like to be more specific about what is broken. Consider what bits are broken but still shipping in usable drivers.  Need to fix the crashes.  Texture gather is useful enough in shipping drivers even though it’s in a bad state. We don’t have to be responsible for getting it perfect if others are not as responsible.
    * GT: Int doesn’t work period, affecting all dimensionalities.  The Intel bug was 3D only. 
    * KG: The driver team there tacitly thinks its ok to ship this way.
    * CW: one way to work around is to have an expensive copy to sample-able texture, then sample that.  Or we can throw a pipeline creation error as an internal error.
    * Jasper: For system hang, can’t ship that.   For the other cases, get zeros or bad data; that’s ok.
    * CW: Don’t think driver vendors expect texture gather on stencil, and didn’t test that.
    * KN: I want to start with the smallest amount of stuff to remove or whatever.
        * First is stencil. We can work around it, but if the driver crashes then it’s probably something people don’t use. We can disallow that in the spec.
    * KG: We have an option in texture binding layout sample type, that has depth in it.  We can put stencil in there, and we can make it pipeline-time validation.
    * CW: That’s a breaking change. Now you can’t bind a stencil view to a uint, even if you don’t use texture gather.
        * <em>Consensus ah.</em>
    * KG: Alternately, pay the price of draw time validation.
    * GL: Is it knowable if this specific sampler is used for texture gather?
    * KG: We know static use.
    * CW: Very much want to avoid draw time validation. Rather have a very bad workaround for Mac AMD.
    * Jasper: if we say indeterminate result for this case. E.g. bind an empty case.
    * JB: Does this consume a binding slot.
    * KN: Bind two views onto the same slot?
    * CW: Not on Metal arg buffers.
    * GT:.... can’t do it.
    * Jasper: Just remove the feature?
    * KG: Objection: Is that not a breaking change also?
    * KG: This is where I want to know the matrix of failures.  If it works on Windows D3D for example, then let’s not throw that out.
    * KN: Can we dynamically avoid the crash failures?  E.g. send in a bit saying “this is a stencil”.
    * CW: Can work around it in implementation work later; and have an errata later.  Very much don’t want to do draw time validation.
    * GT: There’s a compatibility concern.  Seems weird to put in compatibility workarounds in so many places, except for this.
    * MW: I echo the same concern as it increases compatibility issues between differences between developer and customer machines.
    * BJ: What makes this different vs. compressed textures. I can shove BC data on my Android phone, but it will come out straight up wrong. We work around that by exposing a feature to name it.  If we have a set of devices where it works and others don’t.
    * KN: Then we have to validate it out. And a pipeline time validation, which then catches all integer texture formats (which is bad).
    * CW: We have 4 options.
        * 1. Remove it.
        * 2. Remove it and add it back as an optional feature
        * 3. Draw time validation
        * 4. Please implementation work around this.
    * KN: case: stencil, integer, AMD, Mac. Any dimension.
    * Jason: Feature of “integer gather” seems like a viable separate feature.
    * CW: Any of 1, 2, 4 work for me.
    * <em>Pausing discussion until after lunch?</em>
    * GL: A workaround is to use texture fetch and return a 4-dup’d value.  You get something better than zero.
    * Jasper: I need to know if that’s happening to me.  I need the feature flag to know if it’s happening to me.
    * KG: Maybe it takes > 1 year to have it working properly.  Consider we can iterate on this in our implementations.  
    * Jasper: I want to know which features work and which don’t.  I like the shader feature approach.
    * JB: One of the traditions this group has that I like is we avoid surprisingly costly polyfills.  We generally pitch the problem back to users, and that’s good.  The one time we except that is a tiny tail of populations.
    * KN: They will never release another AMD Mac, and haven’t since 2019…  This is also why we don’t think this will be fixed.
    * <em>Adjourn the discussion until after lunch.</em>
    * <strong>After lunch, still needs further discussion</strong>
* Add note about <code>toneMapping</code> mode in configure [#4922](https://github.com/gpuweb/gpuweb/pull/4922) 
    * KN: We are trying to describe the feature detection path.  The idea is to see whether the browser supports the option.  The question is exactly what the browser should do.  The spec requires HDR support in canvas. What should an implementation do if it’s out of spec and doesn’t support that, i.e. it will ignore your request. Only matters for devices that have HDR, since then things will be clamped to SDR.  Need to write the non-normative note to describe how such a browser should behave.
    * KG: Think we should write it as specified behaviour, rather than a not on “if you are not compliant in this one way, then be compliant in another way”.  I proposed something in the PR.
    * KG: I don’t want to talk about non-conformance.
    * KN: IDL doesn’t talk about it “is or is not exposed”.
    * KG: We have a way to do this in Web specs: describe your aspiration, and then fall short of that. It would be in the IDL all the time.  Avoid implying the UA is non-compliant.
    * CW: Can be a clarification after M0. But also have to be comfortable with the spec being a living spec. UAs will progress at different rates. It’s normal to not have the member there.
    * KN: There's multiple ways to not handle it: not having the member; having the member but …something 0…..
    * MW: We support it in WebKit main, and so will get into Safari Tech Preview.
    * CW: So we’re only talking about it for Firefox. So it’s ok for Firefox to progress as Kelsey says.
    * KG: The other thing I struck from the PR is the link to media query on whether HDR is supported. From Firefox’s view, HDR is not monolithic.  You can specify colours in HDR mode that are all SDR colours and we handle that. The media query is not related to being able to handle HDR colours in canvas.
    * KN: I want to assert that if the media query doesn’t say you support HDR then the canvas should not display HDR.
    * KG: I can’t agree to that right now.
    * CW: It’s weird to talk about browsers that don’t implement the whole spec. Don’t block CR on this.
    * KG: Think my suggestion gets us to CR.
    * KN: question, whether we should say <code>toneMapping</code> member is missing or <code>toneMapping.mode</code> is missing.
    * KG: It’s in a non-normative note so it doesn’t have to be that good.
    * <strong>After lunch…</strong>
    * KG/KN in agreement on how this should work / be used by users, will update PR.


## Milestone 0 issues



* Fingerprinting Surface [#3101](https://github.com/gpuweb/gpuweb/issues/3101)
    * KG: one of the core points was a strong request to put this behind a permission. I would like to mention that no UA plans to put it behind a permission behind by default.  This will make our reviewer unhappy but if they want it they can add it in their UA. We care about privacy but won’t put it behind a permission by default.
        * CW: We (Chrome) don’t plan to have a permission prompt for WebGPU ever.
        * MW: The same for WebKit. I’m not sure why this introduces a permission concern.
        * CW: Some browsers want to have zero fingerprinting surface. 
        * BJ: With WebXR, the vast majority of the spec says user consent must be given. There is only one bool that escapes without a prompt, and still got objections from the same reviewer.
        * KN: They want us to have a permission prompt because they want to have a permission prompt?
        * KG: Yes.  I want to write it so they (any user agent) can have a permission prompt.
        * KN: Brave can do what they want, as a user agent. They have a lot of flexibility.
        * KG: Firefox has facilities that are out of spec that let you enforce additional things. E.g. per page control of software-only rendering to reduce fingerprinting)
* `textureGather` with integer texture formats doesn't work on AMD devices [#4841](https://github.com/gpuweb/gpuweb/issues/4841)
* The problem is that `textureGather` on stencil can cause hangs, see [#4857 (comment)](https://github.com/gpuweb/gpuweb/issues/4857#issuecomment-2355920741)  

 



* CW: Summary for M0 we have 1. Integer textures, 2. Fingerprinting, 3. Tone mapping.
* Agenda-wise, will go to lunch back at 1:15 Pacific for administrivia, milestone 0, presentations.


## Administrivia

Milestone 0:



* Tone mapping:  Reached consensus over lunch.  Kai to update the PR.
* Fingerprinting:  KG has the answer, will drive it.
* Texture gather on integer.  
    * CW: Summary(?) is that Mac AMD is small enough so we can work around it. 
    * Jasper: Intel Mac?
    * KG: Care about Intel Mac a bit more than AMD Mac.
    * Jasper: Are we not worried about macs?
    * Jasper: Assume in Intel we can polyfill with 4 loads?
    * JB: Still in discussion. Need time.

Approve of the rechartering:



* CW: Charter has been under review.  Main changes are: 
    * using the new template for charters.
    * Clarification of the relationship between the CG and WG. 
    * Expiration date extension.
* CW: Comment on the charter, there is a discussion about which direction design goes. CG makes design and accepted by the WG?
* KG: “Note that the majority of the work will come from the CG. No major work will drive from the WG.”  This has been not true for most of the year. Talking to Francois Daoust to remove that clause.
* CW: The charter also describes the living spec process.  CR snapshot + … (?), as the new way W3C does living specs.
* **CW: Need the WG to approve the charter.**
    * ***Consensus by yay’s in the room.***

Agree to take a CR snapshot and go for CR:



* CW: Modulo the three outstanding items, shall we take CR?
* KG: Even with the errata, I think we’re fine for CR.  It’s about having issues known and under control. We’re at that.
* KG: We want to go for CR.
* CW: We want to go for CR.
* MW: CR would be great.
* RC: +1
* ***Consensus to take a snapshot and go for CR.***

Reschedule Atlantic meetings earlier?



    * CW:  Propose 7-8pm CET.  2 hours earlier than now.
    * KG: Happy with that.
    * JB: I will make any time.
    * CW: is that ok with you, Teo?
    * Teo: That would work better for me.

How we add features in the future.



* CW: With M1 we’ll start adding bigger features. Like compat, bindless, etc.  While it’s nice to have proposal documents that are detailed, sometimes it’s better to have a branch so we can see the rendering. Branches have discoverability issues, but in terms of spec editing is it fine to have branches with sometimes merge conflicts. Propose merges of branches once done.
* BJ: It’s been handy to break up the spec into chapters as files.  E.g. privacy and security.
* BJ: We may want to have a workflow where features are a subdoc, for maintainability purposes. That makes the conflict problem less.
* KN: Make that in the proposal directory. The challenge is things like one step in every algorithm.There’s often pervasive changes across the spec.  Biggest reason I didn’t want branches is because the spec was in high flux. Now it should be fine.
* David: really like having a single file for the WGSL spec. It is stabilized enough that's it's probably fine to have branches. Reviewed James' texel buffer extension that's very detailed like the GL extension. Useful but makes it hard to review. Fine to have branches.
* Alan: There's a point at which a branch makes sense, but a proposal doc makes sense before that. It's a matter of how far along in the process you are.
* KG: The proposal directory is still useful, but the idea is to switch to branches at *some* point when it’s relatively mature. E.g. compat should become branches.
* KN: Right now 1st stage is proposals, 2nd stage is PR. PR can exist for a long time and are hard to maintain. Branches are slightly easier to manage because it’s not owned by a single person. So we would add an option use a branch for stage 2 instead of a PR.
* DanS: Do you lose reviewability?
* KG: Make a PR into to pretend-merge into main.  Or make a PR into the branch.
* CW: It’s a way to scale more.  E.g. bindless will have extensive independent discussions on both API side and WGSL side. 
* Alan: On GitHub, maintainers can modify your branch.
* KG: We want a PR on the PR; that’s what a branch is for.
* KG: To enumerate the options. One of the ways working drafts work is all the edits happen in it, including half-baked ideas; and you pull it out before CR.
* DN: I don’t like that one.
* KN: I like branches better.
* DN: My expectation is branches are only for large items.  Majority (by number) of things we do would be by PR still.
* CW: Yes, basically, by t-shirt size.
* KG: E.g. we think compat should be a branch now.
* CW: Sounds good.
* CW: Sounds like we can proceed with branches.
* KG: When we go to CR, the CR snapshot should be a branch.  That’s the thing we publish with different metadata/background.
    * CW: Yes.  Will work out details with Francois Daoust.


## Other status updates / non-feature presentations



* Subgroups experience update (Alan) ([slides](https://docs.google.com/presentation/d/1JHnBG0R2PxhdxWp26QrjDls-fpNvfqWQNbrDWI0VyxQ/edit?usp=sharing))
    * Jasper: For devices that specify multiple subgroup sizes, can you force it at pipeline time?
    * Alan: You get what the driver gives you. They are extensions in Vulkan and D3D, and they are different models. D3D puts the constraint in HLSL and you fail compilation if it’s wrong. Vulkan was nicer.  We have a lot of requests to have more control. For base feature I don’t think we can do it.
    * Alan: Call for action to ISVs to give feedback on the defaults for subgroup diagnostics.
    * Albin: Unsure about use of subgroups in fragment shaders. One use is light binning/tiling when you don’t have conservative rasterization.
    * Alan: I see the use case of using broadcast to make sure all the invocations work on the same light.
    * CW: another use case, to force scalaraziation. Doom2016 GDC presentation where they talk about this.  All the same pixels don’t have the same set of lights, but altogether it’s only 20% bigger when you union them, compared to a single one.
    * Alvin: The technique is used by Activision on console.  During light binning.
    * Alan: When we tackled that in SPIR-V for maximal reconvergence, it was trying to solve the problem where nonuniform access to the descriptors is bad in general.  There are tricks like this to force it to be uniform in the subgroup. That’s an argument for leaving it in place, but very hard for folks to have portabile behaviour.  I don’t have a great proposal.  We could leave it nonportable and dev gets what they get, but with good performance.
    * CW: Do we lose coverage by requiring fragment subgroups support?
    * Alan: No. It didn’t cost us devices to have fragment support.
    * Alan: Q: is it time to write spec?
    * CW: Seems like it is about time given the discussion we had earlier on branches for features.
    * Alan: Do we want to add properties in the API first, as separate thing from subgroups, then land subgroups.
    * JB: Have we discussed fingerprinting aspects.
    * Alan: in the sense of will it create too many bins?
    * JB: Can we hide that in the bins we have already?
    * Alan: I think so; most IHVs don’t change their subgroup sizes.
    * 
    * 
    * KN: Subgroup min size and subgroup max size. Those are the two things that would become properties? Of an entry point?
    * AB: It’s on the device, saying it will be somewhere in the range (power of 2).
    * Jason: Can I get the subgroup size in the shader?
    * AB: It’s a builtin in the shader, not a constant, but it is uniform.  It is a runtime expression.
    * DN: The subgroup size is determined by the backend compiler 
    * CW: shader-f16.  If shader is present, and f16 is present, then you need to implement shader-f16.  
    * AB: Vulkan has a separate bit for that, but let’s not expose that additional wrinkle. 
    * KN/AB discussion: Vulkan has 3 capability bits (5 possible combinations), we only expose 2 (4 possible combinations). So on a device where we expose `"shader-f16"`, we need Vulkan to have both subgroup and subgroup-f16 support, in order to expose `"subgroups"`.
    * AB: For spec writing it’s already at the maximal set. So we can draft it and then pare back.
    * DN: Should we land properties first, the subgroups. Also how do we feel about the non-portabilities: very tiny workgroups; 
    * AB: can consider a driver bug; also very tiny 3x3 framebuffers.
    * BJ: Can we constrain wg size vs. subgroup size.
    * AB: If x dimension of wg size is a multiple of the max subgroup size then things work out well.  Maybe that’s too much detail.
    * CW: let’s work that out on the branch.
    * AB: Call for Naga to test their implementation vs. the CTS.  Flush out test bugs. Want to get more feedback.
    * JB: We haven’t reconciled what’s in Naga vs. the spec.
    * Teo: it’s pretty close.,
    * JB: We don’t have broad GPU coverage.
    * AB: Still useful to know if the basics work.
    *  
    * Teo: Feature name in CTS?
    * AB: `"subgroups"` and `"subgroups-f16"` for now. (didn’t have group consensus on merging `"subgroups-f16"` in so we expose both separately.)
* Presentation / discussion about Slang and its WGSL backend (Nvidia) (30m) ([slides](https://docs.google.com/presentation/d/1tYhc0uJE2ja6iQuylOlMxDTPP3jjipyXAtR63-JIncc/edit#slide=id.p))
    * RC: Is the goal for developers on the Web to write Slang and convert on the fly to WGSL?
    * SW: Not sure yet. Suspicion is that if folks are going to using Slang on the Web, either it is a demo like a playground, or you have a Slang codebase that you want to use everywhere and will do offline compilation.
    * RC: If you did do online compilation, how big a payload would the compiler be?
    * SW: Can't remember immediately. Slang can be split in several shared objects and you only need to ship the shared objects of the platform you are targeting, so don't have the overhead from other places. Less than 10MB but that's a big range.
    * GT: 2.2MB .wasm in the devtools, plus a few extra files.
    * KR: Really cool to see! All the modern language features are compiled down into WGSL. Any type compatibility difficulties in how template types are lowered?
    * TF: Generics are lowered basically as you would expect, substituting the types into the struct definition. However there may be dragons in struct layouts needing to match between the stages &lt;Tess to fill in>
    * KN: WGSL doesn't require that the vs out fs in structs match exactly. We just require that they contain the same `@location`s.
    *  JStP: Any WGSL feature or anti-feature that you'd want or not want? Was there any focus on the output size? Any feature that would help reduce the output size?
    *  TF: No big focus on the size yet, focused on conformance. One pain point was the very nuanced rules in where pointers can be passed / not. Inflexibility in pointer usage was the most frustrating part.
    * DN: Unrestricted-pointers help you pass in pointers, but aliasing restrictions are still in. Was to be nice to new developers on the Web
    * JStP: Are there considerations to make these disableable diagnostics?
    * DS: Any plan for WGSL input?
    * TF: No real plan yet, the different flavors are because that's what people use the most. Depends on how much code there is.
    * SW: GLSL/HLSL is an on-ramp for Slang, questions are whether we can accept all of HLSL and it's not the case. The goal is not to take all of HLSL but to make porting less painful.
    * SW: Also there's Intellisense support that's Emscriptened for the playground.
    * JB: Did you do incremental parsing or make the frontend parser faster?
    * TF: Made the parser faster.
    * KN: Some legalization in WGSL already, maybe some of these legalizations you do could be in WGSL? If they are fully robust.
    * TF: Some things where we can fully legalize them. Some where we cannot.
        * Local variables and global variables both have corner cases where they're actually not fully legalizable. We need to rule those out in the frontend.
    * DN: DXC team ran into this. There's a cookbook of code patterns that only work because of specific optimization passes. But making frontend language passes that guarantee that to work can be very complex.
    * TF: We are signing up to do this work in the long run, but have not formalized the limitations.
    * SW: Got an answer back. 7MB with all backends (for the playground). Less with only WGSL. Have done no work to reduce the size.
* Canvas2D WebGPU access [proposal](https://github.com/fserb/canvas2D/blob/master/spec/webgpu.md) (Fernando)
    * JSP: question about timing between WebGPU/2D canvas. Draw text, order, transfer - do I have to submit? How do I stay synchronized with the canvas timeline?
    * FS: the way we solve this right now - transferTo, transferBack - are de facto barriers for flushing. That forces a commit in both directions.
    * RC: one use case I've seen is a 3D scene with 2d overlays, UI. You'd start in 3D first, then transfer back?
    * FS: that case, this API, the canvas context would have to start in 2D land, so you can transfer it back every time. Not a big deal in WebGPU, because the canvas is almost orthogonal to the API. Unlike WebGL where it's tightly tied to the canvas.
    * RC: you'd start with a blank 2D canvas, transfer to WebGPU, do your rendering, transfer back, draw your UI?
    * FS: yes.
    * BJ: the proposal talks about transferring out, and what happens if you e.g. don't transfer back - can no longer transfer back at that point. Specific impl detail behind this - I can transfer back and forth once, but if something goes wrong, I break the association. Why can't I transfer out a stack of textures?
    * FS: 2 things trying to solve. 1) Making this a bit simpler. Zero-copy can be complex in situations. Technical problem: what happens to the canvas if the texture's transferred out, not transferred back, and you need to present? Drawing black is a bad idea, will cause flickering. If you transferTo and don't transferBack, maybe it should do a copy. No good answer for the state of the canvas in between.
    * CW: there's also a lifetime problem. Want to destroy this GPU resource now. Who do you tell that? Needs to be some ownership of the GPU texture.
    * JSP: does this work with OffscreenCanvas too?
    * FS: yes. You can do some other magic there. Right now, with OffscreenCanvas, if you do any reading it's the point of no return.
    * JSP: was thinking about text/texture atlasing.
    * FS: that's one of our use cases.
    * CW: in 2D canvas, have been proposals for more gradients, meshes, etc. Getting almost into 3D rendering. An API where you can compose the two - maybe not as convenient, because you have to set up a whole WebGPU rendering pipeline, but you have full functionality without introducing too many 2D APIs.
    * FS: allows people to extend 2D canvas. Think it's a fairly nice use case. Concerned about implementability across browsers. Think we've reduced the API surface enough so that's possible. Also allows making a copy in places if really necessary.
    * GT: what format's the texture? There's BGRA/RGBA. My pipeline has to match.
    * CW: you're told what the format is - it's in the IDL.
    * GT: what about storage textures?
    * CW: there's an option for the usage too.
    * BJ: with getTextureFormat, might that change from canvas to canvas?
    * FS: not in the same run, usually, but yes.
    * KG: in FF today, in the same canvas, it might change because we might choose a different rendering backend.
    * FS: same is true in Chrome.
    * BJ: if I have to check this every time when I transfer the texture, makes things complicated.
    * FS: fair point. We could allow people to make it stable, like, if you pass a texture format we'll try not to touch it.
    * CW: procedurally, how do we make progress?
    * FS: in next couple weeks, I have an issue on WHATWG - will share with this WebGPU working group. Other browsers will be asked for comment, probably folks working on graphics.
    * LK: in backends there's something like SharedTexture. Here we're doing an import and export from WebGPU. Would that be better? There are other requests from ML folks wanting to import/export buffers from WebGPU for interop with WebNN. In other words, would it be better to have this API on the WebGPU side?
    * FS: this only addresses half of the use case - drawing text to a WebGPU texture. But it doesn't solve the "extending 2D" case.
    * BJ: in that case you'd be creating the texture and feeding it into 2D canvas. 2D canvas would have to handle the format changes.
    * LK: understood. There was another proposal about buffer interop.
    * CW: there's a link to that in the WebGPU proposals.
    * FS: one issue - 2D impl could need a certain texture format to work. Could be hard to make WebGPU produce that.
    * CW: the same way we hardcode the same list of canvas formats. Trying to figure out rationale of importing/exporting things on one side or the other.
    * RC: the web ML working group has been talking about importing buffers into WebGPU. Explainer is here: [webnn/mltensor-explainer.md at main · webmachinelearning/webnn · GitHub](https://github.com/webmachinelearning/webnn/blob/main/mltensor-explainer.md)
* Tint’s Vulkan SPIR-V to WGSL converter (David) 
        * 

            ```
Tint can translate Vulkan SPIR-V to WGSL.
  - To help folks port existing Vulkan shaders to WebGPU
      Used by Unity. Works for them(TM).
  - Only for features supported by WebGPU.
      Won't do extensive polyfills, sorry.
          
  - For now, not officially a production flow.
          
      Takes a backseat to WGSL-to-others compilation!
        
      Hope to polish it in 2025, and advertise it wider.
             


Demo:
    
  $ glslc a.comp -o a.spv
    
  $ tint --format wgsl a.spv


  
Nerding:  
      
  - Influenced the design of WGSL:
      
        loop { continuing { } }

  - Control flow reconstruction algorithm is tricky!
    
     Helped refine(fix) SPIR-V structured control flow spec text.
     "Taking Back Control in an Intermediate Representation for GPU Computing"
     POPL'23
    
  - Spawned a test suite of SPIR-V CFG examples
    
     Snapshot at https://github.com/dneto0/spirv-samples 
     (Sorry, it's stale now)

```


    * DN: Talk about translation of spirv into WGSL. Developed this from the start. TInt can do Vulkan 1.1 spirv 1.3 to WGSL. Onramp from existing VUlkan apps to WebGPU. Think this is very important. Used by Unity. They have helped harden this flow, and we're super glad someone is using it and it works for them. It's not complete, it's only for WGSL features and we don't do extensive polyfills. Won't do combined image samplers. Not officially supported production flow but it's pretty good. First job of tint is WGSL -> other things so this is lower priority but it's pretty good. Hope in 2025 we're willing to advertise. 
    * DN: `glslc a.comp -o a.spv`
        * `tint --format wgsl a.spv`
    * DN: Translation is straight forward. Typically we do the transformation of GLSL and Vulkan spirv global inputs/outputs and you have to transfer it. Typically get a wrapper function on the bottom. See a mostly 1:1 translation of things. Loops will break out ot have a continuing block because that's the same as SPIR-V.
    * DN: Builtins are annoying because in SpirV you get a per vertex structure which gets cleaned up
    * JSP: Point size?
    * DN: We don't do that yet.
    * DN: Influenced the WGSL design. Source of loop/continuing. Control flow reconstruction is very tricky. Spir-v 1.6 rules were updated to better support this.
    * JB: This was structured control flow rewrite
    * DN: Yes, structured dominance. This was a back and forth with the redirection algorithm.
    * JB: They were using fuzzers?
    * DN: Yes, and they wrote a formal Alloy model to give counter examples. Spirv-reader unit tests up on github for folks trying to do this kind of reconstruction. It's out there, you can use it, we do fix issues, but maybe slow.
    * JB: Spirv instructions can be referred to by any code that is dominated by that? So you have to hoist values
    * DN: An essential PHI has to hoist, if it's a pointer, we don't handle it yet.
    * JB: Cases to hoist without phis?
    * DN: Yea there are.
    * JB: Naga’s [awful kludge](https://github.com/gfx-rs/wgpu/blob/a398d9990e3a7512c736b8d2be3bdfaa119df19c/naga/src/front/spv/mod.rs#L463-L497)
    * JB: Naga benefits from the structured control flow rules. “awful kludge” is unfair, it’s necessary given the situation.
    * DN: It's a subtle reconstruction
    * JSP: What happens if you discard in continuing.
    * DN: Discard turns into demote to helper which is not a control flow change. OpKill (i think) is not allowed by language rules
    * JSP: WGSL is a discard?
    * DN: It's a demote to helper
    * CW: In Tint, SPIR-V output there is an output that when demote to helper is not supported it's polyfilled.
    * JSP: Remember an issue with inlining where spirv-tools didn't deal with discard in continuing.
    * DN: Think that was solved. If inlining with multiple returns we stitch all of that out.
* WESL and improvements for WGSL libraries (Lee Mighdoll and Stefan Brandmair) ([slides](https://docs.google.com/presentation/d/1rBSSPT6X67IJQogS_GNd6niQEt65dytN49Punv3p8RQ/edit#slide=id.p))
    * LM: Introducing effort for community driven standardized community extensions.  Calling it WESL, the WGLS extended shading language. Trying to share effort of building tools and libraries and realized you need common version of extended WGSL. Looking at community folks are doing this already which makes tools harder. Lots of module systems, conditional compilation. A few implementations in the slides.  A few different module syntaxes. Some doing it with string manipulation. Can't build shared tools/libraries with a billion different dialects. Want some common standard.
    * SB: &lt;Demo in slides>
    * SB: Now when splitting shader code gets loaded as strings and assembled. All apps are slightly different out WGSL is output
    * LM: Trying to introduce a standard transpilation layer, either new, or replacing ad-hoc systems. Something standard and more featurefull then any one project has. Note, this linking/transplaition can happen at build or runtime.
    * SB: Currently language servers can understand a file but not other shader files
    * LM: If have a standard system them LSP can know how files are connected on disk and in libraries and enables ide feedback from tools. 
    * LM: What do we need from WGSL? Some kind of module system for both local files and needs to be extended for package libraries. Anything that is common in community for enhancements, like conditional compilation, need to standardize as they need to exist in libraries. When writing shader code for libraries need to author differently so the shaders can be portable into different applications.
    * SB: &lt;slides demo>
    * LM: Need to define ways to package public libraries. Want to publish to NPM and cargo for those ecosystems. Hope we can find ways to dual publish packages. Could create a package and publish in multiple ecosystems easily. If we could support in browsers, if it ever happens. What is it that lives in the package. Needs metadata which is standardized. Would like help for these and other design issues.
    * &lt;demos>
    *  LM: We have 3 linker implementations. Could use some test help
    * DN: Need to dump the CTS shaders
    * LM: Working on initial version now with key features and necessary tool and documentation. Beyond that have a lot of ideas but want to prioritize based on community needs.
        * Expect tools to get smarter, bundle plugins, etc
        * Want richer libraries which solve some of the issues raised. Some folks think generics would be useful. Polyfills. Anticipating that as WGSL adds new ergonomics features that the community will want it polyfilled so can be used on broader set of browsers during rollouts.
    * SB: We're happy to polyfill and test sugar features for WGSL.
    * LM: Lots of features worth discussing and we'll prioritize. Y'all are building the core stuff and that lets us play with sugar on top. Would like to figure out how to orient effort to support the core effort, and would love guidance there. Could use your help to share ideas and to align with possible WGSL futures. And to help enable webgpu library ecosystem. That's one of the most valuable things we can do collectively to improve adoption.
    * — questions —
    * BJ: During presentation showed multiple realistic issues for putting together reusable libraries, the one that really stuck out was bindings for libraries. What's not clear is how many of these things do you have proposals for or have working now and how many are raising the issue to say we will need to solve it. Did you solve the binding thing, I want the solution to the binding thing.
    * LM: More confident in first three, not the binding problem. Have discussed solutions but we're early on that. Don't have internal consensus on that yet.
    * SB: Have proposals for everything, but have multiple for some things.
    * JB: Apologies in advance, have a few meta things. Said you'd like the committees help, you'd be amazed at the things we disagree about. Things you think are a slam dunk go sideways. Last people you want advice from, as advice from one won't fly with anyone else. There isn't a consensus, we aren't a repository of wisdom for what will work.
    * LM: Get that, can't predict the future even if self aligned. There is wisdom here that we don't have that can help.
    * JB: Other thing, looks like the only way modern programming languages get designed:
        * 1- bully pulpit
        * 2- co-designed with a demanding application
    * JB: The idea would be for WESL to somehow arrange to be co-designed with a demanding user with someone like Bevy.
    * SB: We have multiple bevy folks in the group
    * JB: If you can say we've used this and it works ok that's a good selling point.
    * CW: Lot of ECMAScript was from community that bubbles up from the community. Problem does come up that the language version is slightly different from what it bubbled up from.
    * JB: Cautionary tail, async-await in ecmascript is a disaster. Everyone in JS calls async functions and forgets to await them. Easy to forget await in a call. Sometimes it works, if timing works out, function gets started but it's wrong. But if they had of designed it different so you have a promise or something else could have prevented a host of issues. That's the kind of mistake a smart committee put together with a lot of work.
    * LM: How did they not see
    * JB: Everyone trying it was on toy small things. Some things are easy to explain, look fine and are bad. Only see by exposing to something like Bevy.
    * LM: Could help us if we have versions worth trying to spread around to get more eyes to try them.
    * JB: This committee would be great for contacts and exposure but depend on you for feedback on usage.
    * LM: Also a good source of ideas.
    * DN: See templates and imports and think about programmers i'm exposed to who don't care how hard, but want to control the cache size. Is the audience super focused on performance out of compute, or rendering or are you aiming for ease of use for trade off. Allowing something to mixin may blowup register usage. How focused on GPU specific performance are you.
    * SB: We have both kinds of members. Some who ship if easy to use, and others who point out register increase.
    * LM: The ease of use camp needs help more then the other. Looking at the list from CW lots of things for squeezing performance but not a lot of ease of use.
    * LM: The first set of things we're looking at, like enabling libraries are just problems. Need to do something so the names don't conflict. Something for binding numbers.
    * DN: Dealing with large games there is a long asset pipeline before shader compiler and no human is looking at the last 5 stages, things get boiled away.
    * LM: Is it really both camp, or is this a wisdom question
    * DN: I think it's bimodal. Folks writing by hand and folks passing through spir-v chain. Use case i think is interesting for high perf folks is shipping an app don't know what device it will end up on and want small part of inner loop to use hardware feature and to encapsulate that so it doesn't make the driver crash without shipping other assets. Conditional if is super important and have seen this and would love to have WGSL solve it. Drives everything dow. Everything else can be transpiled except that.
    * LM: Right runtime vs build-time linking. Runtime linking is critical. Was trying to create an algorithms library and how do you tune the algorithm and need runtime linking. Important we can do some of this assembly in a compact footprint on the web. Typescript linker is 25k.
    * CW: Agree to what was said before, it's difficult to ask this group for a single opinion. Even given the same understanding of problems we come up with different opinions/solutions. Often try to reach perfection and end up with five different imperfect solutions and can't find a perfect one.
    * LM: Conditional compilation for example. You might want it?
    * CW: Was thinking about that. Can enshrine dead code elimination in the spec to some extent. Same with structure members etc. But will run into problems when you want to do late linking with some special feature like f16. It gets messy and dead code elimination may not be enough to solve everything. Also anything that goes in the spec, needs to be extremely well formalized. E.g. uniformity analysis
    * JS: Like a lot of what is here. Iffy on some of the less developed ideas. Would like to reverse the question - what can we do in WGSL to make your project easier? For example could you simplify a lot if we gave you namespaces? Which of these features would you like to upstream into WGSL?
    * LM: Question, what stage do you want to hear about our proposed solutions, e.g. we have several designs for imports. Moving forward with experiments, but when should we bring the ideas?
    * …
    * LM: No specific feature we're ready to propose to WGSL now. Some things we could discuss. If you see something and think for example, "we want that in WGSL, but this design is definitely not going to work".
    * JS: Similarly if we go and add namespaces don't want to conflict with your namespaces. [Often projects like this end up colliding with the spec eventually]
    * JS: Set up more frequent collaboration between the groups?
    * LM/SB: Yes
    * CW: Group not in good shape to do "TSGs" [Khronos term - technical subgroups of working groups] - have run into this with the desire to work on raytracing. All of the implementers that want to have influence in a TSG need to attend the TSG and it takes time away from their implementation work and core standardization work. Think a better option now is to come back in a few months and show what's working, make proposals to WGSL.
    * KG: Concrete needs from us are normal WGSL topics you can bring anytime. For collaborating on an external project, similar to native header (webgpu.h), should take place outside the group. Can take input from there into our WG, like we did earlier for naming.
    * LM: Mathis added a nice thing for conditions. Do you want to see it? Do you want to wait until we have tried it on real projects?
    * JB: You should ship everything all the time and not wait for our input.
    * LM: We aren't waiting, but if you'll read the spec, we'll send it to you tomorrow.
    * JB: We should not design namespaces until you have shipped namespaces and people like it.
    * JS: Would suggest that: make sure there is WGSL bug for the problem, even make a straw-person proposal. And you should attend WGSL meetings so that you have input on relevant things as they come up.
    * KR: Have you given any thought to programs that generate shaders at runtime? E.g. three.js snips together shaders at runtime.
    * LM: Can put in escape hatches for that, some linkers do this already. But those are invisible to the tools. Can't do anything in a language server if JavaScript is modifying the shader. Prefer to find standard constructs that cover for the cases that previously required custom shader generation.
    * GT: Want to second "just ship it". You mentioned error mapping - don't know what's missing, have examples that do this; would like to hear what is needed.
    * GT: Consider a syntax that is less likely to end up colliding with a native WGSL feature, e.g. if we ended up using `@link` later it would break yours. Use `@@link` or something 🙂
    * CW: Think there is also a culture thing. In C++ we do everything with #include. In Rust everything is using real modules.
    * GT: Slang also has a module system, and lots more transpiling than WESL has now. But given it's Nvidia they probably have more traction.
    * LM: Hoping to talk to Tess about this. Hoping WESL could be a backend for Slang.
    * DN: Slang project goes back a long way.
* Milestone 1 triage and feedback ([slides](https://docs.google.com/presentation/d/1mu140ml0m2VeRJQ4Hfvon0x1RDq5TOpZpJ_XYVY2Fx4)) (Corentin)
    * CW: Got 20 pages of feedback from folks. Apart from late feedback slide deck represents everything received. This is all the feedback, not filtered. There are some very common topics which are bolded.
    * CW: Implementation improvements are Chromium/Dawn (folks want Linux support). Lots of Matrix, rendering engines, ML frameworks  and other places. Some non-public sources that Chromium talks too. Goal is to socialize the feedback and we can discuss order for tomorrow.
    * CW: Folks want ML features. Didn't' call out subgroup but everyone wants subgroup matrices.
    * JB: Because you can build large from small multiples? \
DN: Yes and 50x memory bandwidth savings
    * CW: Folks want int8 or uint8 writes, often image processes. Similar to texel buffer proposal. Border colour of 0 for sampling would save bound checks. Folks want grownup game engines. Current ones are 15-10 years ago, we want to get to 5-10 years ago tech. Folks want bindless (most requested) requires a bunch of other things as well. Want multi-draw indirect for GPU driven. Can be an ok replacement for mesh and task shaders. 64-bit image atomics and storage buffer atomics are needed for Nanite and virtualized geometry. Some mention of primitive_id to get triangle. Folks pointed out benchmarks show having per-vertex data and provoking vertex has better perf.
    * CW: Folks really want ray and mesh shading but bindless is required for both of them. Lot of requests for data upload improvements. Immediate data, UMA support to avoid copies. Synchronous mapping to improve latency of ML pipelines and other processing.
    * JB: Thought we killed that as someone could have a 2s compute and it can't be preempted.
    * BJ: There are folks who do audio in native who do GPU to do it
    * CW: From bevy data uploads tend to have stutters. Even when time slicing because there is no good way to do this in WebGPU. Multiqueue for uploads was a suggestion.  D3D has something called upload heaps for accessing GPU ReBAR (PCIe Resizable BAR)
    * CW: Bindgroup creation, being slow or being annoying due to changing one thing and rebinding everything. This was a top 3 item. Both a perf issue and the model isn't super flexible. Have suggestions like bindgroup.setbinding. Will come up during bindless as the giant bindless group you dn't want to re-create all the time. Folks want fixed sized arrays of bindings, in WebGL but not WebGPU. Folks want to use render bundles but struggle to use in ways we wanted. Side by side WebXR can't change the matrix inside the render pass because the bundle don't inherit bindings from outside. So, no way to set data before bundle that parameterizes the bundle. Makes it difficult to use for this and impossible for gpu external texture as external textures expire at the end of the task.
    * CW: Folks want wGSL scale for bigger projects, namespaces, reflection, custom decorations on bindings that get reflected back. Replacing preprocessor usages. Recommended WGSL style, maybe a format tool?
    * CW: Some folks want assign to swizzle. More types in vertex shader output/fragment input so don't have to decompose. Enums, ternary operator.
    * JSP: They want short circuiting?
    * CW: Yes.
    * CW: Uniform buffer data layout, something like DataView, like byte address buffer. Think we could do better, we decay on HLSL could do everywhere. Printf for debugging.
        * DN: So popular in OpenCL.
        * CW: Don't know how we'll do it
        * JB: Storage buffer and atomics. Keep strings elsewhere and append into buffer an ID of the format and the values to go into it.
        * KG: Slow and costly.
        * CW: Costs a bind group
        * KG: Should only be added for static use. If you use it in the shader and if it's in a branch hit dynamic.
        * JM: Does it have to be printf or just strings as array?
        * JB: Strings never appear on GPU. Only thing there is ID.
        * DN: Design later.
    * CW: WebGPU debugger. Brendon Duncan wrote one, we prototyped one.It's hard.
    * CW: Interop with other apis like WebXR. Multiviews. WebNN want to interop to move buffers across so for missing operators can polyfill with webgpu. Have PR that landed last week. MLTensor PR. 1-copy interop with webgl. 
    * CW: Lots of hardware features. pixel local storage, MSAA sample counts. Dynamic states. 64 bit storage atomics. Async compute. `globallycoherent` in shaders, not sure what that is.
    * JSP: HLSL has a magic keyword. Thing in the spec talks about when you should use it.
    * AB: We already claim we are that. If you generate that in D3D backend, but we claim to be coherent.
    * CW: In addition to HW, there are a lot more features. Feature levels, or stuff in core. Multi threading. Dynamic resolution rendering, maybe mixed resolution targets. Engine scales size of render target based on how busy scene is to keep constant frame rate. Early fragment test to be guaranteed and have in pipeline or shader. Memory barrier control. Better frame pacing. Texture memory reuse
    * JSP: That's aliasing, one render target early and one later and want to re-use same memory for both because you know they don't overlap. Don't' have to pay cost for 2 render targets in memory.
    * CW: There was a proposal
    * JSP: How to make things safe, how to clear things out, etc.
    * KG: That kind of invalidation we do already.
    * JSP: Not free.
    * CW: Direct super resolution equivalent.
    * AB: How many of these are high end GPU features and how many for a large portion of users? Seems like a lot are latest and greatest, not necessarily what we want to focus on
    * CW: This is what everyone said, we won't do all of these things, need to prioritize.
    * CW: Viewport bigger then render target. Viewport is constrained due to metal docs which was a doc bug. and we never fixed it in webgpu.
    * CW: Formatless storage textures. Would be annoying in bindless if can only have rgba8unorm. Texture format tiers. Conservative depth, thing where you promise that lets the GPU do early depth test even though the frag depth builtin is used.
    * JB: Because you only make it closer not further
    * CW Or the opposite. Guarantee that if the early depth test fails then shader would have not made it past. Useful for LOD and things.
    * JB: Formatless textures, something dynamic?
    * CW: Understanding is early apis swizzing algorithm was in shader. To know how to access as storage buffer need to know format. Realized that was constraining so now read/writes go through texture unit. So, have  an f32 storage texture, here is a vec4f put it in there. Doesn't matter texture format.
    * JB: So, not a big deal for hardware to adapt at runtime so why constrain.
    * CW: Yup. Will advertise bindless tomorrow. Think it's interesting, unlocks a lot of things. Shows we need a number of improvements. Formatless storage textures, fixed array size bindings, ways to update bind groups. Something else. Need a few features to build up to bindless. Aliasing of storage things.
    * DN: Also things we're going to talk about htat aren't on this list like texel_buffers.
    * CW: They're in there, kinda sorta. Think what's important for tomorrow to discuss what goes in M1. Lots of editing and tiny things but figure out what kind of large features, or actual features we want to work on over the next year. Can't do all of these things but some things like subgroup matrices or bindless that we really should look at. What would make people happy, etc.
    * LM: Who advocates for the newbies. They don't show up on matrix with their corner cases? \
BJ: Some do
    * CW: Lots of folks in this group talk to those folks and care about usability of the API.  Lots of folks in this group argued for things that made it easier to use.
    * JSP: Think a debugger is big/huge. the half way point is to hook into renderdoc or pix or xcode. Just use the platform debugger, something functional.
    * BJ: You can hook into pix at least. Is there something about that that you're hoping to see more then you can do in pix. like don't want to debug the textures as the translated hlsl?
    * JSP: When tried pix didn't see translated HLSL just bytecode. Some low level thing not passing flag.
    * CW: Tomorrow start with texel buffers. Then see from there.
    * KN: Compat stuff as well.
    * CW: Maybe start with compat and east coast things.
* [Request EDT-friendly time; EET-friendly time even better but not critical - also request **not** 2-2:30pm Pacific]
* **Your update here!**


# Day 2 Notes



* CW: If you have more topics, please add to the agenda. Will discuss what we can or can't put in M1. Looser agenda today.
* Update and questions on WebXR/WebGPU interop ([slides](https://docs.google.com/presentation/d/1XEzrBrxJ_p9XTEUojibG2Ct2Ft1f7ul4ggzulCu-jpg/edit?usp=sharing))
    * Querying XRCompatible adapters
    * Patterns for efficiently rendering multiple views 
* `30 sec WebXR Intro` slide
    * BJ: Integration talked about alot in the webxr group. Both BJ and MW have been investigating. Wanted to give overview and look for feedback. API supported on basically any major VR/AR you can think of. Meta Quest, Apple Vision Pro. Android through cardboard or ARCore. OpenXR on Windows. Smaller headsets which have webxr browser. 
    * Imperative API bullet
        * BJ: JS driven. Nothing declarative. 2 parts. 1- matrices/poses delivered for headeset/controllers and 2- render targets to supply images to view device. Later part is reliant on WebGL. No mechanism to give webgpu render target.
    * `Performance` bullet
        * BJ: Performance is major concern. It's JS but trying to supply 70-90 frames per second. Effectively mobile hardware, rendering twice as much as normal. Tough for devs to use. Most uses lower impact then we'd like.
* `WebXR ❤ WebGPU` slide
    * BJ: There is an impression that WebGPU and WebXR are going to be “best buds”, since WebGPU seems hopeful for improving WebXR perf. by eliminating copies. TODO: incomplete?
* `Current Status` slide
    * BJ: We don’t have a spec. Yet, but we intend to once we prove out the thesis of the explainer via prototyping.
* `Look! Triangles! And they're not upside down! Wheee!` slide
    * BJ: Triangles aren’t upside-down! This was a big pain point during development. Works on Windows and OpenXR on Android.
* `API Initialization` slide
    * BJ: Wanted to show a basic idea of how init. is done. When you start a WebXR session, you request features, and if they can’t be given, then init. Fails.
    * BJ: You would get your WebGPU adapter mostly as normal, but we’ll likely want a flag to ensure that the WebGPU impl. Is compatible with the WebXR device/impl. Too.
    * BJ: We associate these two APIs with an `XRGPUBinding`.
* `WebGPU renders to layers` slide
    * &lt;slide contents>
*  `Layer Creation` slide
    * BJ: You get the layers [from the previous slide] with the `createProjectionLayer` method.
* `Frame Loop` slide
    * &lt;slide content>
    * BJ: Generally, 1 view (rendering surface) for mobile, and 2 for headsets (1 per eye). More is rare.
* `View Rendering` slide
    * &lt;slide content>
    * BJ: Might add a motion vector texture as input to the XR compositor later.
* `View Rendering, Pt2` slide
    * &lt;slide content>
    * BJ: Need to set the viewport for each view that you’re rendering. Otherwise, you can just use WebGPU as normal [to render to view surfaces?].
* `WG Questions`
    * BJ: The Immersive Web Working Group has some open questions I’d like to pass to this group.
    * CW: Does the user get a choice with the device they use? If so, how does WebXR make sure the choice is consistent?
        * BJ: We try to make sure that there’s a single device that makes the most sense. In the case where you have, say, 2 headsets plugged in, (1) OpenXR doesn’t support that (I think) and (2) other functionality outside of WebXR would likely be used to force the selection of a single device by the user.
    * `Need to add `xrCompatible` adapter option. Acceptable?`
        * JR: Is there a reason I shouldn’t always just set the `xrCompatible` option? Are there going to be ramifications for features, stability, compatibility etc.? 
            * BJ: We did observe a small start-up time perf. hit while prototyping, but I think we’ve solved that.
            * BJ: I don’t think there’s a reason to not do that, no. Should not have a major impact.
            * BJ: We did have a design where somebody could migrate a device to back WebXR later, but that caused some problems.	
                * JR: That sounds like it would be problematic under the hood. Getting a new adapter to transition to XR sounds better, in general.
        * CW: [lost]
        * JR: Any practical difference between power preference and WebXR compatibility?
            * BJ: If you’re on an SLI system, [lost]
            * BJ: If you’re on a laptop, we’d be picking the one that WebXR wants anyway.
            * KN: So, we’d basically expect power preference to not change anything.
            * BJ: Yep.
        * JB: Wait, shouldn’t devices that request XR compatibility fail if no XR device is found?
            * BJ: No. Adapters and devices should be able to ignore this.
        * BJ: This integration is really the only part of the spec. that needs to belong to the WebGPU working group. The rest can be with the Immersive Web WG.
        * CW: [lost]
        * CW: Want to make sure that WebKit has a chance to provide input here. Mike?
            * MW: We’re still experimenting with this, but so far we’re happy with this proposal.
    * [What are] `Ideal patterns for stereo rendering. Just using RenderBundles isn't enough.`
        * `RenderBundle problem` slide
            * BJ: We thought `RenderBundle`s would be a perfect fit for stereo rendering, since only a couple of matrix values need to differ between two views. However, because render bundles reset state, it’s really awkward to actually set up; you can’t provide any parameters to render bundles. We’ve found an approach that sort of works by resetting common locations of the matrices in uniform buffers, but this causes bubbles in the overall rendering pipeline.
            * JR: There is a bug open for multiple viewports [and render bundles?], but I’m not sure it’ll be enough [link?].
                * [lost]
                * BJ: Side-by-side indirect draws have some unfortunate side effects, I’d prefer using texture arrays.
                * CW: We might need additional hardware support for this?
                * CW: I’m partial to making `RenderBundle`s more flexible, since there are other fronts on which we’re experiencing pain with `RenderBundle`s for similar reasons.
                * BJ: In general, we would like to be to a point where have some sort of multi-view or vertex sample multiplication [?]. We don’t expect most low-power devices to have hardware support for this, though. We *do*, therefore, expect there to need to be a fallback rendering technique for this.
        * BJ: Not expecting a full answer from this group, but I did want to highlight this pain point and bring it to the group’s attention.
        * KR: You provided the WebGPU adapter [?] to the `XRGPUBinding`. Is it possible to recover the reference to the adapter from the binding if the user doesn’t hold onto it?
            * BJ: No, and it doesn’t seem like a real problem? We could solve this if it’s a pain point, though.
        * JR: Is there something in place to keep WebGL and WebGPU’s notions of projection isolated?
            * BJ: Yep! You can only use one of WebGL or WebGPU integration with a given XR renderer.
        * CW & BJ: We only really intend on providing minimal definition for the `xrCompatible` flag, and referring readers to the WebXR spec. for working details of WebGPU and WebXR integration.
* Texel buffers. [PR #4912](https://github.com/gpuweb/gpuweb/pull/4912) (James - EDT)  ([slides](https://docs.google.com/presentation/d/1XR6TbSuJpZ04UA6Q7cwllJNabSagpUE7lZC3dJAkx-0/edit#slide=id.g30fa23859c2_0_0))
    * `Summary` slide
        * JP: Proposing a new texture type that provides a view into a buffer.
        * &lt;slide content>
        * JP: All the languages we’re targeting have a similar feature. &lt;mapping in slide>
    * `Motivations` slide
        * &lt;slide content>
        * JP: Worked on a WebGPU backend in an image processing pipeline called Halide, and not having this feature was awful for that. (emulated with atomics and loops, and that is terrible and not even guaranteed to work due to lack of progress guarantees)
    * `WGSL` and `API` slides:
        * &lt;slide content>
        * JP: Won’t go in the exhaustive list of spec. changes I’d like to propose here [in this presentation? in this slide?].
        * JP: `texbuf` looks a lot like a `storage` texture.
    * `Mapping to native APIs` slide
        * &lt;slide content>
        * `Vulkan` bullet
            * JP: Same as with a storage texture.
        * `Metal` bullet
            * JP: We probably don’t need to care about devices that don’t support Metal 2.1.
    * CW: Does one have to specify `unorm`, `snorm`, etc. for the sampler of a texture buffer view? [lost]
    * BJ: Are these implicitly 1-dimensional?
        * JP: Yep!
    * JB: Do we ever want a sample from one of these? Is that meaningful?
        * JP: No.
    * `Formats` slide
        * JP: Adreno 500 devices and Mali devices might not support the formats noted as `Not requires in Vulkan`.
    * JR: Does this support `rgb10a2*` formats?
        * JP: Not yet. Doesn’t look like this can be part of base support.
    * `Open questions (1/2)` slide
        * JP: …`ReadWithoutFormat` is mostly not supported on desktop GPUs.
        * JP: Some machines don’t support the device-wide capability, but *do* support the format-specific features noted in the slide.
    * DN: Does the return value of the texture load depend on the format?
            * JP: You’d still have to know if it’s a `float` vs. a `snorm` or a `unorm`, yeah.
    * `Open questions (2/2)` slide
        * `Extension vs core?` bullet: prompt to group
            * GM: How does `compat` keep users from using this?
                * JR: OpenGL has an image store load that is equivalent to this feature, and it has its own corresponding feature. We’d likely set relevant limits to 0 to handle this?
                * (discussion): It is not core in ES 3.1. Probably can't guarantee or polyfill in Compat.
            * LK: What's the benefit of this over normal storage textures? Would you use the buffer interchangeably as either a buffer or a texel buffer?
            * CW/DN: Allows pointing at a buffer using the texture units. Provides things for free like format conversions, smaller addressing.
            * SW: Thanks Gregg for bringing up compat. James, if we are going to put this in core, please include a proposal for what compat needs to exclude.
            * JP: Will do. This may not land before compat, anyway.
            * CW: Not worth making an extension, because adding those extra formats would limit the reach of the extension?
            * RC: D3D team feedback.
                * The unorm/snorm distinction in D3D has not been required for a long time.
                * Also `globallycoherent` has significant performance implications, should be optional.
                * Stores have been accessible for a long time before loads. At that time would use a UAV + SRV to get both.
            * CW: …
            * CW: May talk about texture format tiers today.
            * DN/CW: This goes in with a format in the shader, and later we have a feature to relax that (along with storage textures).
        * `Should we add separate constructs for read-only vs read-write?` bullet
            * JS: More needed than the `read_write` vs `read_only`/`read` access mode demonstrated in the shader binding in the `WGSL` slide?
                * JP: API side too. In Vulkan you don't know whether to translate `GPUBufferUsage.TEXTURE_BUFFER` to read-only or read-write.
            * CW: This is similar to what we’re doing with storage textures, right? [DX12 impl. details missing]
            * JP: Pessimize if we specify usages for both "uniform texel buffer" and "storage texel buffer" in Vulkan?
            * CW: For buffers usages should only be used to select memory pools etc.
            * JP: Wonder why Vulkan had multiple bits then.
            * CW: /shrug
            * TT: Different types for …
            * CW: Don't remember, but think you don't say when you create the view, you don't specify the uniform vs storage. You specify in descriptor set layout but not when you create the texel &lt; will double check >
            * JP: Sounds like leaning towards no, keep as proposed which is nice and simple. We could potentially do formatless if we see read-only access mode but probably don't want to go there.
            * RC: Earlier said globally coherent, can you comment on how memory barriers/atomics in other languages work. Do they have the equivalent of globally coherent?
            * JP: SPIR-V has `Coherent`. Metal I am not sure.
            * DN: Think you have to put an image fence between reads and writes on the same texture.
            * JP: within an invocation we do a fence before any reads.
            * RC: Ok. Guess if you leave off `globallycoherent` then the browser has to put in the right memory barriers to make ti work/equivalent. Up to you to experiment.
            * JP: Yes.
        * `Bikeshedding: "texture buffer" vs "texel buffer?` bullet
            * JP: I lean towards `texture_buffer` for no particular reasons. Strong feelings?
            * JSP: I can do that. Textures are a thing, buffers are a thing. Lets not be confusing, most people use texel buffer.
            * CW: It's a buffer of texels. It's not a buffer of texture. Folks will see and think create gpu texture makes a texture.
            * LK: Think texel buffer clarifies
            * CW: Introduce new name so we don't link to concepts which are unrelated.
            * DN: I agree. It's a buffer filled with texel values.
            * KN: That's a lot of agreement.
            * BJ: On one of the slides it showd bind group, was that a new type?
            * JP: It's a new type create by the texel view. `createTexelBufferView` returns a `GPUTexelBufferView`
            * CW: The create texel buffer view only gives a format and size and offset, do we really need this or can we put it all in bind group and create on demand? For dawn it doesn't matter, avoids adding a new kind of gpu object to pass between buffer and bindgroup
            * JSP: This is how we do it everywhere else. do we have buffer views? or bind a GPUBuffer with offset+size
            * CW: Bind GPUBuffer with offset+size.
            * JSP: Ok i could go either way.
            * TT: Where would you put the format, it's in the layout?
            * CW: It's on the layout already. We could get it from the layout there. You need the format for formatless in the future
            * TT: IT would go in the resource
            * CW: It would be a dict with format offset size buffer
            * TT: If no perf implications seems fine
            * CW: You could cache, but it's probably cold code.
            * KN: So format is not in bind group layout?
            * CW: It will be in bindgroup layout, but in formatless future it will give you a sample/component type. We need a way to put it in the bindgroup to tell the texture units how to use it so will probably still be there.
            * JSP: The view seems fine.
            * CW: Ok. No strong opinion.
            * TT: Prefer it to be simple.
            * LK: If we keep the view, does it have to be texture buffer view or could it be texel view.
            * JSP: Buffer.createTexelView
            * LK: Yea
            * CW: Iterated on PR, but createTexelView sounds good
            * KN: Then object is ..
            * JSP: GPUBufferTexelView?
            * CW: We can bikeshed on the PR.
            * JP: To close
                * make core
                * rename to texel buffers
                * leave constructs as one
                * leave formats as optional
                * leave formatless for later (possibly bundle with texture format tier feature)
                * with those, are we comfortable with shape to prototype and build out in Chromium without too many changes?
                * CW: Sounds like that. Sounds pretty stable.
* Break until xx:40
* Bindless ([draft proposal](https://hackmd.io/PCwnjLyVSqmLfTRSqH0viA?view)) (Corentin)
    * CW: Lets talk about bindless. Proposal doc is linked. Will talk about doc, please interrupt. Probably wont' have all the answer as there is weirdness in the apis. May need whiteboard and we'll point a laptop at it and hopefully it works. A lot of folks want bindless. Main reason in webgpu fixed limit of textures you can put in bindgroup. Don't have arrays of bindings, but that's separate. You can pass 10 or 16 textures to a bindgroup. Prevents making shaders with access to all of scene data. Looking at non texture data you can have big storage buffer with offsets and it's annoying to write but no limitation that prevents you from having 1 giant buffer with all data and shader can access any data. With textures you can't do that which is limiting. Because it prevents doing gpu algorithms with global scene knowledge like visibility buffer (like nanite) and you want to shade it you get the triangle and instance id and you want access to the albedo texture and you can't because you only have 10. You want to ray trace and want the texture. You're 2d rendering with lots of images and want to address all the images without doing separate draws. Lots of other use cases.
    * CW: You can do things with atlasing with a few giant texture arrays and try to do that with bindless but awkward. Using more memory then needed due to reservation. It's inconvenient and most folks don't do that. Just for 2d usually not 3d. Why we didn't have ti before, hardware fixed function and registers to set texture params. Last 15 years hardware on desktop and over time mobile has become bindless. You don't bind to registers in hardware you have memory to describe texture and tell texture unit to sample texture and where is the memory address where the texture is. Varies a lot by hard ware. 
    * CW: What's important in bindless. One big thing, there are a number of concepts to keep in mind, not just hey i'm indexing. 1- residency. Super important because the OS the arbiter between multiple applications using GPU. So needs to knw what pieces of memory and app might use so if one goes over mem budget it can swap out for other apps. When it comes back it swaps back in the memory. In bindfull model the driver has perfect info on used resources. With bindless it doesn't, or it's too expensive to compute or inconvenient. Metal and d3d12 have residency so tells driver i'm going to use these resources so make sure in memory. Vulkan doesn't have equivalent. So we need to care for metal and d3d12. With these APIs it's not htat you have a resource in bindless that it has to be in memory. You can leave a descriptor that points at garbage data which was swapped out and that's fine.
    * CW: 2- in bindless you have giant arrays of resources. Say app reserves 4k resources. Don't want to fill all entries with data as that's wasteful. If they don't access it's fine. All of the apis support this sparse model. We have to decide what to do for webgpu. Not too difficult to handle.
    * CW: 3- Shaders index into arrays of resources, so index uniformity. Do all the invocations in subgroup access the same resource? Not sure at the HW level why it's important but in metal and d3d12 you can annotate "it might be non-uniform indexing" and driver emits scalization loop. in vulkan driver tells you if it can do it and you deal with it.
    * CW: 4- samplers are weird as they dont' have contents. So they tended to stay bindful. Intel hardware has some weirdness. Samplers are special and we need to determine if we want to make bindless
    * CW: 5- hw descriptors for various descriptors may not have the same size. Buffer maybe virtual address as 64bit int. Texture descriptor maybe bigger with memory pointer and size, format meta data. For this reason in a lot of apis there is a first version where you can only have array of homogeneous textures. All textures, all storage textures or all buffers. In all apis (need metal clarification) can say i want worse case layout for descriptors so you can reserve space for max size descriptors in memory containing descriptors and this way you can have first storage buffer, then storage texture and then sampleable texture. And you just have all resources in one array instead of one array per type of resource.
    * CW: That's the idea. Do you want to go through all 3 apis to see how it looks?
    * &lt;general yea>
    * CW: This is how we map to 3 the apis for apis. GL doesn't have bindless, GL does not GLES.
    * CW: In d3d12 has root signature. Also the name for the current state ste on the command encoder. Dual purpose for the root signature. When creating signature have number of slots ot contain either (i think 64) bits of contents. A single descriptor to a thing or point at an offset in a descriptor table. The root descriptor table. That's how we do bind groups. You don't manipulate bind groups you manipulate descriptor heaps. These are large allocations of descriptors and you can point at it in places. When you record commands you set a current descriptor heap (there is only 1) and all descriptors used have to be in that heap. To record descriptors you create a staging heap in the cpu and copy over the descriptors using a device level command into the shader visible descriptor heap which contains the bindgroup data
    * JB: Whole heap or subranges
    * CW: Subrages. Copy is done on CPU. As far as i know, can't enqueue a copy of descriptors on GPU. Which causes some issues later. What's interesting is the size of descriptors for all descriptors (or pessimist already) so maybe easier to do heterogenous bindless arrays. There are limits, like hardware can do bindless or not. Need resource binding tier 2 for bindless textures, tier 3 for everything. They expose things as bindless but support a tier where things aren't bindless. D3D is a bindless centric model
    * JB: So bindfull is a downgrade tier?
    * CW: Not really, api is designer for tier 3 but there is a lower tier which says you can only use this many resources. The way it works in HLSL is you declare an unsized array you bind to a register and the register has a 2 level indirection. In webgpu we have group(0) binding(0) in d3d12 you name with a register and then root signature says that register is at group/binding equivalent. You have array and address it. There is non-uniform resource keyword for scalarization loop. Later d3d added explicit support for heterogenous binding arrays where you don't' give type but have resource descriptor heap and say access index 42 and that's a texture (or something)
    * JB: What if it isn't'
    * CW: Deal with it
    * JSP: Undefined behavior
    * CW: These apis are not safe.
    * KR: Does UB, how bad is it, crash GPU?
    * CW: Your gpu crashes. Not blue screen but gpu crashes
    * JSP: Bad things
    * CW: No robustness equivalent. You're on your on
    * JSP: On amd if you use sampler heap the sampler has pointer and  if it's bad you blue screen
    * CW:In Metal, 2 binding models (kinda) 1- looks like d3d 11 where you have per stage per resource type arrays of resources where you set elements. All metal devices that webgpu target support argument buffers. Argument buffers are the equivalent of bind group but they're actual metal buffers that you write too. It is more explicit that things are normal memory then the vulkan descriptor set model. When you want to use them you set the argument buffer as the buffer bidning on the stage and use there.
    * JB: This is a buffer whos bytes are gpu addresses. It's raw gpu data.
    * CW: Yes.
    * CW: For d3d there is a residency api where you can say make resident. Evict doesn't necessarily evict but tells os it can evict. Residency is stateful. It either needs to be resident or not
    * TT: Per resource?
    * CW: Per heap
    * BJ: Can you query residency?
    * CW: Don't know. But it's a d3d suage error if you use a resource which is evictable. So you have to say make resident before using otherwise it's a usage error
    * BJ: By what mechanism does it stop you from making everything resident
    * CW: Please don't do that
    * JSP: Most games don't make resident and drivers try to handle with heuristics
    * CW: Metal residency is not stateful. On encoder say i'm going to use this or that resource or this heap. Or can have a residency set which say i'm going to use this entier resource. Which, I think, is just to make it faster on the api side. In metal there Argumentbuffer tier 2 on macos 13 that, from what I remember but can't find, it made argument buffer layout more transparent. So fixed stride per descriptor type which could allow heterogenous bindless. But would love input from Apple but wasn't able to figure out how it would work,but seems like it would.
    * MW: Bindless on argument buffer tier 2 is straight foward and only excludes a small number of devices from core spec.
    * CW: In metal these are buffers so with some argument buffer tier we might be able to do argument buffer to argument buffer copies on the GPU unlike d3d 12.
    * CW: Last is vulkan, 1.2 made core extension descriptor set dynamic indexing. Descriptor set is a bind group. Idea is that the last element of descriptor set can be an array of bindings. Feature has a lot of knobs. Knob to say have entry entries in last array part of bindgroup or not. Can have last element as unsigned array in bindgroup layout and then decide size when create bindgroup. Create per-descriptor type modifications after binding to the command encoder, basically saying bindgroup is buffer of memory containing descriptors instead of things when you call draw that set registers. Has feature soup. Seems like it's all enable or all disabled. Going to talk as if all enabled.
    * CW: To do bindless with this soup enabled is create descriptor set layout and say the last element is an unsized array of this type and create descriptor set on that layout and fill in values and say size of unsized array for allocation. Then fill in entries you want. Bind to command buffer and works as usual. Without another extension you can only update descriptor sets on CPU timeline. VkCommandWriteDescriptorSet(&lt;data>). In SpiR-V the bindings, instead of being pointers to an OpTypeImage or whatever is OpTypeRuntimeArray of OpTypeImage and this allows unbounded indexing due to being runtime array. Vulkan has EXT extension that is descriptor mutable type same issue as other types, when you set space it may differ per type. The descriptor mutable types tells driver that the unsized maybe one of these types soreserve worse case. Allows heterogenous bindless
    * JB: It's an untagged union, you just have to use it right
    * CW: Yes. The other version for vulkan is descriptor buffer which is close to metal argument buffers in idea. Ask driver what are layout constraints and ask resource about descipro then do what you want and make ti align and can do gpu gpu copies. In spirv for heterogenous binding what dxc does to do HLSL resource heap on spirv is it makes one binding per type but points at same descriptor set, so they alias.
    * JB: And the heaps are always the element size pessimist
    * CW: Yea, that's why the extension was created and mixed with spirv allow heterogenous. Descriptor buffer can after to be better overall.
    * AB: In SPIRV needs extra decorations for non-uniform indexing but they don't go in the same place. DXC puts it in HLSL and propagates on pointer, we have to propagate it from the access chain to the load/store.
    * CW: Something we'll need to talk about for fixed sized arrays of bindings. Buf overall in vulkan there are features for non-uniform indexing of bindless per descriptor type but we can emit scalarization loops if needed. So spectrum of choices from what i can tell, we push constraints on devs, only some of them, or we use uniformity analysis to make it appear to work everywhere.
    * AB: Note, the scalarization loop is only guaranteed to work when we have maximal reconvergence.
    * CW: &lt;sigh> always that one. Need to discuss more in detail. That's the api background, now the proposal for WebGPU.
    * CW: All the target APIs when they do bindiless have arrays of descriptors which are unsized from shader view but when create bind group tell size. Have constraint you can't modify when GPU touching as bad things happen. In base versions can only modify descriptors on the CPU. Also all apis are super unsafe, you do something wrong GPU crash, deal with it. Don't want for WebGPU. Difficulty for proposal is figuring out how to make ti safe without too much overhead and still being convenient.
    * CW: For the first choice, decide if we have a separate bind group array object that is only an array or add to end of GPU bind group like vulkan. Suggestion is unsized array on bind group. We have limited bind groups and most often you have scene global data in first bind group and at the end, it also contains all textures it might use. So natural to bundle with out scene global data
    * BJ: If dynamic at end of binding does that imply (assuming no heterogeneous) one bindgroup that is all bindless textures and another that is all bindless buffers
    * CW: Yes. People really care about bindless textures rest is future where they put everything in giant binding but slightly different architecture. Most folks want textures to begin
    * JB: You said at end of bind group, but you mean end of binding?
    * CW: No group
    * JB: So highest index?
    * CW: If you remove the bindless stuff you can see the fixed size proposal in there. When you create the entry you set an array size and can say it's `dynamic` and that has to have the highest binding index and all entries after it are "reserved" when you creat ethe GPU bind group you can say the dynamic size of the thing is 1k or 100 it's when you choose the size of the bindless array in the descriptor. But because there be only 1 dynamic array sized entry when we promote the setting to the root and not a specific entry. There are validation rules as expected.
    * CW: Next section has a lot more design choices, not as obvious what needs to happen. Also you can probably see that we need a proposal for updating bind groups even before bindless but bindless will inform that so that it works for bindless. Problem is we can only update bind group entries, or bindless descriptor heaps on the CPU and we want to make that safe for webgpu and also we have a constraint that the knowledge of what entries may or may not be used is on the GPU process but we're on the web process and we need to make sure we don't step on entries in use, etc. There are many things we could do there is the simple thing, spoiler, the proposal because I think we can make it fast is let applications say i'm taking this binding and putting it here, we deal with synchronization issues, probably by ophaning the bindgroup used before and creating a new one with updated data for future operations. Could have method on gpu bindgroup to get the index of an empty entry. Then communicate between web and gpu to get empty entry. 
    * CW: Alternative, application says i want ot add this texture and the api says where it put it
    * JSP To be clear, we're creating a giant 4k array for textures and we're filling it in and the app says all 4k are entry, i'm binding this here and this here and this hear. It's not like d3d 12 where you have global heap where you're putting things explicitly into bind groups
    * CW: Yes.
    * BJ: You mentioned a binding becoming empty, how does that happen? Only if resource is destroyed?
    * CW: Multiple ways if a resource is destroyed after done being used by gpu seems like in all the proposals there will be a bidirectional mapping between bind group and resource. Resource knows bind group it's in, generally 1 all zero. WHen resource is destroyed it tells bindless bind groups that i'm no longer in use, my entry is free to be used by someone else
    * JB: And this is when you destroy when in use by gpu and you have to wait for gpu to finish and then you can destroy and that's why it becomes empty
    * CW: Yes
    * BJ: Do we need a way to destroy without empty?
    * CW: Yes. For the first point we need a way for resources to become unresident or to tell a bindgroup that i want to free a slot when the gpu is done with it
    * JSP: To be clear. for first bullet point application is in charge of indices the second …
    * CW: For first it's in charge and we "pretend" we can update whenever, Webgpu implementation deals with synchronization. In second &lt;missed>. In 3rd bullet point application says i want to put somewhere and we say we put it in #4.
    * BJ: For native, i believe you go to resource, ask for a pointer and then placing that into the heap. so a method where you say i'm going to place on bind group and give me back index seems similar. You don't get back direct pointer but …
    * CW: In all apis you say i'm taking descriptor and putting at index I. In 12 you put it in staging descriptor heap and do copy. On vulkan we write the specific index. The 3rd bullet point inverts that where the implementation tells app what indices it gets for resources.
    * KN: Saying the gpu process would know where it could put something based on what's finished executing which means it's async.
    * CW: Not really, can imagine there is constant web/gpu process communication where we says im putting things here here here and gpu tells it when things are available
    * KN: So a bit pessimized but fine
    * CW: Could think buffer with atomics and stuff…
    * CW: 4th bullet is message from gpu when slots become available instead of being captured in browser goes to JS which does callback saying slot is free to use. Most ergonomic is the 1st alternative and think we should do that. Has some implementation difficulties and may have to copy big heaps but apps will be generally well behaved and do index +1.
    * BJ: Would we need controls for residency?
    * JSP: All have same residency constraints? \
CW: For residency we need to do it on d3d and metal. period. so always in backend. I think question was do we need a way to say bindless bind group this slot is no longer needed?
    * BJ: Right, if they do the copy, we can assume that anything in and bound is resident. So if i don't want resident just replace the binding?
    * CW: Could have a "set to null" but from webgpu application standpoint the only thing it achieves is usage scope validation. I'm going to be writing to this instead of reading. I'll remove it as render attachment and add back later. Otherwise, if the app wants to be extra nice to OS and say i don't need this texture if you want to swap out that's cool. Don't think devs will care about doing that.
    * BJ: Apparently native dont' care about doing that.
    * KN: Can we make the update look like a queue operation?
    * CW: No. It is a device operation on all apis and only on metal and high feature levels and vulkan on high feature level allow copying the escriptors on the gpu. Think we should expose it eventually
    * KN: Can we make ti look like it anyway so JS doesn't need to wait? In implementation GPU process say there needs to be a bindgroup update before we submit we wait and then submit the next work
    * CW: Maybe, and gives upgrade path when feature is available. More difficult to design as not immediately similar to bindgroup updates.
    * JB: So on vulkan you'd be waiting for the previous work to finish and then any subsequent work you'd wait for fence before started and on cpu say work finished, do write then signal fence? \
CW: I think it would be more similar between gpu and web process negotiation. Say no longer using, put in list of indices that are available when fences past. When fence pases put on list and when texture take first out of previous
    * JB: BUt if i want tot treat this as if happing on queue, subsequent commits can use state, so they have to block for gpu work to be done.
    * CW: If vulkan wants to make look like queue will have to do this or make a copy of the bindgroup that's not in use
    * JB: So remap under the covers from old to new
    * CW: You aren't' remapping references as there are no new references as you make ne commands using the new
    * JB: So you make an orphaned thing that you throw away
    * JSP :Yup
    * JB: Could have a few to cleanup
    * JSP: Yup. When you see pool in VK, pool is expensive, pool item is cheap. So creating more descriptor sets shouldn't be that bad.
    * CW: There are d3d12 difficulties as i assume we have 1 descriptor heap and need to suballocate. So if you have lots of bindgroups need multiple descriptor heaps and have to copy over. It's a mess but need to hide details.
    * RC: With regard to setting things to null, how does this work with lifetime management, You create buffers and textures and then lose all references will the references keep objects alive and then you will need some way to null out so we can cleanup without the dev knowing. Don't want to expose JS garbage heuristics
    * CW: Yes, another reason we want to unset entrines in bindless bind groups.
    * LM: What are the implications for libraries who in bindless world?
    * CW: WGSL i don't know. On the api side …… not very compostable as it's global state. Not ideal
    * LM: Thinking on wgsl side.
    * CW: Will talk about WGSL afterwards. Makes webgpu slightly less compostable. You have big bind group and you update things in it. so it's more stateful then very stateless concepts of apis. Don't think we can pretend it isn't' stateful.
    * BJ: Not that much more statefull then bindgroups. Still setting at slots.
    * CW: Can update bind groups
    * BJ: Right;
    * LK: If you update you can get rid of resources. If it doesn't have a ref in the bind group and you drop the last ref you can set to undefined?
    * CW: No, a hard but soft constraint for webgpu is that the gc should not be visible to applications. If you do this and use the bindgroup and see it's garbage collected ….
    * CW: Which means we need a .destroy on bindgroups so it can be freed when not needed.
    * LK: By the that logic can’t you just call destroy on the resources and then it’s fine for GC at that point; they objects have been destroyed.  Instead of setting the entry to undeff, you call .destroy()
    * CW: Yes. There are multiple ways to have users help us destroy memory. Setting value to undef also helps usage scope validation.  You may get unset() then set() and we can optimize that case.
    * CW: The proposal for setting entries is intended to make it simple for users. It’s not a queue operation; though that may be even easier ?.   Also this proposal is good for regular bind groups; avoids having to create whole new bindgroups when just updating one.  
    * BJ: For the update you sketched here, does this imply you have to reset all the bindings each time?
    * CW: No.
    * BJ: Does the sequence have holes in the them?
    * CW: The bindings have a binding number.
* CW: Next part is on the API side: dealing with residency and memory barriers. On Vulkan residencey is easy: everything is resident. On D3D12 we have to walk the resources. On Metal you have residency set on bindful / bind group and you’re fine.
* CW: The hard part is memory barriers 
    * CW: A lot of the CPU overhead of WebGPU is in these computations to track where barriers will be needed. I’ve written notes on how to make this fast in Dawn’s architecture. Connor had something about doing it fast in wgpu.
    * JB: Connor has reworked things at the data structure level; algorithms are similar.
    * CW: These implementation details arguing why the simple scheme should be ok are based on Dawn; I can’t vouch for how it would work for wgpu or WebKit.  We want to make common cases fast, and the non common cases fast enough. We (Dawn) have bidirectional mapping between resources and bindgroups they’re in.  tl;dr doable; we know resources are in a bindless thing, so they won’t change state. So most of the items won’t require a memory barrier; will need to recompute the decision when removing or adding bindings.
    * JSP: If I put a texture in bindless bindgroup, can I use it as a texture target? 
    * CW: will validate that out; you have to take it out of the group first.
    * JSP: (other question)
    * CW: That would be ok.
    * JSP: How about storage buffer: put  it in bindgroup, can I still write to it while it’s in the bindgroup?
    * CW: In first version you probably have to take it out of the group first?
    * BJ: It’s not the mere presence of a texture in the bindgroup; it’s that you couldn’t bind that bindgroup. The fact it’s in a bind grou[p intrinsically is not the problem.
    * CW: It’s a matter of both uses in the same usage scope. Lots of details to figure out. 
* CW: on the WGSL side, builds on a fixed-size-array of bindings. Start with that. Homogeneous arrays instead of `array` use `binding_array`. Then we have some getters, e.g. arrayLength. Then index at a specific index.
* JB:So the shader does a dynamic check to see if the entry can be used.
* CW: Yes. Implementation choice. On Metal you could load a binding entry and check if it’s a null pointer.  On other APIs add a metadata buffer containing availability bits for each entry. It’s a bit annoying, adds memory accesses but not a huge deal.  This is how you mark the entry as not available without having to overwrite the entry itself.  When the implementation removes a binding, it can move it from the state tracking, mark the bit as unavailable, but keep the descriptor still there, as a weak pointer like concept; then we can resuscitate it later very quickly. If you access an unavailable resource, I propose you get a default nil/null/zero-only resource. Nice to make it a zero’th entry in the array. (Always create such an entry at position 0). Or add it as a special entry before the bindless array.
* JP: Would you do a dummy one or any valid resource in the array?
* CW: How do you know which one is valid?
* JP: A dummy storage buffer might be huge.  (unsized array of array of 64kb)
* CW: Willing to waste 1 such buffer of say 64KB.
* CW: the implementation will make the dummy binding for you.
* BJ: Developers will rely on using that dummy space.
    * CW: For reading stuff I’m not worried. It’s a problem for writing; but you’re leaking info to yourself.
    * BJ: you want to spec it so all browsers behave the same.  For portability.
    * DN: WGSL already has cases (OOB) where variety of behaviours are allowed.
    * LK: Eventually you have to solve this for the heterogeneous case. You can’t fall back to a consistent thing.
    * CW: For nonuniform indexing of things, i thought we could emit a scalaraization loop; but Alan refuted that (requires maximal reconvergence). 
    * JB: Clarify scalarization loop.
    * CW: Use subgroup operations to bundle cases where indices are the same; then do that; repeat until exhausted.
    * JSP: Nonuniform is up in the air everywhere. 
    * CW: It’s not universal; it’s a feature bit in Vulkan.
    * JSP: Tobias was working on this bit in the spec. PTAL at recent updates.
    * CW: … walks through example from the proposal.
    * CW:  How could heterogeneous work.  On WGSL side have an untemplated `binding_array` and then methods to get particular kinds of bindings from them.  In the metadata array the residency info is not just a bit; it also says the kind of resource there; if there is a mismatch then you get the dummy/fallback return value.  
    * JB: It’s a discriminate union, but that’s stored separately.
    * CW: Yes.
    * JB: You scan the shader to see the covered set of resource types?
    * CW: Probably not; only about 20 kinds of things.  Dimensionalities and sampled-component-type. They’re small, allocated once, and their descriptors can be copied around quickly. They are tiny except potentially storage and uniform buffers.
    * JSP: When you talk about heterogeneous.  Everything across buffer, sampler, …
    * CW: Yes; samplers might be different.  
    * JSP : texture descriptors might be larger than buffer desctipros (e.g. on AMD).  Are you splitting those out? I’m Ok to make it pessimized.
    * CW: Will need to look at all the APIs. 
    * AB: Thinking about the fallbacks. If you have a bunch of holes, and access many of them at the same time you could end up racing, and hence cause undefined behaviour.
    * CW: This proposal requires a bunch of things.  And if we do this we really want to make them fortmatless to shrink the combinatorial explosion.  We need fixed size bindings, we need …, and WGSL improvements (e.g. racing like this). 
    * AB: Instead of using a 1x1 texture, can you make more fallbacks. 1-1.
    * CW: You can have 4000 group, making 4000 dummies would be bad.
    * Teo: …?
    * JB: I’m surprised heterogeneous is so valuable. Shader knows what type it wants it can select from a different global variable.
    * CW: Each unsized array uses a whole bindgroup.
    * JSP: And you don’t want to annoyingly count out how many of which kind of texture.  Just want to keep it in one big array.
    * CW: This is just one proposal; a bunch of things came up in discussion. Including aliasing.  Think I will make a PR.
    * CW: Want to look at the future to see what heterogeneous version would be; but first cut is homogeneous.  And before that start with fixed-size arrays that will flush out some common things like aliasing and dynamic indexing.
* — return in 1h15m () —
* Compat
* Reconsider “renderable float16, float32 textures” [#4417](https://github.com/gpuweb/gpuweb/issues/4417) based on new data from the [comment in #4721](https://github.com/gpuweb/gpuweb/issues/4721#issuecomment-2237577058)?
    * SW: We found more than expected # of devices that don’t support float rendering; some f32 but not f16; some support neither. Somewhere between 30 and 40% that could support compat are in this bucket.  Including Mali T720, PowerVRRogue 8..?? 8320.  As part of the investigation into vertex buffers and storage textures, GLES also doesn’t requires storage buffers …? Either. 
    * SW: Consider renderability first, and make it optional so we can include these devices in compat.
    * KN: How many of these devices are compat only and some could maybe hit core.
    * SW: If you trust the old vulkan drivers 1.0.3 and lower, some of them but do you trust them.
    * KN: do they support it in vulkan but not GL.
    * TT: Don’t think T720 supports Vulkan at all.
    * KN: If they would otherwise support compat, I think we should take this feature out of compat.
    * SW: I should verify that they can do compat otherwise. They are GLES 3.1.
    * CW: Are there other formats on these devices that could provide higher bit depth renderability?  If an engine wants to do things with things like HDR, or PBR things they usually need colour buffers with higher depth than just 8 bits per component.
    * KN: We don’t have any 16unorm formats. Those are not in webgpu.
    * KG: Just RGB10.
    * SW: It’s a lot easier to do HDR if you have a floating point format.
    * KG: I think for these devices that don’t support this, that games won’t consider HDR to be in budget. For mem bandwidth etc. Not just the tone mapping pass.
    * KN: I wonder how annoying it would be if we combined the two features into one.
    * SW: There are some device that support f32 only and not f16.
    * KN: i thought it was the other way around, but regardless, this is not a very big list. Videocore is unknown how well it works to begin with. It’s not really meant for graphics; it’s one of those embedded like chips. The powerVR one I’m not sure either.
    * SW: The big ones here is Mali and PowerVR.
    * KG: What’s the denominator on this.
    * SW: active android devices that could run compat otherwise.  
    * KG: If this is android devices, depending on the flavour of android some of these are not things that have browsers basically. Deployed android is not necessarily potential browser targets. More interested in those that have Chrome.
    * KN: I think the Intel ones are not significant because they are ChromeOS or unusual Intel Android devices.  The PowerVR ones and Mali ones do show up in phones.
    * CW: How about: we take it in, eventually we see if we cannot lift it.  Add the restriction to compat: don’t allow compat to render to float; get telemetry of how often its’ required. 
    * KG: To me the feature is renderability of floats. When you take that out of compat. It’s an important WebGL 2 thing. If you have compat the way you phrase it is you forbid float rendering in compat. Instead phrase it as take teh feature out of compat, but then add a feature of float renderability. That’s how you phrase it.
    * KR: There are two floating point renderability extensions for WebGL for 1 and 2. In webGL 2 they're not automatically included in the spec. For a long time iOS only had f16 renderability. Desktop had both.  People have been using it forever. It wasn’t a big deal to add them as extensions.
        * [https://registry.khronos.org/webgl/extensions/EXT_color_buffer_half_float/](https://registry.khronos.org/webgl/extensions/EXT_color_buffer_half_float/)
        * [https://registry.khronos.org/webgl/extensions/EXT_color_buffer_float/](https://registry.khronos.org/webgl/extensions/EXT_color_buffer_float/)
        * Both apply to WebGL 2 (there was an earlier version for WebGL 1, ignore it)
    * CW: Do you think you can rely on it in Android.?
    * KR: Take the feature out of compat core.  Add it as a feature you can add to compat; and make the feature included in core.
    * **CW: We’re agreed to not offer these in compat, add them as two optional features**
    * KG: If you view WebGPU compat as largely a rephrasing of WebGL functionality in WebGPU terms, then that’s the nice way to look at it.
    * CW: Sure + compute, etc.
    * SW: Does this affect any core content?  Do you have to opt into the feature?
    * KG: If you ask for core, you get it automatically. If you ask for compat then in the future you have to ask for the features explicitly. (But today you’ll get autoupgraded…)
    * KN: Happens for any feature we explicitly add for compat.
    * CW: Sounds like a good time to make a spec branch for compat. Also we need to request TAG feedback.
* Fuzzing lessons from wasmtime (Jim)
    * JB: At a recent conference there were folks from wasmtime and Ferrous (certifiable Rust for automotive). One thing that struck me is “[wasm-smith](https://github.com/bytecodealliance/wasm-tools/tree/main/crates/wasm-smith)”.  The fuzzers provide strings of random bits, ideally is coverage based so it sees what code it can reach, then mutates the bit string to reach new parts of the program.  A classic problem for fuzzers is, for example a block of data with a checksum. The fuzzer will almost always run into bad checksums which are then rejected by the program. The classic way to avoid this time wasted is to automatically generate the checksum yourself instead of having the fuzzer explore it.  Happens all the time with gatekeeping layers with lexical analysis and parsing, for example.  In Rust we can have a derive clause on types to automatically generate a value of that struct.  It’s still often not useful for us.  Interesting thing about wgslmith is that it creates *valid* WASM modules by construction.  So that module gets past those first gatekeeping layers.  You can choose what level of validity you can guarantee, based on what part of your codebase you want to explore.
    * JB: This was significant to me is in wgpu we are not adequately exploring our backends with fuzzing. We’re a member of OSSFuzz and get a few hours a week but I think we’re mostly wasting that time.
    * JB: I want to follow up to this; we want to be able to generate valid inputs to various stages of our stack. The final destination is to generate valid shader modules and then generate valid draw calls to exercise them.
    * JB: I think adversaries are likely doing this, so we should do this.
    * JB: Does Google fuzz DXC?
    * DN: We fuzz WGSL which then is valid but fires bad behaviour in DXC.
    * DN: DXC is not engineered to a standard to be used in hostile environments
    * JB: you don’t know what is sensitive or not until after the fact.
    * JB: IF there is a fuzzer that generated valid WGSL modules then we could all use it.
    * JB: to fellow implementors, I was shown that it could be taken much further than I had anticipated.
    * Antonio: One of the takes for Chrome security is we don’t trust any compiler, not just DXC.  For Chrome, we run it in the GPU process.
    * DN: We do work with researchers to fuzz as much as possible, and they make innovations in this space. There are also researchers we didn't talk to until after they submitted bugs, who are making advances.
    * JB: Have a fuzzing team who has a fuzzer producing valid API fuzz cases. But they are using a fixed set of WGSL programs.
    * JS: Would be great to share this between browsers. CTS?
    * CW: Fuzzer needs introspection into the "system under test" (e.g. code coverage)
    * KR: Some cases that couldn't be handled in browser. Example of infinite loop triggering bad compiler behavior in Mesa. Only able to fix that upstream. Another example with texture views and buffer overrun.
    * KR: Also important to have better sandboxing in the GPU process or whatever process runs compilers. Ours is good on Windows and weaker on Linux.
    * CW:  on the ANGLE team is looking into … we want to run the driver's compiler in another process, too.
* Support for arrays of textures [#822](https://github.com/gpuweb/gpuweb/issues/822) [#4940](https://github.com/gpuweb/gpuweb/pull/4940) (Corentin)
    * (AB requests after 1030a)
    * CW: add an array size in the bind group; it’s inline with binding numbers (not a separate index like in Vulkan). On the WGSL side, there’s a new array type `binding_array`&lt;type, number> (constant or override). And array indexing. The validation you would expect.
    * CW: What do we do with a GPU external texture.  It’s a composite of bindings. WGSL could do a structure of arrays; 
    * CW: Texel buffers could be supported.
    * CW: Question about uniformity of indexing. There’s some Vulkan HW which only allows constant indices. Worked around in ANGLE with a giant switch. I think we should support uniform indexing of every binding type.  Question is should we support nonuniform indexing.
    * JSP: No preference on nonuniform. Can wait for bindless if we have to.
    * JSP: 1. Do we support the thing with dynamic offsets into these arrays.
    * CW: No.
    * JSP: Can you explain the binding number tithing.  Can I put texture in binding0, then storage buffer starting at 10 to 20.
    * CW: Bindgropus can have holes. So that’s ok.
    * CW: Vulkan has a 3D binding model,  but here I suggest we flatten it, collapse the array entries as individual binding numbers.
    * JSP: SGTM.
    * BJ: Unlike bindless proposal these don’t have to be last in the bind group.
    * CW: Right.  And you can have multiple fixed size arrays in the bindgroup.
    * BJ: So basically this is syntactic sugar over having a bunch of individual bindings. Advantage is you get to index them in the shader.
    * CW: Yes.
    * AB: Looking at your GL hack again, GL couldn’t do storage buffers and things like that.  How you’d want to do this in SPIR-V is you’d want to return a pointer, and then you need varaiblePointers.  So maybe you have to specialize these functions (inlining etc.) to a great extent, which might get ugly. What’s allowed in base vulkan is arrays of textures.
    * CW: Let’s check gpuinfo for arrays of storage buffers; i think it doesn’t remove any devices.  For nonuniform indexing it doesn’t remove many devices.
    * AB: Each nonuniform indexing is its own bit per resource type.
    * JSP: If I say I have 10 textures in the array I have to bind all of them. 
    * CW: yes.
    * CW: We have to check availability in Vulkan: when they allow uniform indexing of these;  try nonuniform indexing later.
    * KR: There were different rules for 1 and 2. 2 was restricted.  Caused problems upgrading 1 to 2.
    * GT: Does the switch statement break derivatives?
    * KN: It works because you assume the index is uniform.
    * CW: Want to get agreement on direction.
    * JSP +1
    * JB: +1
    * TT: How prevalent is this feature. It’s not in Vulkan core.
    * CW: It’s in core in Vulkan.
    * KG: The only reason we punted on it before was because it was work.
    * JSP: you already have that WGSL language feature in wgpu.
    * **CW: ok, sounds like we land the proposal doc, then check Vulkan coverage of uniform indexing, and go with that for now.**
* Dealing with holes in the pipeline layout. [#2043](https://github.com/gpuweb/gpuweb/issues/2043) (Corentin)
    * CW: If you have a GPU pipeline layout, and you want to leave an empty bind group layout, you have to add a bindgroup with zero bindings. That’s annoying.  Proposal has three parts:
        * Change `GPUPipelineLayoutDescriptor.bindGroupLayouts` to be `required sequence&lt;GPUBindGroupLayout?>` (the change is the addition of the `?`)
        * Change the algorithm for createPipelineLayout to reify an empty BGL to "null" instead.
        * Change [Validate encoder bind groups](https://gpuweb.github.io/gpuweb/#abstract-opdef-validate-encoder-bind-groups) to clarify that it iterates over the non-null BGLs in the pipeline layout.
    * JSP +1
    * BJ +1
    * TT: +1
    * KG: Sure +1
    * CW: Next step is make a PR, CTS, prototype, ship
*  Allow getting adapter information from GPUDevice [#4810](https://github.com/gpuweb/gpuweb/issues/4810) (Intel)
    *  CW: The proposal is to put the adapter pointer on the device. The use case is middleware that has the device but needs info from the adapter.   
    * JB: There’s two possibilities. 1. Put an accessor that gives you the adapter. 2.
    * CW: the adapter are features, limits, and ability to get a device. So there’s more.
    * MW: Isn’t the change unnecessary because the framework can be given the adapter in the first place?
    * KG: We added parent references to all the objects?
    * BJ: No. 
    * CW: We added reflection for a few things.
    * BJ: Concern is an adapter can also spawn a new device. We don’t want patterns where folks want, in the case of a device loss that folks will try to create a new device from it.  
    * KG: But folks won’t use it because it won’t work.
    * KG: The info on the adapter have varying utility.  I don’t want in the future is to add a bunch of other stuff from the adapter.
    * CW: Example, when we add properties for subgroup size, then avoid duplication on the device.
    * LK: weird to have request device, but sure it won’t work.
    * GT: Should we split adapter into two interfaces and one of them is the static part, the other has the requesting API.
    * KG: this is suffering from nobody in the room caring enough to push it through.
    * KG: The solutions; none of them are perfect. Let’s pick one.
    * MW: We should let the framework do it and not contort the API to satisfy it.
    * KG: It’s not a good solution for the frameworks to do it.
    * MW: My understanding it’s passing another parameter along with the device.
    * KG: Generally you want to avoid passing parameters together when there is a secret important link between them.
    * BJ: (spells out a footgun scenario in middleware).
    * JB: No objection; my favourite is adding a .adapter  accessor on the 
    * GT: I’ve tried wrapping the API multiple times.  So many times I need a map of parent-child relations all the time. (e..g texture and device etc).  Balloons into a lot of bookkeeping.
    * BJ: Strongly +1 that. Ran into that last week.
    * KG: What you want is a weak parent pointer.  Years ago when we talked about the JS world this was frowned upon.
    * JB: Why does it weak?
    * KG: Dont want accidental liveness.
    * JB: We internally need it to be a strong need.
    * KG: It should be weak. WebGL doesn’t need it to be strong.
    * CW: in our webgpu.h we have strong links….Let’s focus on just the adapter.
    *  
    * KN: I kind of prefer not to include the whole adapter; concerning to look at the adapters features and limits not the devices features and limits.  Prefer to expose Adapter Info.  Also we have to support subgroup params.   
    * CW: Put the subgroup properties in the adapter info.
    * KN: Yes, I propose that, and expose the AdapterInfo on the device
    * *Round of +1’s. *


### M1 priority discussion



    * stars = top 5 priorities as estimated by us



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.jpg). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.jpg "image_tooltip")




* UMA buffer mapping [#2388](https://github.com/gpuweb/gpuweb/issues/2388) (Albin)
    * Interested: Mike Wyrzykowski
    * AB: Issue bigger than just memory bandwidth. Also serious performance impact on barriers. (no one was taking notes here). End up with all-graphics–to–all-graphics barrier. 15% regression just from barriers.
    * …
    * AB: If we know the buffer is not scheduled for use, but it's mappable (in vulkan terms, which is basically all buffers on UMA), and any gpu writes are visible to the host, we know we can skip the upload buffer and write directly. For all of these conditions to be true, there is a hidden fast path where developers have to keep one buffer per frame in flight.
    * CW: This is why it gets a star. Being in the browser makes it more complicated…
    * JS: Does UMA mean zero-copy? Reduced copies?
    * AB: 
    * CW: In WebGPU we have this idea we call "triply mapped buffers" which are visible to the GPU, the browser "GPU process", and the browser "content process" all at once. Possible but very experimental. On Windows may stress the memory system of the OS. On Vulkan not super clear how many devices support it and how well. So unfortunately can't guarantee mappable buffers get triply mapped.
    * JS: If there's one copy from content process to GPU process and zero-copy after that, is that OK?
    * AB: Zero copy ideal, but eliminating synchronization barriers really good. I've been profiling this mostly in native WebGPU so no browser processes involved.
    * CW: Overall, yes please: want to avoid the expensive memory barriers. Very interested in having your input, and as an oracle of whether it actually fixes the memory barriers.
    * MW: Many devices with low memory limits like 1.5GB for a web process, zero-copy helps a lot with this [reducing memory pressure].
    * KG: whiteboard:
        * UMA pool sizes:
            * None: Harsh Reality
            * Partial: uniforms, dynamic vert data
                * On discrete, have about 256MB memory you can DMA from CPU
            * Full: texture data uploads
                * But with ReBAR you can get large DMA regions.
    * JS: Addresses barrier issues?
    * CW: No. Think we need to collect all of the problems and try to find a solution. It won't be perfect.
    * CW: Know in the past we said we didn't want to expose on discrete because it's pessimizing. But maybe not true. OK if the "UMA" API is slower on discrete.
    * KG: Need data. If this "None" category is empty, then it becomes a very interesting thing to have some partial UMA with possible performance cliff past 256MB or whatever.
    * CW: Not sure how …
    * JS: But doesn't solve synchronization bubbles.
    * CW: Since our mappable buffers can't be used for anything the user has to enqueue a copy. … applications can have rolling buffers 
* textureGather
    * CW: Want to suggest we work around the textureGather stencil issue on Mac AMD and 
    * GT: textureGather int is crashing GPU process on a lot of linux intel tests.
    * KG: We need a full list of what's wrong before we can decide here.
    * CW: Block CR?
    * KG: Not sure right now
    * JB: You were saying it would just be errata if we found this after shipping.
    * CW: We can fix it after CR if we have to.
    * KG: OK going to CR with something that may give bad results. But not with something we have to remove because of crashes.
* CW: Let's call this meeting. No meeting next week.
    * Yay candidate recommendation!