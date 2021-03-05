# GPU Web 2021-02-24 VF2F Day 3

Chair: Corentin (WGSL = Jeff Gilbert, David Neto)

Scribe: Austin, Corentin, Kai, Myles, Ken, (...others, add yourself)

Location: Google Meet


## [Agenda / Minutes for Day 1](https://docs.google.com/document/d/1Csyz-MF9u2iw7WmjENTCr4BUbxnTgPk9l0u7ux6DSL4)


## 

<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: undefined internal link (link text: "Agenda / Minutes for Day 2"). Did you generate a TOC? </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

[Agenda / Minutes for Day 2](#heading=h.mwy32mzdki7z)


## Tentative agenda



*   Method of ensuring GPUShaderModules can contain MTLLibraries [#1064](https://github.com/gpuweb/gpuweb/issues/1064)
*   Web platform image interop
    *   Proposal for importing Web platform images in WebGPU [#1154](https://github.com/gpuweb/gpuweb/issues/1154)
    *   Investigation: Import VideoFrame from WebCodec to WebGPU [#1380](https://github.com/gpuweb/gpuweb/issues/1380)
    *   Proposal for a GPUVideoTexture object. [#1415](https://github.com/gpuweb/gpuweb/issues/1415)
*   Zero initializing textures can not be done efficiently [#1422](https://github.com/gpuweb/gpuweb/issues/1422)
*   (stretch)** **[#1465 Make requestDevice not return null](https://github.com/gpuweb/gpuweb/pull/1465)

**Put your agenda item below:**



*    (quick) status updates (timeboxed to 30m?)
*   Short term API items:
    *   Method of ensuring GPUShaderModules can contain MTLLibraries [#1064](https://github.com/gpuweb/gpuweb/issues/1064)
    *   Require [SecureContext] for using WebGPU. [#1363](https://github.com/gpuweb/gpuweb/pull/1363)
    *   Require storeOp when resolveTarget is specified? [#1376](https://github.com/gpuweb/gpuweb/issues/1376)
    *   Zero initializing textures can not be done efficiently [#1422](https://github.com/gpuweb/gpuweb/issues/1422)
    *   Can WebGPU canvas alpha be configured? [#1425](https://github.com/gpuweb/gpuweb/issues/1425)
    *   Timestamps units:
        *   Query API: How to understand the execution time of the Dispatch command [#992](https://github.com/gpuweb/gpuweb/issues/992)
        *   Query API: What is the GPU timestamp unit on Metal? [#1325](https://github.com/gpuweb/gpuweb/issues/1325)
    *   Make requestDevice not return null [#1465](https://github.com/gpuweb/gpuweb/pull/1465)
        *   KN: Late addition but simple
    *   Add GPURequestAdapterOptions.majorPerformanceCaveat [#1302](https://github.com/gpuweb/gpuweb/pull/1302)
    *   Remove GPUDevice.{adapter,features,limits} [#1309](https://github.com/gpuweb/gpuweb/pull/1309)
*   Short term WGSL items:
    *   Ptr syntax	
        *   Jeff’s proposal: [https://github.com/gpuweb/gpuweb/pull/1455](https://github.com/gpuweb/gpuweb/pull/1455) 
        *   Dzmitry’s: [https://github.com/gpuweb/gpuweb/issues/1456](https://github.com/gpuweb/gpuweb/issues/1456)
        *   
    *   [[WGSL] WGSL needs a plan for expansion · Issue #600 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/600) 
    *   [allow workgroup size components to be pipeline-overrideable constants · Issue #1442 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1442)
        *   PR: [https://github.com/gpuweb/gpuweb/pull/1444](https://github.com/gpuweb/gpuweb/pull/1444) 
    *   Brackets for array types - [https://github.com/gpuweb/gpuweb/issues/854](https://github.com/gpuweb/gpuweb/issues/854) 
    *   [vec4&lt;f32> is too hard to type on the keyboard · Issue #736 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/736)
    *   [The order of top-level declarations should be irrelevant · Issue #875 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/875) 
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
    *   Myles C. Maxfield
    *   Robin Morisset
*   Google
    *   Austin Eng
    *   Ben Clayton
    *   Brandon Jones
    *   Corentin Wallez
    *   Dan Sinclair
    *   David Neto
    *   Kai Ninomiya
    *   Ken Russell
    *   Ryan Harrison
*   Intel
    *   Yunchao He
*   Microsoft
    *   Greg Roth
    *   Rafael Cintron
*   Mozilla
    *   Dzmitry Malyshau
    *   Jeff Gilbert
*   Francois Guthmann
*   Michael Shannon
*   Mehmet Oguz Derin


## Method of ensuring GPUShaderModules can contain MTLLibraries [#1064](https://github.com/gpuweb/gpuweb/issues/1064)



*   (continuation of yesterday’s discussion)
*   Understand constraints / various points of view
*   Do we have a proposal for all the different constraints?
*   MM: left off: proposal where author can attach any subset of a RenderPipelineDescriptor to createShaderModule and presumably a map of entry point to those, and if author provides what’s necessary, browser can compile during createShaderModule. If not, compilation’s delayed until createPipeline. Different browsers will have different behavior.
*   CW: Is there concern that an application author would target only iPhones, and get good results there. But then perf portability concern when retargeting to another platform like Android.
*   MM: I think there is. This proposal is the only one though that satisfies Ken’s desire to give all the information they can, so that if the browser needs to do a workaround early, they can. If you want the other direction of asking for only a limited subset, and the browser will do it’s best, that’s also a fine direction to go.
*   MS: My preference would be that it’s defined what I need to give it, otherwise I see myself running all kinds of benchmarks on various platforms. Don’t know what I need to give it to get that performance.
*   MM: could we put it in the spec but not make it normative? Trying to allow Chrome to expand that set retroactively, and can only do that if it’s not normative.
*   KR: I don’t think there would be a way to specify IDL for that. Need to say something about what the argument is.
*   MM: Right, can’t literally be “any”. It would be useless for example to specify only the 1st and 3rd vertex attributes. There will be some amount of granularity. A strawman would be: make an object that is parallel to a pipeline descriptor, with all it’s field, but mark each at the top-level as optional. You can specify any of them, but if you do, the object must be fully populated.
*   JG: Basically the more carefully worded version of - “allow giving createShaderModule a pipeline descriptor”
*   MM: Yes, but you can give it half a descriptor.
*   KR: Don’t see how it solves the problem where we have a new workaround, and apps fall off the fast path.
*   MM: Someone made the point yesterday that if we have this, you might as well have a single function.
*   CW: If you want the fast path, then yes, you’d want a single function - which is what createPipelinesAsync would be.
*   JG: Sort of the GL model where you setShaderSource and compileShader, but compileShader may do nothing at all. It might be linkProgram that gives the compile error.
*   KR: Michael, can you comment on the forward compatibility / performance problem? The spec provides a set of things to specify, an app provides it, but then the set changes.
*   MS: I think there’s two types of users. Simple, and full 3D engines. 3D engines will try to keep up with it, but if any changes were made, it would be great if it were documented. As long as we knew what we needed to continue to add, it would be great. If it changes, and it’s documented what needs to be provided, we’d go back and add that if we can.
*   MM: Do you mean normatively int he spec, non-normatively somewhere on a website like developers.google.com, etc.
*   MS: In the spec would be nice, but as long as it’s somewhere, that’s good.
*   DM: Seems to me we’re choosing between always-slow and sometimes-slow. There is a path where we compromise performance compatibiltiy a little bit. I posted an example of the descriptor in #1064.
*   CW: I agree, and it’s unfortunate we might need to compromise performance portability to get better performance. I worry as Michael does that this is going to change. That’s why createReadyPipelines may be better because the browser can batch everything up front. Getting the state piecemeal looks difficult to optimize even if we have all of it, because of how many different combinations of state we need to consider. So the WebGPU implementation needs to talk to the shader compiler, …, etc. Don’t see it being able to use very well.
*   MM: In the world you just described, that’s the same world we have now. And Chrome can defer compilation if it chooses. This proposal is allowing some browsers who would like to do the early compilation to do it. Also, I don’t think createReadyPipelines is a bad idea. I just don’t think it’s sufficient.
*   JG: Is createReadyPipelines the one that is async and doesn’t jank and takes an array of pipelines?
*   &lt;darn, internet dropped>(discussion about which calls are async: createShadreModule, vs. createPipeline, and when jank can occur)
*   MM: The place where we’re coming from are different use cases. Applications that can’t compile all the pipelines up ahead, they would use createReadyPipelines. And those that can’t would provide the information at createShaderModule.
*   KR: If different browsers are going to provide different things, we should have them provide everything.   Or have spec a subset of the properties, encapsulate as its own object, and have the real pipeline descriptor inherit that object so it gets all that data.
*   DM: Devs don’t have or want to have all the information (e.g. multisample state)
*   MM: Hints are usually able to be wrong. I don’t think that’s palatable here - it would mean two compilations. So maybe we should use a different word.
*   JG: If you get the hint wrong, I think that’s fine.
*   DM: And you also want to be different multisample states, one that might be different from the hint.
*   JG: Could do a dummy pipeline shader compilation, and create a pipeline that looks a lot like what you’ll render. Put in guessed data.  If you later put in data that’s different but the difference is not material to the implementation, then you can reuse the compiled shader. Does htat work?
*   DM: Does not have the benefit of multiple entry points. The author might know that three entry points use the same pipeline layout, and this proposal doesn’t preserve that knowledge
*   JG: Motivates creating multiple pipelines from the same source.
*   CW: There was a github issue about create pipeline, but have a flag of “just fake it” i.e. don’t actually create the pipeline, to warm the cache.
*   CW: We might not have enough judgement from potential users about what they will do.
*   JG: We have createReadyPipeline. Can we make that plural?
*   MM: Could do that. Would not close this issue.
*   CW: Yes, and at that point you know all the transforms you need to do.
*   MM: Sounds like a good thing to me.
*   DM: Warmup flag sounds good. Also create multiple pipelines.
*   MM: I thought the point was to create a pipeline they might use in the future. 
*   JG: If you have this blitting pipeline, and it can operate in SDR or HDR mode, and you’re guessing in the beginning that it’s probably SDR. How bad is it that the pipeline is sitting around, and probably gets GC’ed?
*   CW: In my benchmarks. Creating metal library took about as much time as creating the whole pipeline.  In Vulkan the cost is even more weighted toward pipeline creation. If we go this route, a warmup boolean is not a terrible idea.  “Don’t give me the pipeline, but give me back null.”
*   JG: I’m curious what profiling would show for that - if we can demonstrate the performance. My intuition is that in the cases where perf matters, there’s a disjoint set between times you create pipelines you don’t use, and times where it doesn’t matter. I suspect we can get away with making it, and not having a warm-up flag.
*   MM: I’ll have to think more about the warm-up flag.
*   KR: In Chrome, the implicit caching of similar shader sources has been in place forever. There’s no explicit flag for it. That sort of deduplication could happen automatically without the flag, theoretically. I’m trying hard to understand the use cases here. Are we talking image/video apps that need a plethora of pixel formats or something? .. and not have jank at runtime?
*   MM: But there would be jank. If you passed the warmup flag, they would still need to actually compile the pipeline at runtime.
*   KR: I thought the cost of creating the MTLLibrary is just about the cost of creating a pipeline.
*   CW: Just to be clear: if there’s a warm-up flag, it would have to return null.
*   MM: Right, I think that makes sense.
*   DM: To Ken’s point, we can’t cache this. At pipeline creation we only have a single entrypoint. We’re talking about modules with multiple entrypoints.
*   MM: That’s a good point. The perf gathering I found showed that’s totally true. One of the benefits of a MTLLibrary in the first place. If you make a library and then many entrypoints, it’s way way faster.
*   KR: Still failing to understand why on pipeline creation we couldn’t first make a MTLLibrary out of the shader modules passed in, and then use that same cache for pipeline creations later.
*   MM: On pipeline creation? on pipeline creation, you only know the pipeline layout for a single entrypoint. You don’t know the layout for all the other entrypoints.
*   CW: I think the question was: when you have createReadyPipelines, you can see a lot of pipelines and a lot of shader modules, and you can bundle as much as you’d like.
*   MM: What’s the next step here?
*   DM: Aren’t we all happy with the warmup and multiple pipelines?
*   JG: MM says this doesn’t completely satisfy his concern. If createReadyPipelines satisfies Google’s desires, can we talk about what would satisfy Apple’s ?
*   MM: 1 MTL library then 100 entrypoints is much faster than 100 MTLLibraries. That’s the pragmatic goal. createReadyPipelines doesn’t solve the problem for apps that don’t know the rest of the pipeline information early.
*   JG: Why don’t you just make the MTLLibrary with all the entrypoints expressed in WGSL?
*   CW: You need to know the pipeline layout.
*   MM: In Metal, the pipeline layout lives inside the text of the shader. So to generate MSL from WGSL, we need the pipeline layout information.
*   JG: crazy idea: is that something we can move?
*   CW: Problem is that it solves the problem for Apple’s implementation for now. I know it doesn’t solve the problem for Chrome’s implementation. It’s brittle, and we could do it.
*   MM: But you’re no worse off than you are today.
*   CW: It sounds like a win until developers think they need to provide the layout there all the time, until things change it it does nothing most of the time. And then as an engine developer, you need to figure out what you need on what browser.
*   MM: As an engine dev, you supply everything?
*   KR: If we go in that direction, I request you factor it out into a separate structure, make it mandatory to pass in, and have renderPipelineDescriptor extend it.
*   JG: I’m describing moving pipelineLayout into shader module creation. period.
*   MM: What Google is saying: how did you determine the set of RenderPipelineDescriptor?
*   JG: Because it doesn’t work on Metal.
*   CW: Not satisfactory because it only works on Metal on Webkit.
*   JG: If you had createReadyPipelines, are you worried about this?
*   CW: Yes, worried about devs targeting iPhone, then target Android, and perf is poor.
*   MM: They’re no worse off than if we didn’t have this proposal.
*   JG: The different ways are not mutually exclusive though.
*   CW: I don’t think Webkit is going to be able to use that in the long term. Sure I’m speculating about someone else’s implementation, ..
*   DM: We don’t want to be stuck there either.
*   JG: I think we’re heading down the path of perfect being the enemy of the good.
*   KR: Moving the PL to createSM pollutes the APi in a non-orthogonal way. the PL is for pipeline creation. It happens that on some platforms you need it, and frankly yes, we’re going to need more information. But let’s not warp the API in ways that are harder to understand.
*   JG / MM: Don’t agree with “harder to understand”. It’s only true for the Vulkan model.
*   CW: And D3D12.
*   JG: We don’t have to permanently commit ourselves to what lives in the structures yet. We can take a good stab in the dark here, and see what we need and talk about it again. I don’t think there are perfect solutions here, but there are less bad solutions.
*   KR: I think incrementally factoring fields from the render pipeline descriptor into the render pipeline hint is the best solution
*   MM: Either is fine with me.
*   GR: Fuzzy on what the pipeline layout may include. Suspect it would benefit.. &lt;> ?
*   CW: it is the root signature’s descriptor.
*   CW: Moving the layout from pipeline to shader module is.. abysmal from our point of view. We’d rather have createReadyPipelines, and hints on the shader module that we can drop.
*   MM: Both Ken’s hint description, and JG’s pipeline layout migration would solve our concern.
*   JG: Ok let’s pick one.
*   CW: Let’s do what Ken described for now then.
*   MM: Can I try to restate this? I hear you’re saying: \
take the pipeline descriptor, make it inherit from a new class \
move more than 0 items into that base class \
make createShaderModule accept that base object, optionally \
and presumably each of the fields would be optional
*   KR: No, the whole thing optional, but not the subfields.
*   ..
*   KR: Breaking change though? Want to make sure the hint is optional, and extensible, so if we promote fields to the base class, we can.
*   BJ: Probably need to duplicate the structures.
*   JG: Not a big deal, we can.
*   DM: the binding model we have has the groups and bindings, and Metal has two binding models. One of individual resources, and one for argument buffers. But if you’re writing WebGPU, you’re writing based on the WebGPU binding model …  ?
*   DM: I think that having the plural createReadyPipelines with a warmup flag is the best we can do.
*   CW: I think that’s most useful what we can optimize for Chromium. We need to solve this issue. If we have to have two ways to solve it, that’s okay.
*   MM: So We’d be encouraging applications to take all the entrypoints they think they’ll need, put them in one shader module. Then, the application would make many fake pipelines. ..
*   DM: Yes, but you could end up with multiple MTLLibraries in the end/
*   MM: Or a single one with names mapped.
*   MS: But if you don’t return anything from that, then you have nothing to GC later. So those libraries will stay alive for the life of the application anyway?
*   MM: We’ll want to deduplicate pipelines anway.
*   BJ: That could also be related to the shader module, so it could be ref’ed by it and GC’ed when the SMs are gone.
*   MM: I’d like to think about it and discuss with the Metal team. Even if we do go with this proposal, I don’t think the story I just told will be obvious to developers. I hope we have a better story than evangelism. I’ll come back to this on the next call in 2 weeks.
*   RC: if the warmup thing provides no object, does that mean nothing was warmed up?
*   MM: No it’s the opposite. We don’t want to have any validation rules about what is/is not allowed for using a warmed up object, so we don’t return it. Later, if they do something, it might be made faster by the warm up.
*   RC: Does it live forever?
*   KN: web developer doesn’t control the cache. Browser has it’s internal mechanisms for it.
*   BJ: from a developer’s perspective, I’ll probably know my pipeline layout up front, etc. That’s probably constant. What’s not constant is probably the vertex layouts from a glTF model. So then I’ll just make up some hopeful vertex layout that I expect, and hope it works. As a developer, I’ll hope this works, but there’s no guarantee that the fact that I’m using a different vertex layout doesn’t bust the cache for me.
*   MM: I think your question is similar to Michael’s point earlier which is: how do I know what do pass in? What if I pass in the wrong thing? How do I know what I need to be confident about?
*   BJ: So if I guess, and it’s the wrong thing, I’m worried about overall developer mindset coming in.


## Web platform image interop

_Proposal for importing Web platform images in WebGPU [#1154](https://github.com/gpuweb/gpuweb/issues/1154)_

_Investigation: Import VideoFrame from WebCodec to WebGPU [#1380](https://github.com/gpuweb/gpuweb/issues/1380)_

_Proposal for a GPUVideoTexture object. [#1415](https://github.com/gpuweb/gpuweb/issues/1415)_



*   CW: ended up last time: generally, people felt GpuVideoTexture idea and importTexture idea were workable. Disagreement on which one we should do first. Jeff took action item to see what it would take to implement GpuVideoTexture. Had time to do this?
*   JG: I did some experimentation here for WebGL. There’s some tricky parts, but it still seems viable. What do you want to know?
*   CW: I think a lot of people in the group were thinking importTexture was a good first step to tackle first, and you had doubts about this.
*   JG: Eventually we’re not gonna want import texture - vast majority of use cases are not that. So it becomes this old method that we have to keep fast for old benchmarks. Instead want to support minimal copy path. So if we can do the One API that will be useful to everyone, we should. And if it’s implemented behind the scenes as a copy into an intermediate, so be it, but it’s on the path toward what we want.
*   CW: We also want importTexture for things like Canvas.
*   JG: I think it’s usually not what you actually want for Canvas. The only case where being able to sample from an external texture is not better than copying in a texture, is when you copy in a texture and want to reuse/modify it.
*   JG: Two proposals. One is copy texture from external. Other is external sampler.
*   CW: The first one is importTexture that _may_ do a direct import.
*   JG: I don’t know what that means, so I can’t comment on it. Can you describe the use case? I feel like you’re saying importTexture, but what you’re describing could either be an external texture sampler, or copy this external texture into my WebGPU texture.
*   RC: use case for 2D canvas like Babylon.js folks described: UI goes in 2D canvas, then use 2D canvas and put it in 3D scene. Want to avoid copying 2D canvas texture, and as much as possible, sample from original one. Know browsers may optimize the output of 2D canvas, may be more complicated for video (NV12, etc.).
*   JG: In my mind, I’d prefer to have that be satisfied by the external sampler API, where you can sample directly from the canvas or video.
*   RC: And you think that is different than what people mean when you say importTexture ?
*   JG: I took importTexture to mean make a copy.
*   RC: I see. Not what I [had in mind].
*   CW: importTexture means you have an ImageSource, and you import it, possibly with 0 copies (JG: copy-on-write?), maybe back buffer of canvas is torn off. As much as possible, no copies happening. If importing video not already in RGB data, yes, would involve a copy conversion.
*   KN: that’s source of some confusion. Have been focusing on video, and in most cases that requires a conversion to RGB.
*   CW: unless we expose multiplanar formats to the spec which is a bigger problem.
*   CW: For WebGPU in the importTexture proposal, you pass a subset of the texture descriptor, and you pass a usage. We could say it must be readonly.
*   RC: And then for 2D canvas, we could say you can’t write to it.
*   CW: Or 2D canvas does a copy on write.
*   KN: think copy-on-write would be reasonable but that’s just a surface thing.
*   JG: I worry that the implementation complexity is higher for that. Figuring out grafting a WebGL resource onto a WebGPU device..
*   KR: It does get into interoperability extensions for various APIs.
*   JG: More specifically, I think sampling/fetching is easy for that case, but I don’t think anything else is. The thing we’re able to offer is not really just a full GPUTexture, so I’m a little gun shy.
*   CW: Could be limited to Sampled and CopySrc, for example.
*   JG: It’s just not really a texture..? It’ll be a very different underlying object. It’s not going to be a VkImage.
*   CW: It would be. In our impl, there’s an interop image concept. The canvas backbuffer is one such interop texture. We create one of these, and then say, please wrap this in a GPUTexture. It’s a real VkImage, but it doesn’t own its allocation.
*   JG: Worried about WebGL in that it’s a different type of texture. A different texture target. I understand that Chromium experimentation has been with Texture2Ds.. but worried about the sharp edges there.
*   JG: I’m worried about trynig to make a thing that most of the time looks like a GPUTexture that refs a different resource, and worried about the implementability given .. the other types of textures we have?
*   CW: How do you solve RC’s problem with canvas.
*   JG: Have ability to sample from more different textures
*   CW: So there would be no way to import external stuff to look like a normal “GPUTexture”
*   JG: Not for NV12 stuff. I don’t think I can always take a video coming from a decoder and turn it into a texture2D without doing a copy.
*   CW: Correct. The point is, you need importTexture for something like canvas, and you can do a copy conversion for video, but the video sampler is harder to implement which is a more research-y thing.
*   JG: Worried both statements are untrue. Having a sampler which just handles this is not a super difficult problem. The thing I’m worried about on the flip side is having to write the code that handles the edge cases like mutating textures, how it gets promoted if you do that, while also hitting fast paths.
*   KR: If things are phrased as acquire/release, we provide the feasibility of super optimizing in the future. A trivial implementation could do one copy behind the scenes. But you provide all the API structure for getting to 0-copy incrementally, bit-by-bit.
*   JG: What I worry is that it is much more to commit to than just sampling.
*   KR: That’s definitely not the intent here. The intent is only to allow them to be sampled from WebGPU. Not written or mutated.
*   JG: Sounds the same as my proposal, except mine says that there’s always an external sampler. Because we’re not going to want to always polyfill and put extra things in the shader.
*   KR: CW, if this importTexture idea used an external sampler concept all the time in chrome’s implementation that was lowered to normal 2d sampler, does that sound palatable? No automatic sampling of NV12 for now.
*   RC: does that mean video will always copy to do NV12->RGB.
*   KR: yes
*   CW: If we have to do a GPUVideoTexture(GPUExternalTexture?) and only that, we would fake it and see later. I don’t know if having GPUExternalTexture would work for all the things we want to do or not.
*   KR: Do you think we can lie to the application and tell them all the types sampled must be sampled with an external sampler? and then paper over the differences.
*   CW: What we call GPUVideoTexture today which is external_sampler_OES-like proposal. Technically can work for canvas as well. But the problem is this is only sample-able. You cannot copy. You have to do it with a blit shader. Can be slightly less optimal, and I worry that it prevents: 1. extending to other stuff that’s not video/canvas. 2. Prevents importing, then releasing, and transferring ownership to someone else. Prevents importing and then transferring to WebRTC or WebGL or WebML, etc.
*   JG: Really hard to commit architecturally, particularly on multi-GPU systems.
*   CW: Yes, but browsers already have magic to do copies between browsers.
*   JG: Paper over too many differences and it makes it look like it’s fast but actually slow.
*   KN: I think that being able to do texture copies out of things from canvases - for text in texture atlases.
*   KN: Secondly, I think video and canvas don’t have to look similar. They can be different APIs.
*   JG: I see the definite need for minimal-copy video. At the same time, I’m sympathetic to the need of copying into a texture atlas. For that maybe a simple CopyFromCanvas would suffice.
*   DM: What’s the end game? I thought zoom and friends want to squeeze performance. Or do we want good enough.
*   JG: I think balance between portability and performance is important. So we should drive down number of samples and copies, but I’m gun shy of giving planes directly.
*   MM: Isn’t there an obvious problem about lifetime?
*   JG: Like access to a surface that needs to return to the decoder? Yea there’s proposals about doing latching for that. You have to explicitly say you want to latch a new frame, and then can unlatch it sooner. But at the end of rAF, everything unlatches. That’s one of the proposals.
*   MM: Yanking them away seems like poor design.
*   KR: Can’t give irrevocable access. That would cause deadlocks. Texture going black is probably the best solution we can do.
*   JG: If they want to hold on, I’d ask them to do so explicitly, rather than adding copy-on-release semantics.
*   KR: WebGL has preserveDrawingBuffer: false. It is a well-understood failure mode that if they see flickering from copying from a WebGL canvas, it’s because they’re doing it wrong.
*   DM: I wanted to ask if we have ISV feedback about what they would want in a larger sense. Like would they be happy with a fat sampler at all.
*   RC: Babylon.js cares the most about RTC videos, then 2D canvas, then other videos. They’re happy with either fat sampler, or give them NV12 stuff. NV12 is more inconvenient, but they’ll do what it takes.
*   JG: Interesting they have a preference for fat sampler.
*   RC: Only because it’s convenient, but they would do NV12 as well.
*   CW: On our side, users are less interested in canvas, so I don’t have data there. Either importTexture or the fat sampler works for them. They don’t think the extra copy will be that expensive if it stays on the GPU. No real preference, but not real concerns with the fat sampler. They would be using WebCodec which is yet another thing.
*   DM: Nobody is concerned they won’t be able to linearly filter it? Maybe they assume they will be and we’ll tell them they cannot.
*   RC: For NV12?
*   CW: That’s kind of why GPUExternalTexture as a short term solution is scary, it has a lot of hidden issues like this.
*   KR: We’ve had difficulty getting customers to pick up the WebGL webcodecs extension. Neither wanted to build out the pipeline for it. I did want to ask. is importTexture mutually exclusive? (No). So importTexture could always use the external sampler and we’d treat it as RGBA internally.
*   CW: Not mutually exclusive but importTexture->GPUTexture, fat thing gives you an opaque GPUExternalTexture.
*   KR: Going to JG’s copyImageBitmapToTexture. I pushed for this but unfortunately it carries significant overheads that make it unsuitable for video.
*   JG: It was a good dream.
*   KR: Consider renaming it to copyToTexture and take any ImageSource. Would be great, would solve a developer problem in WebGPU. Or can say importTexture is the path, gives a restricted texture [but without fat sampler?]
*   JG: Unsatisfactory thing about WebGL’s solution. API friction uploading canvas2d to WebGL. Actual pixel format usually different. Hard to bridge that gap, can’t get super fast uploads without conversion. Would be nice to have an API where if you were copying into a subresource it was very fast. Dream would be a canvas-sub-region->texture-sub-region that can be a blit even if pixel formats don’t match.
*   KR: Chrome implemented this for WebGL (2 only). The subrectangles of src/dst, keep it on GPU.
*   CW: Shaobo is implementing the same in Chromium now. It helps if the receiving texture is OutputAttachment, so you can use a blit. If you do that, the problem is much easier.
*   KR: Would be great. Would prefer it.
*   JG: [...] for MVP
*   KR: Can issue warnings if you try to blit into a non-OutputAttachment texture. But would prefer error.
*   CW: Attempting to summarize
    *   CW: The fat sampler approach also works well for canvas.
        *   KN: Small concerns but should be easy to resolve.
    *   JG: Most agreed-upon thing is being able to copy resource subrect into texture subrect.
    *   CW: Fat sampler approach first?
        *   CW: We are pretty warm to doing this (naive-approach).
        *   DM: Can we get more details on the proposal first? Need details on opaque texture and sampler types, filtering, etc.
        *   RC: Pretty sure they will want bilinear filtering e.g. if rendering into a 3d scene.
    *   MM: My thought is I’d like to leave the door open for the copy call. Fine to have fat sampler first.
    *   KN: There are three things: copy-subrect, fat sampler, and maybe-zero-copy-import.
    *   CW/JG: Want to have copy-subrect now.
*   CW: Consensus: Copy-subrect yes. Fat-sampler yes. Other thing maybe later.