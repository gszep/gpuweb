

# GPU Web 2021-02-22 VF2F Day 1

Chair: Corentin (WGSL = Dan Sinclair, Jeff Gilbert, David Neto)

Scribe: Ken, Corentin, Kai, Austin, Myles, (...others, add yourself)

Location: Google Meet


## [Agenda / Minutes for Day 2](https://docs.google.com/document/d/1JXZ9DhMTq-Y0_132zDX3-46omBKg3wSqgEJLsRZEUkg/edit#heading=h.mwy32mzdki7z)


## [Agenda / Minutes for Day 3](https://docs.google.com/document/d/1EaCJCXsD-uhbmpBnRqPiemmM9V965yRhoE6yboceO9A/edit#)


## Tentative agenda



*   Discussion of planning
*   Status updates:
    *   Apple
    *   Google
    *   Intel
    *   Microsoft
    *   Mozilla
    *   Editors/Specification
*   Babylon.js
*   Break + demos
    *   Wasm-shaders quick demo (2min) Pelle Johnsen - [https://wasm-shaders.pjohnsen.com/](https://wasm-shaders.pjohnsen.com/)
    *   Deno WebGPU demo (3min) Dzmitry Malyshau
*   WGSL items
*   Last hour of Tuesday:
    *   First public working draft

**Put your agenda item below:**



*    (quick) status updates (timeboxed to 30m?)
*   Short term API items:
    *   Require [SecureContext] for using WebGPU. [#1363](https://github.com/gpuweb/gpuweb/pull/1363)
    *   Require storeOp when resolveTarget is specified? [#1376](https://github.com/gpuweb/gpuweb/issues/1376)
    *   Zero initializing textures can not be done efficiently [#1422](https://github.com/gpuweb/gpuweb/issues/1422)
    *   Method of ensuring GPUShaderModules can contain MTLLibraries [#1064](https://github.com/gpuweb/gpuweb/issues/1064)
    *   Can WebGPU canvas alpha be configured? [#1425](https://github.com/gpuweb/gpuweb/issues/1425)
    *   Timestamps units:
        *   Query API: How to understand the execution time of the Dispatch command [#992](https://github.com/gpuweb/gpuweb/issues/992)
        *   Query API: What is the GPU timestamp unit on Metal? [#1325](https://github.com/gpuweb/gpuweb/issues/1325)
    *   Add GPURequestAdapterOptions.majorPerformanceCaveat [#1302](https://github.com/gpuweb/gpuweb/pull/1302)
    *   Remove GPUDevice.{adapter,features,limits} [#1309](https://github.com/gpuweb/gpuweb/pull/1309)
*   Short term WGSL items:
    *   Ptr syntax	
        *   Jeff’s proposal: [https://github.com/gpuweb/gpuweb/pull/1455](https://github.com/gpuweb/gpuweb/pull/1455) 
    *   [[WGSL] WGSL needs a plan for expansion · Issue #600 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/600) 
    *   [allow workgroup size components to be pipeline-overrideable constants · Issue #1442 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1442)
        *   PR: [https://github.com/gpuweb/gpuweb/pull/1444](https://github.com/gpuweb/gpuweb/pull/1444) 
    *   [https://github.com/gpuweb/gpuweb/issues/854](https://github.com/gpuweb/gpuweb/issues/854) 
    *   [vec4&lt;f32> is too hard to type on the keyboard · Issue #736 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/736)
    *   [The order of top-level declarations should be irrelevant · Issue #875 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/875) 
    *   PR approval: [https://github.com/gpuweb/gpuweb/pull/1452](https://github.com/gpuweb/gpuweb/pull/1452) remove 1d_array textures
*   Longer term API items:
    *   Web platform image interop
        *   Proposal for importing Web platform images in WebGPU [#1154](https://github.com/gpuweb/gpuweb/issues/1154)
        *   Investigation: Import VideoFrame from WebCodec to WebGPU [#1380](https://github.com/gpuweb/gpuweb/issues/1380)
        *   Proposal for a GPUVideoTexture object. [#1415](https://github.com/gpuweb/gpuweb/issues/1415)
    *   Multi-queue
        *   Strawman Multi-Queue Proposal [#1066](https://github.com/gpuweb/gpuweb/issues/1066)
        *   Multi-queue synchronization [#1169](https://github.com/gpuweb/gpuweb/pull/1169)
        *   Associate command encoding with a queue [#1278](https://github.com/gpuweb/gpuweb/pull/1278)


## Attendance



*   Apple
    *   Dean Jackson
    *   Myles C. Maxfield
    *   Robin Morisset
*   Google
    *   Alan Baker
    *   Austin Eng
    *   Arman Uguray
    *   Ben Clayton
    *   Brandon Jones
    *   Corentin Wallez
    *   Dan Sinclair
    *   David Neto
    *   James Price
    *   Kai Ninomiya
    *   Ken Russell
    *   Shrek Shao
    *   Ryan Harrisson
*   Intel
    *   Bryan Bernhart
    *   Yunchao He
*   Kings Distributed Systems
    *   Hamada Gasmallah
*   Microsoft
    *   Damyan Pepper
    *   Greg Roth
    *   Rafael Cintron
    *   Thomas Lucchini
    *   Sebastien Vanderberghe
*   Mozilla
    *   Dzmitry Malyshau
    *   Jeff Gilbert
*   Andrew Varga
*   Chas Boyd
*   Dominic Cerisano
*   Francois Daoust
*   Francois Guthmann
*   Matijs Toonen
*   Michael Shannon
*   Mehmet Oguz Derin
*   Pelle Johnsen
*   Timo de Kort
*   Evgeni Popov


## Status Updates



*   Apple
    *   MM: WebGL2 and GPU process consuming 100% of the the WebKit team’s GPU expertise recently
        *   CW: And WebGPU standardization :)
*   Google
    *   CW: we’ve been doing this working group for 4 (!) years now
    *   Not shipped yet. Eager to get it out.
    *   Chrome: aiming for Origin Trial mid this year.
    *   Would like to bring a sense of urgency to this group. Would be nice to finalize WebGPU this year. Get it out and useful. Right now it looks more like vaporware, which would be bad. Get first version out, and iterate on features more later.
    *   On Chrome side:
    *   Occlusion queries and timestamps
    *   STencil and depth sampling in shaders - led to bunch of spec changes
    *   Anisotropic texture filtering.
    *   Correct gl_Instance/VertexIndex in D3D12
    *   createReadyPipeline
    *   video interop
    *   Interop’ing other kinds of images from the web platform into WebGPU
    *   Trying to make Tint - our WGSL compiler - as production ready as possible. Converting everything we could find from GLSL to WGSL. Updated WebGPUSamples on Austin’s Github, CTS, etc.
    *   Lot of focus is on hardening impl for origin trial. Fuzzers, zero-init, etc.
    *   Also bringing up OpenGL ES backend. Working fairly well, still WIP. Not for WebGPU, but reduced feature set for future version.
    *   CTS work: it’s become its own test plan viewer. View with any browser today. Faster to load. Great quality of life feature - compiling TypeScript at development time using server. Just reload and it works.
    *   DS: On Tint side - most tests using WGSL - going through SPIRV. HLSL, MSL backends in progress. Reflecting data out of Tint to Dawn, for updating binding / set groups. Not passing user strings into backends, fixing security issues, etc. Have WGSL and SPIRV readers - WGSL one lagging the spec a bit. Removed uniform namespace. SPIRV reader - fairly complete. Most of hard lifting is there. Fuzzers are running & finding things. Test plan for WGSL validation. Going through spec, what do we need to test and how to test them. Looking forward to landing CTS tests.
    *   DS: Ben will be taking over TL of Tint. I’m leaving Google the middle of next week. Handing off spec editorship.
    *   MM: is the SPIRV path for a SPIR-V -> WGSL compilation pass?
        *   DS: yes, also for Dawn/Native.
*   Intel
    *   [Slides](https://docs.google.com/presentation/d/1EysIjT6Ipxp60A765uLLXW7a9sHRbx1scIJ-o26QcEQ/edit#) 
    *   YH: some bug fixes Intel did for D3D backend, small CTS changes, some other bigger changes like buffer/texture copies, video uploading (separate topic for this).
    *   YH: query API. Timestamp queries.
    *   YH: create render / compute pipeline async. Mainly for multithreading.
    *   YH: pipeline cache.
    *   YH: performance analysis on Babylon.js workloads, WebGPU vs. WebGL. WebGPU’s perf better, but still some issues like JS binding
    *   YH: 3D texture in Dawn implementation, CTS, and spec changes. And buffer sync in CTS
    *   YH: TF.js webgpu backend: optimized vec4, fp16 for workloads. Got some perf improvements.
    *   MM: how’d you implement fp16? no spec text for it.
    *   CW: Dawn/Tint has an extension which you need to enable. SPIRV path only for now. Chrome still accepts both SPIRV and WGSL right now. 
    *   MM: TF.js uses that (SPIRV) path then?
    *   CW: right now TF.js targeting WebGPU uses GLSL. That’s converted to SPIRV. Assumption is that it’ll be WGSL at some point. In general, assume that by the time we do a public beta of WebGPU, it won’t have web-visible SPIRV support.
*   Microsoft
    *   RC: providing input to spec; various bugs about how to do things with D3D; have a dev & Intel collaboration on SPIRV->DXIL backend for Mesa drivers. Hopefully better perf & compatibility for WebGPU, compared to using slow fxc compiler.
*   Mozilla
    *   [Slides](https://docs.google.com/presentation/d/1vQ9KTYxNBIIXf5evLylA79BKPHsgsfA-_W_dRPMgpNQ/edit?usp=sharing) (in [github](https://github.com/kvark/slides/blob/1d6c15714eaed1815df45757f82ee3717daf2afb/W3C/MozillaWebGPU_VirtualF2F_2021_February.pdf))
    *   DM: our WebGPU impl consists of: wgpu, Rust native lib implementing API; naga, shader infrastructure; Gecko, web IDL bindings & ipc transfers.
    *   DM: Gecko: full serialization rewrite. Much more scalable in growing API without breakage which we had before.
    *   DM: implicit bind group layouts. Last major piece: error handling. Can now detect, catch errors, route to users. Previously GPU process would just crash.
    *   DM: updating every few months to ToT WebGPU API. Still have wgpu-rs samples running in Gecko.
    *   DM: wgpu, community library. Sometimes updated before spec does something, sometimes after. New GPU memory allocator. Error model update. Limited support for timestamp, pipeline stats. Lot of validation.
    *   Don’t do a lot of hardening on shader side. Don’t yet do out-of-bounds access on vertices. Added a lot of validation in the meantime. Lot of focus on ecosystem & polish.
    *   Naga updates: WGSL input, SPIRV in/out, MSL out, GLSL in/out
        *   IR: standard library functions, updated texture sampling/storage, added queries
        *   New testing infrastructure
    *   MM: Naga goes in/out of GLSL and SPIRV? Will your WebGPU impl always be on Vulkan?
    *   DM: no. Our WebGPU impl, at wgpu level, works with either SPIRV or WGSL, like Dawn in Chromium. In browser, only works through WGSL. Produces SPIRV, MSL, HLSL, DXIL, GLSL.
    *   MM: on Mac, you’ll be running on top of Metal? Not going through Vk->Metal library?
    *   DM: the wgpu is running on top of gfx-rs to access platform native APIs. It’s vulkan-like. We do sort of map from vulkan-like API to Metal. So far it looks efficient for how WebGPU’s structured.
    *   MM: that accepts HLSL on Windows, etc.?
    *   DM: it accepts naga-ir.
    *   MM: so your IR is API?
    *   DM: The Naga IR is not exposed to users.
    *   DM: also: aggressively pursuing WGSL, have changed all of our testing over to it.
*   Editors / Specification:
    *   DM: [slides](https://docs.google.com/presentation/d/1tcSbHXWfdsQUsjBmgUPXcITLEVAEY6N0E4YPx-nDRrw/edit?usp=sharing) (on [github](https://github.com/kvark/slides/blob/1d6c15714eaed1815df45757f82ee3717daf2afb/W3C/WebGPU_SpecUpdate_VirtualF2F_2021_February.pdf))
    *   on spec side, 50 PRs merged (!) since October. Huge changes. Main areas:
        *   validation
        *   limits
        *   api
        *   functionality - described rendering algorithm to first few stages
        *   fixes / refactor
    *   Not merged:
        *   multi-queue
        *   reduction sampling
        *   …
    *   Main contributors:
        *   Brandon
        *   Corentin
        *   Yunchao
        *   Jasper
        *   Hao
        *   Others - thanks to you too! (Doesn’t include WGSL)
    *   Still TBD: other pipeline stages, validation, prose & algorithms, WGSL interfacing & relevant validation bits.
    *   Going fairly slowly. Lot of churn. Renamed a lot of things. Not growing the spec as much as changing it. Sign of maturity in some sense. But still missing a lot of things.
    *   CW: want to echo the point that we’re churning the spec and not extending it. Would be good to start extending / finalizing it.
    *   FD: anything blocking publication as First Public Working Draft? When the group wants to publish it.
    *   CW: FPWD could almost be tomorrow. Spec’s a bit dry, missing motivation section. In a sense, no later than we go for TAG review.
    *   MM: this group in general hasn’t been tracking progress in W3C stages. Instead mostly tracking our work by trying to finalize initial version of API that’ll break as little as possible, and impls working toward that. Vocabulary we’ve been using hasn’t been FPWD, but MVP, V0, V1, etc.
    *   CW: correct. Does this group think we should publish FPWD?
    *   KR: WebGL had breaking changes before v1.  Caused lots of confusion.
    *   JG: why publish working draft knowing we’ll make changes that will break anyone’s code based on the working draft?
    *   FD: good question. Thinking: we can automate the process of publishing drafts in the W3C. Publishing has IP implications. Call for exclusions, etc. Then set up automated publication process - when editors’ draft changes, gets published to TR space, dont’ have to worry about it any more until you move to Candidate Rec. Accessibility, etc.: other people not following your work, as we publish from W3C perspective - they’ll notice, provide feedback.
    *   JG: how does that signaling work?
    *   FD: new working draft, they won’t. FPWD, that’s the first time you’ll be in the TR space - you’re not in there yet. Not proposing the way you do your work, just want to automate the process.
    *   CW: if you don’t mind we’ll add this as a topic for the WebGPU API discussions tomorrow.
    *   MM: would like Dean to be here for that talk.
    *   CW: could make it last hour of tomorrow.
*   Babylon.js
    *   TL presents ([slides](https://1drv.ms/p/s!AiaI-WKvqjz1o-E1bpgQ2WFJErS-BQ?e=LVKjcO))
        *   “Full implementation finished in december”, part of daily build
        *   Part of Babylon.js Playground.
        *   Coming: compute shaders
        *   Requests:
            *   Speed up transfer of video textures
            *   Speed up transfer of 2D canvas textures
            *   Render bundle. E.g. set-stencil-reference
                *   Lack of set-stencil-reference, have to break the render bundle into two parts, then do the stencil part.
            *   Query graphic card capabilities _before_ creating a device. “To avoid WebGL for it”
                *   +1 from Francois Guthmann
        *   Feedback:
            *   Caching. Required to approach current WebGL performance; but is a lot of work to implement.
                *   Simplicity of WebGPU pushes work onto clients. Will affect adoption.
            *   Sebastien: more dynamic applications (like glTF viewer) have trouble because it’s hard to tell what the framework client is going to do.
            *   Sebastien: An engine designed around WebGL can turn on/off stencil fairly easily. But if were to rewrite now, then would make ‘stencil’ aspect baked into the material, and have the engine duplicate it to with-stencil and without-stencil. (?)
    *   DM: vertex buffer size? Is that what you’re asking?
        *   SV: can’t remember exactly which name changed.
        *   CW: maxVertexBuffers. Went from 16 to 8.
        *   DM: OK.
    *   DM: renderbundle restriction? Stencil? Do you mean stencil reference?
        *   SV: yes, some of those not encompassed by renderbundles. Just a few commands not available, forcing us to record, break recording, preventing us from reusing them. I know it’s not the main goal of render bundles, but when we render 3000 shapes, doing everything with renderbundles brought us an insane amount of perf. Renderwise, instead of setting all pipelines, bind groups, etc. - brings down number of calls from 3000 to e.g. 1. Makes us much more efficient despite this not being the main goal of render bundles.
    *   DM: what kind of WebGPU are you using? SPIRV? Recent API? etc.
        *   SV: most recent APIs. Still on SPIRV. Using shaderc to compile GLSL to SPIRV.
    *   MM: we’ve also gotten requests for more powerful renderbundles.
    *   MM: querying limits of devices - you didn’t use the word “limits” - our team’s interested in reducing fingerprinting by making similar devices look the same - “how many can i use” vs. “how much does the hardware expose”
        *   SV: totally agree with that.
        *   MM: compatible with your design?
        *   SV: yes. When we went back to 8, we didn’t know whether 16, 32, etc. were available. Or just rely on WebGL to give us the WebGL limits, then ask for WebGPU - a huge hack. If could query before creating device - would be great.
        *   CW: this is a current Chromium limitation. We don’t expose the limits and expose the defaults. It’s because we didn’t implement the adapter interface completely.
        *   SV: no worries. We just don’t want to have to make multiple queries raising the limits each time.
        *   MM: think you can describe what you’re using 2D canvas for?
        *   SV: 3 main use cases. 1) WebXR / WebVR, any kind of text. Don’t want to use text rendering tech. Currently in WebGL we render to 2D canvas, upload as texture. 2) Similar - when you have a game, put little info on top of player. Text rendering for annotations, e.g. for glTF model. Reference point to something in 3D. 3) Inking scenarios. 2D desynchronized canvases. When we want to record what they’re inking in WebRTC, want to composite into canvas. Have to upload the canvas every single frame.
    *   DM: impressive you can switch WebGL/WeBGPU with one line! Perf measurements?
        *   SV: still finalizing this. Esp caching mechanism. Lot of states involved in render pipelines, etc. How to keep engine flexible and still more performant than WebGL, without the impact of the cache?
        *   SV: beginning - no caching. Way less performant. Recreated everything every frame.
        *   SV: then only WebGL -> WebGPU comparison. Better in WebGPU than WebGL
        *   SV: now trying to do the same in Babylon. Can have numbers for the next sync. Would like to approach other web engines who are switching paradigms from WebGL to WebGPU. Loved conversations with Corentin, e.g. how to update pipeline reference rather than creating a new pipeline (not for V1 API, but…) Having something faster for caching, knowing there’s one internally. Huge amount of work / rewrite to support all of our use cases.
    *   CW: Suggest a one-off meeting to talk about the specific caching issue.
        *   
        *   
        *   
    *   
*   Demos:
    *   Pelle: [wasm-shaders](https://wasm-shaders.pjohnsen.com/). Basically compiled naga to WASM and created something like a shader playground with everything running on the client side. So you see everything happening immediately.
        *   MM: So you emscriptened Naga?
        *   Pelle: Actually Rust directly compiles to WASM. Uses spirv-tools to disassemble.
        *   Sebastien: Would this means that Naga could convert all our GLSL to WGSL.
        *   MM: So I can type WGSL and get anything but not vice-versa?
        *   Pelle: For now.
    *   DM: Deno is a secure JS/TS runtime a successor to node.js. [https://github.com/denoland/deno](https://github.com/denoland/deno)  A few folks have implemented WebGPU in Deno using wgpu. This is a boids example in typescript, you can run it and it generates (png images) with the output. Basically a complete implementation of WebGPU.
        *   DM: THe interesting thing is that it is all in Rust, there are no language barriers instead of the TS ->Rust barrier.
        *   DN: Actually running on the GPU?
        *   MM: Links with the native library?
        *   DM: Yes, should land any day now. The main problem is the linkage issues with SPIRV-Cross. Naga will get to a stage that doesn't require SPIRV-Cross.
        *   MM: Does this mean you're going to have a hard time modifying the API given you have clients?
        *   DM: It's fine, they can depend on published version, or the main branch, and they update on breakage.
*   
*   
*   


## WGSL


### Ptr syntax



*   Couple of proposals - one by David Dan’s ([https://github.com/gpuweb/gpuweb/issues/1334](https://github.com/gpuweb/gpuweb/issues/1334)) , one by Jeff ([https://github.com/gpuweb/gpuweb/pull/1455](https://github.com/gpuweb/gpuweb/pull/1455)) 
*   JG: overview: played with this, tried few different ways. Tried: having pointer load/store be explicit. Addresses one of the troubles I had with pointers - they feel magical, not clear when you’re using them. Act more like references. Why not call them “references”? This is the alternate approach. Deref is syntax for load/store. More explicit version of SPIRV pointer concept.
*   JG: caveat: little weird. Can only take pointers to storable variables. Not intermediaries, not samplers. Not quite pointers.
*   MM: no pointers to a const?
*   JG: correct. Those are intermediates. Ran into: conflict in my head because of name we have for variables. OpVariable from SPIRV. Have a lot of variables, but not “V” Variables. Pointers can only point to Variables. Maybe should rename those.
*   MM: didn’t see this PR till now. If I have a Variable, “var myles : i32;” - what’s the type?
*   JG: it’s a Variable of type i32. Things that determine type - the explicit type, and the thing that infers it. &lt;var, i32>.
*   DN: also need storage class.
*   JG: right.
*   JG: another way to think - move things from var side to ptr side. Would have something looking like pointer. Type is of “var&lt;private i32>”. Didn’t want to get too caught up in widespread change. This takes existing syntax & makes it explicit for deref.
*   MM: initial feedback: I like that there’s a natural path toward variable pointers in the future. Thing I don’t like - pointers look like C pointers but are less expressive than C pointers. Can’t just take a pointer to whatever you want. That’s misleading.
*   JG: it’s definitely differently leading. Do we want to look like something we’re similar to, but still different from?
*   MM: one way to possibly solve this - change the symbol.
*   JG: maybe.. Think this is future-proof, but maybe.
*   DN: this is explicit when deref happens - that’s useful and future-proof. Not hung up on what symbol (unary “*”) is used. const is a value, not something in storage - so can’t take address. Programmer could do this by putting the value in a variable. I like Jeff’s comment at the top, “this is more verbose” - gives programmer control.
*   BC: concern with people coming from GLSL / HLSL. Though this is nice and explicit, it will be jarring for people used to local variables.
*   MM: people coming from GLSL / HLSL don’t have pointers though. Won’t be typing “*”s.
*   JG: they won’t be used to this extra pointer work. They want to say “inout” on parameters.
*   BC: this is more for, say, local variables. Need to load/store on them? That’s where will be greatest friction.
*   JG: maybe most obvious in SPIRV. Alt to variable - SSA intermediate. We have middle road - have SSA intermediates, but either const or mutable. Thus variables, just not Variables. We expect most peoples’ code will not use Variables (“var”) unless you need to pass something out of function, or need something at shader boundary. Most shader code won’t use variables.
*   MM: if I want to store into my variable above, what would I write?
*   JG: you could write “myles = 3”.
*   MM: is that it?
*   JG: you could also get a pointer out of a var. “const pMyles : ptr&lt;i32> = myles;”. Now have pointer. Then “*pMyles = 3;”.
*   MM: no ampersand there. Intentional?
*   JG: yes, currently intentional. Would have been more spec changes. Questions about how variable declaration works. I talk about declstorage. Alternate proposal for replacing “var” with - you always access via pointers, but declare storage for it.
*   AB: I understand some of the concerns with e.g. function scope. I like this proposal for the storage and uniform. Maybe don’t mention … function scope variables. Just treat those as values. Not memory. Do think uniform storage should be treated specially.
*   JG: re: no ampersand - one way this could end up - if had “&” operator taking “var”, would evaluate to ptr-to-var. Intend to propose that, but not the case currently. Ptr-from-var is implicit conversion.
*   MM: Is this right?
    *   var myles : i32;
    *   myles = 7;
    *   const kyle : decl_storage&lt;private, i32>= myles;
    *   *kyle = 8;
*   DN: I didn’t realize that was permitted by the PR.
*   JG: not intentional. Let me fix it in the notes. Today this is the proposal:
    *   var myles : i32;
    *   myles = 7;
    *   const kyle : ptr&lt;private, i32>= myles;
    *   *kyle = 8;
*   (JG: Maybe instead we want:)
    *   const kyle : decl_storage&lt;private, i32>= &myles;
    *   *kyle = 8;
*   DN: now that it’s not so evident what’s going on...not entirely consistent, need to take another look. Maybe lot of people have gotten confused about what ptr means, and var isn’t a type in the spec. Myles’ question, what’s the type of a thing declared as a var is a key thing to get clear. Saying “it’s a var” - there’s no var type, only pointer types.
*   MM: follow-up: on third line, what if instead it were const, it were a var?
*   JG: pointers are not storable themselves, only operate on storable things. Maybe we should explicitly say you can’t have pointers-to-pointers today.
*   DM: main idea here - make pointers explicit. Future-proof, solves problem Dan found in the first place - want reference, so pass by pointer. Problem: we’re used to vars, we want vars. They don’t conflict. We can have vars like today, but can take pointer, pass to function. No conflict, no ergonomic cost. Have cake and eat it too.
*   BC: think that was a confusion on our part, that reads/writes required “*”.
*   MM: so coming from GLSL variables will still work the same? But inout will require * and &?
*   DM: we already merged the PR that removed inout. Just inputs and return values.
*   MM: talking about people used to GLSL. Doesn’t GLSL say, inout is as-if copy-in and copy-out? With this proposal, the programmer gets control over whether to copy?
*   AB: GLSL made it worse. Atomics, for example, have to be references, but still inout.
*   DN: and for the non-atomic case, you can’t pass the same variable multiple times to inout. Can’t alias multiple inouts. Yes, programmer has more control in this case because you pass reference to it. Can avoid copy.
*   MM: analysis sounds complicated.
*   DN: good point. Aliases - not to be allowed. Logical SPIRV has rule, pointers assumed not to be aliasing (unless you decorate them). In my PR, I define originating variable for a pointer. Then can tell which variable you came from. If rule - at function boundary, you (... garbled) then you know what it came from.
*   MM: in this proposal there’s no aliasing.
*   DM: wouldn’t you need aliasing rules to produce SPIRV?
*   DN: yes, need to ensure you’re not feeding in aliased pointer. May surface as a WGSL rule.
*   AB: GLSL also really restrictive about what can be pointers.
*   DN: copy-in/copy-out for function local storage. Doesn’t matter at that point. In MSL also can’t take address of component of vector. Compiler lets you do it with one syntax but not another.
*   GR: Is that documented?
*   DN: MSL isn’t adequately documented that way. Compiler errors out with &vector.x, but with C-style casts can get it.
*   MM: in MSL spec, it says, don’t do the C-style cast.
*   AB: I bet a lot of people do it and it works.
*   MM: in MSL it’s easy to write non-portable code. It’s C-based.
*   MM: I think this (Jeff’s proposal) makes sense. Wish had little more time. Proceeding in this direction would be fine for now. Slight preference to requiring “&”. Think type system is internally consistent. Don’t think I’ll enjoy alias-finding code. Timid thumbs up.
*   DN: I’m more confused about what this proposal’s getting at before start of the meeting. Need to re-look.
*   JG: had a direction I was going in with it. More I want to do. Just making load/store explicit here.
*   DN: in my mind, var is a pointer.
*   JG: Alan had a good question - are all vars pointers? I changed grammar for assignment statement. Can be either var or pointer. Always use a *. Now can only do through pointers. vars not something you can access directly. That would be a further milestone. Would that be more attractive?
*   MM: think being able to write direct assignments to vars (without *s)  is a requirement in the shading language.
*   JG: I think it would be nice to always have to store through pointers. Conflicting design requirements. Get pointer to storage in one line. But would look like memory leak. Similar to alloca().
*   KN: not completely clear on dir of proposal, but couple of things I don’t like: 1) don’t want variables to be pointers. Want them to represent storage. Pointers should be separate feature. vars give you syntactic sugar over pointers, + alloca. 2) i don’t think we should have implicit loads/stores from pointers ever. But don’t think vars should be pointers - think they should be special. “var myles” - myles is not a pointer type, but identifier representing variable. loads/stores happen automatically, but can also take pointer to it.
*   AB: for certain storage classes, is very different on the GPU. Treating it not just like any variable makes you think about what you’re doing.
*   JG: SPIRV surfaces this with OpLoad/OpStore.
*   AB: value in local variables being value-based. But the way you set up descriptor tables, etc. - paying very different price.
*   KN: makes sense. I’m concerned about local / private.
*   AB: if we use different terminology, might make it clearer.
*   MM: when you use HLSL / GLSL to store through a descriptor - you just use an “=” sign.
*   JG: some amount of overhead you have to think about when doing this.
*   KN: that’s what I’m getting at - variable with a value vs. const.
*   JG: my instinct here was to follow SPIRV. We have different design constraints though.
*   DN: 1) spirv’s not special - llvm is the same way. variables are allocations, type is natively a pointer. same discussion happens there. 2) what’s the type of variable, how do i manipulate? multi-level structure - expression you break down with “.” - have to describe how to step into a variable. That duality showed up in the PR I posted for the other branch of this discussion. Need clear rules about when you step in to things, when you do dereference, are you operating in pointer’s vs. value’s domain, etc.
*   MM: in C, that’s rvalue/lvalue. In this proposal there’s the same thing no?
*   JG: it’s - what is the type of s.foo. Needs some sort of type to know how to deal with it. If s is a variable, is s.foo a pointer? a sub-variable (need new sub-varlable type)? In SPIRV you move to pointer land, s.foo is just a pointer, then you dereference.
*   MM: or - the type of s.foo is &lt;int, lvalue>. Compiler can turn that into a pointer, can do it for any lvalue. But type isn’t pointer.
*   DN: that’s just a different kind of pointer.
*   DM: want to see “var” as sugar for pointer dereference. If we decide what “.” does for pointers, then “.” can have higher priority than dereference.
*   MM: re: type of s.foo above - exposing lvalues to a language explicitly is impossible, but exposing pointers explicitly to a language is possible. Pointer of int - programmer has to spell out “pointer of int” in their program. But if you think of it as &lt;lvalue, int>, they wouldn’t type out “lvalue”.
*   JG: it just has to infect everything else. Special handing for propagating rvalues/lvalues, different things you can do on LHS / RHS. Lot of complexity to bring in.
*   Discussion about whether Jeff’s proposal is the one he wants
*   JG: I think this proposal is good and is also along the path toward another proposal. Didn’t want to change how the whole world works just for this proposal. If they want the entire quanum leap all at once, we can do that, but don’t want to write the whole spec all at once. Could write examples, see if people like how it would work.
*   DN: I’d like that. Would like to see what can no longer be done. Purely additive thing caused some confusion to me.
*   DN: Myles if you could write down technically vs. philosophically correct thing above I’d appreciate it.
*   MM: I think JG’s direction of explicit loads/stores for everything isn’t a direction we want to go. Lot of typing.
*   JG: this is part of the confusion. I’m saying Variable, you mean variable. None of the math you do is using Variables. Only when you need a pointer. No reason to use Variable if you don’t need pointer. And not coming to you from external value.
*   MM: as soon as you have a loop, will want to modify variable in loop body.
*   AB: that’s local storage. Different from other things, and the “v” variable. Write loop like you normally do, without hiding explicitness in other parts.
*   MM: would like to see the details.
*   CW: how about we take some homework for tomorrow, come back with more concrete proposals.
*   MM: is tomorrow too soon?
*   JG: not for me.
*   DN: if Jeff puts up a loop example, things that will work / not work, I can look through it.


### PR approval: [https://github.com/gpuweb/gpuweb/pull/1452](https://github.com/gpuweb/gpuweb/pull/1452) remove 1d_array textures



*   Approved, great!


### [vec4&lt;f32> is too hard to type on the keyboard · Issue #736 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/736)



*   DS: thought we’d punted this and the one before to MVP.
*   MM: prioritization spreadsheet - MVP or soon after. Small, isomorphic to things in language, things people will type all the time in the language.
*   JG: i type this all the time. has a chance to get better when we have type inference. For now, this is poor. Syntactic sugar? fvec4?
*   MM: HLSL does both.
*   DS: can also use type alias yourself.
*   MM: think it’s poor design to expect all authors to write something to make the language not terrible.
*   BJ: using type aliasing also reduces copy/pasteability between shaders.
*   JG: sympathetic to having stdlib typedefs - fvec4. I really like having the full names vec4&lt;f32>.
*   GR: that’s what HLSL does. HLSL model is why we don’t have to decide this now.
*   JG: any reason to not do this now?
*   DS: we have vec4, float4, more bit widths coming soon. bfloat16, ML type floats. Big can of worms.
*   MM: think it’s OK for these to not be exhaustive.
*   JG: +1
*   AB: if we’ll have type inference, why do we have to add this now?
*   JG: even with TI I’d like to write fvec4 rather than vec4&lt;f32>.
*   JG: probably ⅓ the time.
*   CB: (I’m at AMD now) why not stick with what existing language like CUDA, SYCL, HLSL do? Follow C++ standard, float32_t, like c++-ish languages?
*   MM: fine with me
*   JG: this is where we’re ready to see proposals.
*   MM: I can make a proposal. Or three, and pick one.
*   (DN: GLSL has an extension for many  more builtin scalar+vector types [https://github.com/KhronosGroup/GLSL/blob/master/extensions/ext/GL_EXT_shader_explicit_arithmetic_types.txt](https://github.com/KhronosGroup/GLSL/blob/master/extensions/ext/GL_EXT_shader_explicit_arithmetic_types.txt))


### [allow workgroup size components to be pipeline-overrideable constants · Issue #1442 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1442)



*   _Discussion about how/whether this constrains when you generate machine code / compile the HLSL._
*   MM: What do HLSL shops do to tune the workgroup size
*   AB: Seen uber shaders that are hyper optimized at pipeline creation time.
*   CW: Seen ML workloads that specialize workgroups size via spec constant.
*   MM: We will have IR before and after function constant values are specified.  Will run some LLVM passes after function constant values are specified.
*   &lt;missed a lot of discussions>
*   MM: The discussion is not about whether to have spec constant, but just whether to have them for workgroup sizes.
*   JG: Anyone doesn't want to use them for WG size?
*   MM: That would put a restriction on when HLSL is compiled.
*   JG: Without this people would be recompiling HLSL anyway.
*   AB: something i wanted in Vk - extra API call to specialize shader module prior to pipeline creation. Only parse WGSL shader once, split up after that - more potential use cases. Lot of spec constants you could partially specialize shader module on.
*   JG: having shader module creation.specialize() would be useful.
*   AB: not wanting to duplicate shader module that many times. May not know all desired specializations until later.
*   MM: in Metal this is possible. We have an intermediate stage, takes MtlLibrary & spec constants, and produces Mtl library.
*   AB: it’s easy to do this in Vk, it just doesn’t have an API for it.
*   RC: when D3D team brought up spec constants, IHVs said - they’d have to redo a lot of the stuff where they reapply spec constants themselves, Doesn’t help things a lot. Obviously they did it - it’s in Vk - but doessn’t help as much as you’d think. I’m inclined toward more esoteric ones, let web developers do this themselves manually. Esp if requires lots of copying ourselves.
*   CW: isn’t that OK if we save lots of parsing / validation for the end users?
*   RC: if we’ll be duplicating a lot of work then we should leave it up to web devs. But if one time only application, then do it ourselves.
*   JG: if this turns into shader recompilation and that’s what user would be doing themselves in worst case, and in best case we’d do things faster, I’m inclined to take the API improvement.
*   RC: how often would we apply these constants?
*   JG: once, during pipeline creation.
*   DM: if we go with spec constants we save a lot of revalidation.
*   MM: in our experience that revalidation’s a lot smaller than what’s inside the platform APIs.
*   DN: have to revalidate sizes. Negative sizes, etc., and anything derived from that.
*   RC: loop unrolling, getting rid of functions / statements, etc.
*   KN: I’ve wanted to see for a long time - multi-step specialization of shader modules - and to also do linking. Nice to have a step where we can put those things.
*   JG: like shader library?
*   KN: yes. We don’t have SPIRV style linking at pipeline creation right now. Can only pass one shader module. One reason I’d like to see linking: from years ago when arguing for SPIRV, we argued in favor of SPIRV and against a preprocessor because a lot of string manipulation people do on shaders are because of lack of linking / specialization. Not because they want to do it that way. I’d like us to get linking in eventually - not necessarily for MVP - and spec should go along with it.
*   MM: in issue 1064, compilation inside createShaderModule, one proposal (#2) was similar to what you’re describing. Intermediate obj between shader modules and pipeline creation.
    *   [https://github.com/gpuweb/gpuweb/issues/1064#issuecomment-714308897](https://github.com/gpuweb/gpuweb/issues/1064#issuecomment-714308897) 
    *   “Entry point”
*   KN: I’d prefer something more flexible. Several shader modules & spec constants, get shader module out of it, and keep iterating.
*   DN: Kai is that gluing together translation units?
*   KN: yes.
*   CW: the question here is should workgroup size be spec-constantable. Backward compatible to add it later so we should add it later.
*   MM: think that’s fine to defer this.
*   JG: move to post-MVP.
*   CW: for this one specifically we should resolve #1064 before topics like this one. GPU shared module = MtlLibrary.
*   
*   
*   
*   
*   


### [[WGSL] WGSL needs a plan for expansion · Issue #600 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/600) 



*   DN: not much motion on this. Concretely: strawman at bottom.
    *   [https://github.com/gpuweb/gpuweb/issues/600#issuecomment-783575662](https://github.com/gpuweb/gpuweb/issues/600#issuecomment-783575662) 
*   DN: want module to declare any extension it needs before it starts using it. This adds an enable statement, module scope, I’m using a new feature. Sort of like attribute name, parameters in parens. Can always parse enable statement, even if don’t know what extension is. Can be redundant. You could write snippets of code declaring their use of a feature. Don’t have GLSL annoyance of first line requirement.
*   CW: feature bundles.
*   DN: if in 5 years f16 and other features are super common, name a bundle, have a short string for it. “WGSL 2” shader. Common sequences can be shortened. Some notion of namespacing too.
*   RM: reasonable. Q: when you say “enable foo, bar” - is that the same as “enable foo” and “enable bar”?
*   DN: no. first token’s an identifier, everything else are operands for what it wants. If foo and bar are different features, need 2 diff lines.
*   RM: not sure I like the syntax in that case. Not obvious at all to someone not knowing the language. Not sure what would be better one. General approach seems fine, just the syntax - first element is “special”.
*   DS: I recommend not using the comma. Used to separate attributes. Using brackets would clear it up.
*   MM: seems a hierarchy? Tree, your enable line identifies a leaf in the tree? Sequence of args in enable function are from the root to a leaf? Features / extensions / etc. are just a flat set. Why not just use a flat set of tokens?
*   AB: it’s just two levels. namespace, and set of featurs.
*   MM: Q still stands. Why not just flat list?
*   KN: DN addressed this. So you have a way to express things not in the standard.
*   MM: anti-pattern on the web. enable Chromium thingy, thumbs down.
*   KN: it’s similar to --webkit- .
*   MM: we don’t do those anymore.
*   KN: don’t want to say we shouldn’t have any way to do this - just reserve things starting with underscores etc.
*   MM: if content uses Chromium specific things…
*   KN: we wouldn’t ship those things to stable.
*   DN: mechanism vs. policy.
*   KN: people might want to try turning on a feature before it ships.
*   JG: could be CHROMIUM_f16. What we do for WebGL.
*   KN: i’m not specifically in favor of the comma.
*   JG: if it’s all for WGSL then we shouldn’t need WGSL_ prefix.
*   DN: how do you do f16 today? If not in MVP, how would you declare you use it?
*   JG: declare it anyway. enable f16_prototype.
*   DN: you’re saying no namespaces.
*   CW: idea here - there’s only prototype stuff. Things in namespace = things in WGSL spec in W3C. People who want to use WebGPU outside browser context, will people take things from their namespace here?
*   JG: what if people make an f8 extension & then want to bring it to the web. We’d say, you did something weird outside the browser, we’ll work as though your spec didn’t exist.
*   MM: people can write their own browsers & do their own CSS things...we ignore those when spec’ing CSS.
*   JG: don’t say WGSL_ , because it’s always WGSL. If not, something this group doesn’t care about.
*   MM: if people want to join this group and make suggestions - that would be great.
*   JG: other alternative, if they want to make their own extension, could do their own f8 extension. More palatable. Curious: new syntax. How do we handle that? Different subject.
*   DN: it’s the same, as long as you use this declaration before the new syntax.
*   JG: have to tokenize and parse. Turn off tokenization while we parse.
*   CW: all syntax needs to be backward compatible.
*   JG: if syntax wasn’t backward compatible, would need to pause tokenization while we parse these entries.
*   KN: think we can pause tokenization while parsing these.
*   JG: not sure I want to sign up for this.
*   KN: historical arguments - don’t want to maintain multiple parsers / impls of language. One impl understanding latest version of language, and correctly throws errors if you use new stuff. So, just parse newest version of language, and when you see something new, see if it’s been enabled or not. Don’t think you’ll need to change tokenization. Wouldn’t be able to add global keywords, but already have that restriction, because anyone using those as variable names would break.
*   JG: ESSL 300 changed a lot of things compared to 100. Maybe we don’t think that’ll happen here?
*   KN: if we do want to make a leap like that then it shouldn’t work like this - enable it in the middle of your shader. Either at the top, or out-of-band. This is a WGSL 2 file.
*   CW: can assert in shader that you’re version 2. But out-of-band sounds best.
*   MM: not sure we can’t add global keywords. Variables have “var” in front of them.
*   KN: right. Can add keywords in contexts.
*   …
*   MM: e.g. c++ added “override”.
*   CW: there’s major version breaking change thing. Enableable feature backward compatible = everyone OK with this proposal, except it should be arbitrary identifier instead of a list of these things?
*   MM: didn’t DM say these extensions shouldn’t be per module but per entry point?
*   DN: discussion that’s being tracked. I disagree, think that’s a bad direction to go.
*   AB: think whether it’s enabled per device is different thing.
*   MM: so maybe module scoped & entry point scoped extensions?
*   AB: nothing you need to attach to entry point specifically. Done via inspection.
*   JG: what when someone writes code activating extension, and uses it when unavailable?
*   AB: error. Accidentally use a feature.
*   JG: like f16? Not realizing it’s extension?
*   MM: you could use f16 without enabling it, or enable it and run on HW not supporting it.
*   CW: the latter. What happens when you write code on device supporting f16, and you run it on non-f16 supporting HW?
*   JG: i thought there was a call to make extension enabling implicit.
*   DN: I think that’s a bad idea.
*   CW: shouldn’t you have to turn on feature at device level?
*   KN: yes.
*   AB: someone has to catch the error.
*   MM: hypothetical future, adding ray tracing: enable API side ray tracing extension, and enable ray tracing in shaders?
*   DN: yes. Often two separate folks, need to communicate. Copying code snippets around, etc.
*   CW: RT might be special - new pipeline stage.
*   MM: f16 not good example, device doesn’t care.
*   AB: yes it does. It’s a HW feature.
*   KN: it has to be on device creation.
*   MM: so every WGSL extension has to have separate API side extension.
*   AB: if it’s enabling hardware and not just tooling side, yes. New syntax for loops in WGSL? doesnt’ need to be in API. HW features? Yes.
*   JG: f16 - people should opt into using them. Default place to put that is the device extension.
*   MM: different ways to opt in. Either device extension or enable(f16) at top of shader.
*   AB: having it be in WGSL is good for the tooling / compiler. API side - enabling the HW cap.
*   JG: you need to know whether you *can* opt in. Person writing website needs to know, can I use f16s? And then opt in to using them. Putting it in the shader gives you the second, but not “can I use” this.
*   AB: technically in Vk you have to request features before you create the device. Turn on before using.
*   DM: validation layer will complain if you don’t do this.
*   JG: could add another way to detect on program side if f16 is allowed or not. Natural way - it’s an API side thing. Similar to how WebGL works. Like enabling shader derivatives in WebGL 1.0.
*   MM: why do we need both?
*   AB: I agree if it’s entirely in shader tooling it could just be on the shader side.
*   DM: Vulkan requires request on device creation time. Opposite question: should we require that in shader language side?
*   CW: Explicit in shader supports good error messages when copy/pasting code. In addition to simplifying tooling.
*   MM: Why two sources of truth? In API side and WGSL.
*   CW: Do you agree on API side that I as a dev need to know if f16 is available or not.
*   CB: Distinction between HW features and software features.  E.g. private/protected is a language-only issue.
*   JG: Becomes an API concern if I want to have a hope to absorb a module that uses that new language-only feature.  Hopefully a feature detect process is “create a dummy shader and try it”
*   CB: May want a class of extensions that are related to what compiler can do and not necessarily the API.
*   KR: An explicit enable syntax will make for much better error messages when things don’t line up.  To developer it’s a weird interdependence with device creation, etc.
*   DM: You need to enable the feature on the device.
*   
*   DS: Timecheck!


### [Use brackets for array types #854](https://github.com/gpuweb/gpuweb/issues/854)



*   DS: Thought we punted to post-MVP
*   (like the next issue)


### [The order of top-level declarations should be irrelevant · Issue #875 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/875)


## Next Meeting



*   API
    *   Shadermodule creation
    *   Texture initialization woes
*   WGSL
    *   Ptr syntax again - #[1456](https://github.com/gpuweb/gpuweb/issues/1456)
    *   Extensions again
    *   Barriers #[1449](https://github.com/gpuweb/gpuweb/pull/1449)
    *   More of workgroup size specialization [#1442](https://github.com/gpuweb/gpuweb/issues/1442)
*   First public working draft (last hour, with dean)