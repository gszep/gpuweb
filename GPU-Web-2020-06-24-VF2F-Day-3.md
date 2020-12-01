So as to not be anonymous animals, the doc is shared for writing with the google accounts that are invited to the meeting. If you can’t edit let [cwallez@google.com](mailto:cwallez@google.com) know. Also be sure to be in “Edit” mode and not “Suggest” mode.




Chair: 

Scribe: 

Location: Google Meet


## Tentative agenda

WGSL



*   Textures ([#573](https://github.com/gpuweb/gpuweb/issues/573)) and Samplers ([#574](https://github.com/gpuweb/gpuweb/issues/574))
*   Enums ([#661](https://github.com/gpuweb/gpuweb/issues/661))
*   Specialization constants, function constants, or other forms of shader modularity ([#572](https://github.com/gpuweb/gpuweb/issues/572))
*   Packed vector types ([#780](https://github.com/gpuweb/gpuweb/issues/780))
*   WorkGroup memory size and barriers? ([#863](https://github.com/gpuweb/gpuweb/issues/863))
*   A single shader is often split among many source files ([#741](https://github.com/gpuweb/gpuweb/issues/741))
*   C-like declaration order ([#677](https://github.com/gpuweb/gpuweb/issues/677))

Web GPU Core



*   Out-of-memory for buffer mapping [#872](https://github.com/gpuweb/gpuweb/issues/872) (Austin)
*   Copying of depth24plus formats [#652](https://github.com/gpuweb/gpuweb/issues/652) (yes, again) ([summary](https://github.com/gpuweb/gpuweb/issues/652#issuecomment-646917611)) (Kai, Dzmitry)
*   Development-only API features ([#814](https://github.com/gpuweb/gpuweb/issues/814))
*   Case for optional GPUBindGroupLayoutEntry entries [#851](https://github.com/gpuweb/gpuweb/issues/851) (Dzmitry)
*   Follow-up on when GPULimits are enforced [comment on #409](https://github.com/gpuweb/gpuweb/issues/409) (Austin)
*   Can we simplify the data uploading primitives to 2 at most? (mapBuffer, createBufferMapped, writeBuffer) (Ken)
*   Introducing a GPUEncoderBase (Dzmitry/Brandon) [comment on #785](https://github.com/gpuweb/gpuweb/pull/785#discussion_r428266148)
*   Fancy render pass attachment load/store/readonly (Kai/Brandon) [comment on #831](https://github.com/gpuweb/gpuweb/pull/831#discussion_r436195260)
*   Where to store encoder state in the spec? (Kai/Brandon) [comment on #850](https://github.com/gpuweb/gpuweb/pull/850#discussion_r437053808)


## [Agenda/Minutes doc for Day 1](https://docs.google.com/document/d/1eiDkPsGbrrYjoiHKBTSH_KspYmqJj3ZWbjXuoMAk_rk)


## [Agenda/Minutes doc for Day 2](https://docs.google.com/document/d/1WI9PzwXy39fFHuZLUnEQtSplwfmFaZ2oVb0gXJsA2P4/edit?usp=sharing)


## Attendance

WIP, it is the list of all the people invited to the meeting. **In bold the people that have been seen in the meeting:**



*   Apple
    *   **Dean Jackson**
    *   Fil Pizlo
    *   **Justin Fan**
    *   **Myles C. Maxfield**
    *   Robin Morisset
    *   Theresa O'Connor
*   Google
    *   **Austin Eng**
    *   **Brandon Jones**
    *   **Corentin Wallez**
    *   **Dan Sinclair**
    *   **David Neto**
    *   **James Darpinian**
    *   John Kessenich
    *   **Kai Ninomiya**
    *   **Ken Russell**
    *   Shrek Shao
    *   **Ryan Harrisson**
*   Intel
    *   Brandon Jones
    *   Bryan Bernhart
    *   **Yunchao He**
*   Microsoft
    *   Chas Boyd
    *   **Damyan Pepper**
    *   **Rafael Cintron**
    *   **Greg Roth**
    *   **Tex Riddell**
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
*   Kings Distributed Systems (KDS)
    *   Dominic Cerisano
*   Joshua Groves
*   **Matijs Toonen**
*   **Mehmet Oguz Derin**
*   Pelle Johnsen
*   **Timo de Kort**


# Textures ([#573](https://github.com/gpuweb/gpuweb/issues/573)) and Samplers ([#574](https://github.com/gpuweb/gpuweb/issues/574))



*   DS: I posted an update in #573.
*   DN: bias parameter probably needed too.
*   DS: bias is on there now. LOD is bias?
*   DN: no, different. Bias is for depth stuff.
*   DS: depth comparisons have bias.
*   MM: what does spirv use a sampler for?
*   DS: image fetch takes OpImage, which takes OpSampledImage, which is formed from sampler and texture.
*   MM: i mean what is it actually used for?
*   DN: samplers can be used for texture coordinate wrapping
*   CW: reading spirv1.5 spec, OpImageFetch takes Image, not SampledImage. Just the Image, not Sampler.
*   MM: don't need sampler when loading from image then.
*   DS: OpImage takes SampledImage. It's the first parameter.
*   DN: OK, that's for deconstructing a SampledImage. Different things. Might have been supplied as a SampledImage, and need a way to deconstruct it to get the Image to fetch out of it.
*   DS: do we need to handle both of them?
*   DM: we don't have combined image samplers, so we don't need to deconstruct them.
*   DS: the only change then is Load no longer requires a sampler. The rest is fine?
*   (all: yes)
*   MM: question: why does Metal have distinct type for depth textures? Opcodes on actual devices are the same. Depth textures are a convenience, just for type safety when writing C++ code. There are functions on depth textures not on regular textures though. To expose those in WGSL we have to go through the depth textures.
*   DM: the function's SampleCompare, only available on depth samplers. Do you know - can you define both depth- and non-depth textures in Metal on the same register? Could then use the one which matches
*   MM: don't know the answer to that. Can take an AI.
*   CW: why is it mytexture.sample(sampler) rather than sampler.sample(mytexture)?
*   DM: sampler is a subject. texture is an object.
*   MM: I agree with Dan.
*   MM: the sampler is a struct that contains all the arguments that would have been passed into the sample function.
*   KN: it's not though because you have to create and bind it. Conceptually a piece of state in your hardware.
*   JG: I don't like that it's a method call.
*   MM: that's what i was about to say. We don't have the concept of classes and methods.
*   DS: I'm fine to switch it to the GLSL way: textureSample(texture, sampler). I went this way because it seems to be the way most shading languages (HLSL, MSL) do it.
*   JG: I like it independently. If it was C++ this is how I'd like it to look.
*   ...
*   HLSL calls it Load, MSL calls it Read, GLSL calls it TexelFetch.
*   TR: I think the function makes more sense for this language.
*   GR: I agree. Then methods could be an alias in some future WGSL.
*   JG: I'm used to Load/Store. Read/Write works.
*   MM: I prefer something one word. TexelFetch is too many.
*   JG: Need both read/write variants too.
*   CW: do we have to match ReadOnlyStorageTexture, so Read/Write?
*   ...discussion about Read vs. Load, Sample...
*   TR: might be useful to separate typed vs. untyped loads/stores. Found that to be important. Difference between Image and Buffer.
*   MM: can you load from a buffer? Think you use square brackets for that.
*   TR: if that's the way the lang separates it, that's fine. Single function that maps to both is problematic since they're different operations.
*   JG: let's go with that for now then. Load, Sample, Store.
*   DM: there is a problem with having those 3 methods on the same texture. No distinction between storage and texture bindings currently. At API level, they're separated. SampledTexture allows loading, but not other way around.
*   JG: type matching should tease that out. sample() function would not have overload, would not accept Image. We decided not to do methods, so this is free-standing function which has specific overloads.
*   CW: concern is that the type doesn't encode the constraints on the texture. You're allowed to pass REadOnlyStorageTexture2D because constraint doesn't exist on the type.
*   MM: so we should make StorageTextureRWTypes?
*   JG: texture/textureRW makes sense to me.
*   MM: RWTExtures wouldn't support samples.
*   JG: this is just images then?
*   DM: ROStorageTextures can't be sampled.
*   TR: Images are the only thing that can be sampled.
*   DM: we probably need 3 diff types in WGSL to handle this. Decl of sampled texture in shader only cares about component type; the others need the full format. Might as well have different type.
*   MM: so in WGSL can you load and sample from the same texture?
*   DM: We should support loading from a sampled texture.
*   MM: so 3 types, load, load/sample, store.
*   CW: and later a 4th type, just load/store.
*   KN: [there’s a table about this in an issue somewhere](https://github.com/gpuweb/gpuweb/pull/532#issuecomment-570130102).
*   CW: Could call for implicit promotion/conversion. Sampled texture should be able to decay to a readonly texture.
*   JG: can do that with overloads, or we allow conversion.
*   CW: Thinking about composability of WGSL code, where you write a helper function that takes in a readonly texture
*   JG: can do that without being explicit - allow downcasting from RWTexture to ROTexture, for example. If we had RWTexture.
*   CW: Or downcasting from a sampled texture to a readonly texture.
*   KN: found the table (see link above). More complicated. Sampled textures can be sampled and fetched (two different op codes), but not read or written. Writeonly storage can only be written, readonly only read.
*   DN: Fetch: getting single texel from sampled image. Read goes through sampler. Different in Vk.
*   CW: OpImageFetch and OpImageLoad are the same thing except for the HW path they take. Fetch goes through memory subsystem, Load computes the texel location in memory and loads it as though it was a buffer. Same thing, diff hardware paths.
*   JG: possibly bypassing the texel fetch path
*   MM: Metal doesn't have this distinction. If HLSL doesn't either then it's a strong argument for not needing both in WGSL.
*   DM: certainly tempting.
*   DN: One of the resource types is if you want to access a Vulkan buffer, put an image view on it (texel buffer), which does the unpacking and implicit conversion as a regular image format, but you don’t have to change the kind of resource type in Vulkan. Not sure if that matters now.
*   MM: why does that have diff operations on it and not just a 1D texture with no mipmaps?
*   DN: On the API side you set it up as either a buffer or an image.
*   TR: in D3D diff btwn buffers and 1D textures: can't sample a buffer...
*   MM: my question was really, if you have buffer and then make texture object referencing its buffer, sounds like in spirv you use diff opcodes to access it as opposed to 1d texture that doesn't reference an underlying buffer?
*   JG: the two opcodes are OpImageRead and OpImageFetch. OpImageFetch, have to use image whose sampled mode is 1. OpImageRead you must use with an image that has sampled mode 0 (don't know, for opencl) or 2 (not sampled, storage image).
*   MM: How does GLSL expose these?
*   CW: GLSL has diff types for storage and sampled. Sampled is sampler2D. Storage is image2D. Can't cast between one or the other. Depending on type you know which of the two views you have.
*   DN: shows up in decl of image. Storage texel buffer, has dimensionality of "buffer" rather than "1D" or "2D", "Cube", etc.
*   DM: in spirv if you have a ... then you have to fetch from it rather than sample from it?
*   CW: OpImageFetch doesn't use a sampler.
*   JG: possible to OpImageFetch things out-of-bounds, since it doesn't care about wrapping.
*   DN: you give it an image, but its sample operand must be 1 (constructed as a sampled image).
*   DM: then it'll use Fetch if the binding is sampled image, and Sample if the binding is ...?
*   Discussion about compile failures when you attach bindings to it.
*   MM: can you do that operation in spirv? 
*   JG :think we need different types for these.
*   CW: think we're all saying the same thing.
*   DM: what does downcasting mean?
*   JG: RWImage -> ROImage. Some function only reading from image, don't have to duplicate the code path.
*   CW: can you downcast sampled image to ROImage? No, because spirv code will be different.
*   JG: if we want that type safety we need to be able to do this. Doesn't make sense to do one without the other.
*   DJ: we're getting to half past, very much in the details now, maybe file an issue?
*   DM: we went into this because we need different types.
*   CW: at this point we agree that we need SampledTexture, ROTexture, WriteOnlyTexture. Not just one texture.
*   MM: and if no way to turn RWTexture into ROTexture in spirv, then shouldn't be able to do it in WGSL.
*   CW: it is possible, because you handle the RO-part yourself.
*   DM: we need at least one type for storage. Whether controlled by access flags or types is a different decision.
*   JG: sad about depth textures being different, but sure.
*   MM: you can bind depth texture as non-depth. Only need to use depth texture type in MSL if you want to use the functions that only exist on depth textures. Need to verify this.
*   DJ: can we agree - next step is PR on this, and we can discuss it at next week's WGSL meeting?
*   DS: OK, will see if I can turn it into PR.
*   DM: another problem: if we have separate types for depth textures, and comparison samplers, what's the conversion path from spirv?
*   DN: being depth texture or not, is a matter of usage. Can bind depth texture in ordinary way. Would write depth through rendering pass, then in another pipeline call use it for depth comparisons. Matter of the binding. (Then said wrong thing about setting Depth=1 on OpTypeImage. Later KN says correctly that Vulkan ignores the Depth=?? parameter.)
*   DM: I'm talking about comparison textures. Unused flag?
*   KN: Vk env spec says that field is ignored.
*   DM: how do we generate a WGSL module out of spirv-module?
*   JG: detect static use of depth-only samplers. Duck typing it. Do you use depth comparison operators? From GLSL i see how to do it. From spirv that's already narrowed and had this info thrown away...can do the same thing. Know whether the depth comparison operators are used statically. Can be wrong if they're based on runtime use. Is that allowed in non-OpenCL spirv?
*   DM: so have static analysis for globals used, now have it for comparison samplers.
*   MM: also could read spirv-cross.
*   DM: don't think it'll be good example. Trying to do this from the ground up.
*   JG: Presumably if SPIRV-Cross targets MSL it has to handle this.
*   DM: doesn't have the same constraints we do. Want to do at shader module creation time,  and have it be fast.
*   AE: spirv-cross has a way to tell you if a shader id in the shader is a depth or comparison texture. They use it in the MSL backend.


# Enums ([#661](https://github.com/gpuweb/gpuweb/issues/661))



*   MM: don't think anyone thinks we need enums. Just make global constants. Maybe want to add in the future.
*   JG: agreed. Do we have a way to mark things for future versions?
*   DS: we have a post-mvp list. Also should add "enum" to the list of reserved words.
*   MM: sounds good.
*   JG: I'll file the issue
    *   [https://github.com/gpuweb/gpuweb/issues/881](https://github.com/gpuweb/gpuweb/issues/881) 


# WorkGroup memory size and barriers? ([#863](https://github.com/gpuweb/gpuweb/issues/863))



*   Discussion about entry points as well.
*   MM: in Metal, 2 ways of describing workgroup variables. workgroup keyword. Size is sum of variables marked that way. Or, array of indeterminate length. Length spec'd at pipeline creation time. Don't think we have the second way in WGSL?
*   DN: Vk can do that by making array size specialization constant.
*   MM: you'd make workgroup array with size spec constant?
*   DN: yes. We don't have to use the same mechanism. Don't know if we need this for mvp.
*   MM: people who use Metal say it's useful. But agree we don't need it for MVP. Annotation on particular variable is fine.
*   DN: in OpenCL you’d pass a size to ... kernel arg, set size.
*   TR: does the array size break down into both # groups and threadgroup size, or just the latter? In D3D you dispatch a grid of threadgroups. Each TG has a size. Limited # you can handle on a single compute unit with amount of group shared memory you have.
*   DN: wg allocated per-tg. you don't have to allocate them all at the same time.
*   MM: There is a maximum value that the author can request: like you can’t allocate 100GB of memory.
*   TR: is it 1024 threads?
*   MM: limits both ways - on #threads in TG, and total amount of WG memory available to a single TG. Different numbers, because different units.
*   TR: you expressed something in terms of array size. Only local to the group?
*   MM: The programmer is responsibly for converting between the bytes specified at pipeline creation time, and which indices are valid inside the unsized array
*   TR: OK.
*   MM: create memory, X bytes long, and it's the shader's responsibility to not index off the end.
*   DN: is there a query in MSL to get the size of that runtime-sized array? 
*   DN: Will need to fail pipeline creation if shader requests too much workgroup storage.
*   get max value in Metal?
*   MM: must be.
*   DN: when I wrote index clamping for spirv, I have the array size as specialization constant.
*   MM: also impl-specific limits on max # threads in WG for example.
*   DN: at pipeline launch / creation time can check the limit. Also check you haven't blown other size limits
*   TR: was confused we were talking about TG size, then talking about array size, sounds more like group shared memory usage.
*   MM: we have both of these constants. Impl- and device-dependent. Will have to figure out how to expose both concepts to WebGPU.
*   DM: do we know how we'll expose TG memory to WGSL?
*   DJ: was going to ask because the next topic is specialization constants. Will that be the way we expose these limits?
*   MM: think it'll work fine. Sad we won't use the same way to expose this and the max # threads in the workgroup.
*   DJ: how is max number threads exposed?
*   MM: isn't now. Have to come up with way. Specialization constants can't work for that purpose.
*   DN: max you can request, or size of current workgroup?
*   MM: the former?
*   DN: the max size i could launch is immaterial inside the shader. Need to know the dispatch size I'm in.
*   KN: these are all limits that would go into GpuLimits array on the API side.
*   MM: # threads in WG is based on register pressure.
*   CW: discussed this a while ago. For compute, shader compiler can say "no, didn't work" - API-side issue.
*   MM: we resolved it was OK for WebGPU to reject compute shaders?
*   CW: we said it was a difficult topic and never came back to it.
*   DM: driver has to be able to spill registers to memory and then support any group sizes. People see drastic perf hit, but at least it compiles. Could issue synchronous perf warning.
*   MM: is it possible to validate that? Create program with huge register pressure?
*   DN: Yes, it is. See here: [https://github.com/gpuweb/gpuweb/issues/275](https://github.com/gpuweb/gpuweb/issues/275)
*   DN: would like compiler to be able to reject. WebGPU has to be able to run a prescribed set of things. If I requested a single word's use of WG memory, that's not sufficient reason for the driver to reject my shader. Have to have a minimum bar that the WebGPU impl has to support.
*   MM: So webgpu needs to specify minimum/maximum?
*   DN: .. might be spilling some things from private memory into workgroup memory..
*   DM: how do we know if it spills? We don't know register allocation of shader.
*   DN: Right, but I’ll write a simple test that uses 16kB or 32kB of workgroup storage, and that should pass. You might write some complex shaders that cause the driver to reject, but those would be on the fuzzy boundary. We should still have a set of tests that definitely should pass in the CTS.
*   DM: You said they must pass? What does it mean to not pass? Reject pipeline creation?
*   DN: Should be able to write a test that uses 16kB of array workgroup storage. In order to pass WebGPU, that pipeline should execute and compute the correct result.
*   MM: what about device that doesn't compute and create the result?
*   DN: No because that test we will all agree is a reasonable use of workgroup storage.
*   MM: so we need to know which devices / drivers can pass the test so we can disable WebGPU on them. Contract of Vk is that you can reject things for any reason.
*   KR: This isn’t practical and ran into this in WebGL ad infinitem. For WebGPU to be reasonably correct, the working group needs to define what the conformance criteria are and say like: we pass on two GPU types on some systems, but at runtime the user will have to check the compilation status, and hopefully the driver would reject. Hopefully there aren’t bugs in driver register allocation. I think the best you could hope for is validating the WebGPU implementation is correct up front, but knowing that a certain driver is bad is infeasible.
*   MM: API contract of webgpu is we can reject any kernel for any reason, but browser vendors will try to know what gpus are unreasonable. authors will still have to check their code.
*   CW: Yes, I think if you start to do compute shader toy and go way overboard, we can fail compilation. The intent is that all real use cases will be supported, but there isn’t a good way to define that properly.
*   DN: confused by why this is complicated. Shader might be rejected, can't predict ahead of time which ones will. We'll have some shaders that will define what GPUs pass the CTS. Users' shaders will roll the dice.
*   DM: I don’t think rolling the dice is fine from the user’s perspective. We can’t get enough coverage in the CTS. We have to anticipate that some things may fail because Vulkan allows them to. We can’t just close our eyes on it. We should at least adopt createReadyPipeline so that users can react accordingly.
*   CW / MM: Yes we agreed to that.


# A single shader is often split among many source files ([#741](https://github.com/gpuweb/gpuweb/issues/741))



*   DJ: think we can close this one. In MVP we don't need this function. Can be done on the JavaScript end. Think Myles is the only one who's expressed a strong desire to have it.
*   MM: think it's OK to leave out of MVP but its omission means there's a different issue. [#875](https://github.com/gpuweb/gpuweb/issues/875).
*   DJ: was going to say that. Need to be wary that we don't do anything order-dependent that can't be overridden by what we do for multiple includes.
*   MM: think we need 1, maybe 2, of these solutions to the problem.
*   DN: #875 says "irrelevant" - that's a strong condition - definitions coming before use is the only constraint.
*   MM: should be able to use before declared. If wanted to enforce something like "file A needs to be before file B" in concatenation, need dependency tree in order to maintain that. Dependency tree for shaders today means #includes. 2 solutions: #includes are resolved by the browser. Or have this dependency order be out-of-band somewhere. Third option, don't have order be imp't at all
*   DN: don't think the necessity to keep dependencies is crazy. Need to track that anyway. Developer needs to know what they're doing.
*   KR: does any native shading language support use before def?
*   MM: no, but we're in a unique situation in the browser where we don't have #include.
*   Discussion about preprocessors, their complexity, etc.
*   JG: people can have cheap little solutions on top of us
*   KR: Allowing use before def dramatically changes the structure of the program and compiler. I strongly object. Very other library like Babylon and Three.js have their own user-level shader mechanisms to handle this.
*   JG: we're disagreeing on what counts as onerous. Having JS string identifier / rejector / using JS templating mechanism is easy, and what happens in WebGL at least. Stronger argument is that it's too hard to do this.
*   MM: there are fewer browsers than web sites. Don't think it's hard to support use before def. Calling a function before it's def'd isn't hard
*   JG: it's weird. Signing us up for a two-pass compiler is something we can discuss, but not hard requirement for us to use #includes.
*   DS: does that mean you can use global variables before they're declared?
*   MM: yes.
*   #875 is this issue.
*   DJ: propose #741 closed as not needed for MVP. Continue in #875. Myles requests that we be aware of #875.
*   MM: right now it's underspecified. Not requesting any changes now. Not clear now whether you can call a function before it's declared.
*   DS: think it's spec'd that you can't do that.
*   DN: that should be a scoping rule. Declaring function, like variable, that object exists and is in the scope. Module scope.
*   DS: it's in the validation section but maybe not in the spec text.
*   MM: I missed that
*   KR: allowing use-before-def completely changes parsing. Don't know the types of variables in expressions!
*   DJ: Let’s not discuss this now. That’s an open issue that we’ll discuss at another meeting. We’ll close this one as not needed for MVP (#include).


# Specialization constants, function constants, or other forms of shader modularity ([#572](https://github.com/gpuweb/gpuweb/issues/572))



*   MM: Made investigation about what facilities languages have. MSL and SPIR-V effectively have the same thing, but HLSL doesn’t. Maybe we should ask the HLSL experts whether they think that exposing the Metal/SPIR-V way of doing this is acceptable. The Metal/SPIR-V way is that you have a global variable with an annotation that says it is a function constant. In our two-phase compilation, one is string to code, and the second you provide values for those global variables. The second step is supposed to be faster.
*   DN: I'm curious about DXIL - does it have a facility to make this faster?
*   TR: what's gained from this?
*   DN: can do a lot of precompilation to intermediate language. Tries to collapse some of the dimensions of shader explosion.
*   MM: One common use case is an uber shader where there are 100 different code paths. In the naive way of implementing this, you’d have to run a full compilation step for each permutation of those flags. If you instead annotate these flags as function constants, then you don’t need a full compilation for every one. You run the heavy compilation first, and then run a faster second step for every shader permutation. This is really important for Metal. We have lots of third party iOS apps that use this strategy to improve compilation times.
*   TR: DXIL doesn't have specialization constants, but we can generate something in the same IL, just not DXIL, that has the same constants that we can run passes over. Have to do this anyway for other reasons. Can include specialization constants in this, but driver overhead won't be lower when you change a specialization constant.
*   MM: Sounds like a big win on Metal and SPIR-V and only somewhat of a win on HLSL.
*   KN: not convinced the win on HLSL is that much smaller. I assume app developers expect (e.g. Vulkan) driver to do is dead code elimination after spec constants so there’s quite a bit of work to do at pipeline creation even if supported.
*   MM: we do dead code removal. Second compilation step is much faster.
*   DP: this is a request the D3D team's heard before. Currently they've decided not to implement it in D3D. Don't think it's a pessimization for D3D - and we could add native support for it. We don't object to it being in the language. They don't think it's that much of a win for the D3D ecosystem.
*   MM: there is another impl strategy - compile shader, leave these spec constants in as root constants, and then specify them in the root signature.
*   DP: yes. D3D12 does have the ability to do profile-guided optimizations to handle that form.
*   DM: I like the idea of shader translation / type checking being done knowing this thing has this type, value supplied later. What's the interaction with the shader interface? "if (false) { ... use binding ... }" - static use or not?
*   MM: probably has to be static use even if backend doesn't use it, for portability. Can't define how dead code elimination works. Or, could, but don't want to.
*   KN: agree.
*   DM: sounds good.
*   DN: need a proposal. What can you do with them? Can you form expressions out of specialization constants? spirv lets you do compositions, etc. We'll see what's possible.
*   DJ: who wants the AI for a proposal?
*   DN: I'll do it.
*   MM: what else do you have in mind aside from plain globals?
*   DN: scalars. Can compose vectors, structs, matrices, etc from them. Use regular operators. A spec constant is a flavor of constant, so you can use it anywhere you can use a number of that type.
*   TR: might be complex when you can size arrays with it. DXIL can't normally do this.
*   DN: yes. I'd like to take the set that makes sense.
*   TR: useful of course. Adjust size of group shared array. Important uses. Not trivial.
*   MM: another solution: tell authors to use unbounded arrays, and say WGSL can't size arrays with spec constant.
*   DN: #2 use in spirv is for sizing arrays. #1 is for sizing workgroup in shader. Can leave it off if it's not viable in the other APIs.
*   DM: only specific cases where you can use unbounded arrays.
*   MM: maybe the only thing left is global variables then?
*   DN: then where do you use those variables? Can use them in comparisons inside code.
*   MM: OK.
*   DJ: we'll look forward to the proposal.
*   DN: in Metal you always pass in the spec constant from the outside?
*   MM: yes. At pipeline creation time we could parse out the constants, and supply any that the author didn't supply themselves.
*   DN: I like the shader having a default value.
*   MM: sounds OK.


## Out-of-memory for buffer mapping [#872](https://github.com/gpuweb/gpuweb/issues/872) (Austin)



*   CW: can get OOM for [interprocess] shared memory.
*   AE: on some platforms, the mapping can be the same as the GPU buffer allocation. Cross-mapped between processes. On some platforms we can't do this - two allocations (GPU buffer allocation, mapped staging allocation). How do we surface OOM errors?
    *   Call mapAsync, reject Promise
    *   Create buffer with mapAtCreation=true, then call mapRange synchronously
*   On Github, realized it's persistent - mapWrite multiple times returns same contents.
*   So mapUsage could mean you allocate both staging and GPU buffers. If either has OOM, then whole thing is error buffer.
*   Other concern: mapAtCreation=true, getMappedRange synchronously, not guarded by Promise, so could return null or empty ArrayBuffer.
*   Also similar, writeBuffer/writeTexture, impl can fail to allocate staging memory.
*   DM: why can't we raise exception at createBuffer with mapAtCreation=true?
*   AE: thought exceptions were for programmer errors. More that you allocated too much stuff.
*   KN: could be an exception. Fuzzy how it should be exposed. It's a case where large allocation should have to handle this. Checking for null is less wieldy than checking for exceptions.
*   AE: sounds workable.
*   KN: writeBuffer/Texture - if we run out of shmem we can put into renderer process memory. Less fragile.
*   JG: or just Error.
*   KN: would be nice if they didn't error as often.
*   JG: we have ot handle the case where it doesn't work.
*   AE: renderer process already has ArrayBuffer, existing allocation, and if browser runs OOM it can do multiple allocations.
*   KN: can issue part of copy, flush (frees mem), returns that memory.
*   JG: can fail to allocate staging buffer in some process along chain. Has to be able to fail. Since it has to be able to fail, need to figure out how it fails.
*   CW: allocating large ArrayBuffer can throw RangeError exception.
*   JG: problem is that exceptions have to be synchronous. Want something involving a Promise.
*   AE: isn't GpuQueue.writeBuffer synchronous?
*   JG: then staging allocation on renderer process could fail too. For writeBuffer, have to sync copy into staging buffer, could accept on that. Shove to renderer process, to get it on device-local memory might have to allocate staging buffer, that allocation might fail.
*   AE: could use Error Scopes maybe.
*   DM: impl of createBuffer with mappedAtCeation and writeBuffer are very similar on our end. OOM conditions can be handled the same way. they both have the problem Jeff pointed out of failing to allocate staging buffer on GPU process.
*   RC: either throwing exception, rejecting promise, or error scope is fine with me. Exceptions on main thread or web worker can run OOM at any time. Can't assume they're the only program running on the system. I'm fine with all of these error signaling mechanisms.
*   CW: isn't device loss the right mechanism for this?
*   JG: not for OOM. It's an approach Chrome's WebGL backend used for a while. Then you can't handle memory constrained situations. If you ask for too much memory your whole context gets destroyed.
*   CW: that works for resources. If you want to be careful of OOMs you can't use writeBuffer/writeTexture. Have to do your own staging, that's OK.
*   KN: the more places we hae in the API where we could get OOM the more complex the API becomes. If some OOMs result in device lost - it's just one thing the app has to handle globally that they already handle. Where are the places where apps should have to handle OOM? Buffer / texture allocation. Less clear for writeBuffer.
*   RC: for allocating large things, fine to explore OOM and recovery, but for tiny things like shader module creation, if GPU process is running low on memory you're doomed anyway. Fail fast, device lost.
*   JG: that's what we do in our WebGL impl. Check for OOM on buffer / texture allocation, but any uploads into buffers we don't check for OOM.
*   CW: concern about writeBuffer/Texture OOM'ing is you don't know what part of it failed. Maybe some worked, some not. Also resource creation is something you do on level loading or similar. Use async error scopes, etc., to know if OOM happened. writeBuffer means "do this now, pipeline it my frame". Can't recover from OOM in GPU process from mid-frame stuff.
*   JG: is it that bad if writeBuffer fails on OOM, you can capture it in ErrorScope if you want, but we don't try to surface it explicitly? Probably OK? Assume it works, and you can debug it if you want by adding ErrorScopes. Expect it to generally work. In WebGL we expect bufferSubData to always work, but not bufferData.
*   CW: OK, so you expect writeBuffer to either completely succeed, or completely fail with OOM?
*   JG: yes.
*   CW: sounds ok.
*   AE: so do we have the case where the staging allocation fails synchronously? Two failure modes, one Exception on renderer process side and one async, GPU process side?
*   KN: think on impl we should try to do everything we can to avoid that. Chunk things up, or fall back to non-shared staging allocation. Do everything to avoid surfacing to user.
*   JG: also, surfacing OOM warning here in the console to the user is helpful.
*   CW: OK, so writeBuffer/Texture surfaces error in ErrorScopes. what about createBuffer mapped? mapAsync / getMappedRange? Do you get RangeError?
*   AE: mapAsync should reject the Promise. mapAtCreation / getMappedRange may fail because we can't create ArrayBuffer that large.
*   DM: wasn't it decided to create regular ArrayBuffer rather than shmem?
*   AE: in Chromium can't make an ArrayBuffer as large as MAX_INT I think.
*   JG: can we have same failure mode as when we try to map something and it's in use?
*   CW: it's a Promise so it gets rejected. getMappedRange for unmapped buffer - don't know if we spec'd what that does.
*   KN: think it is an exception.
*   JG: thought it was return null.
*   CW: throws OperationError.
*   KN: makes sense to me as exception.
*   JG: sure.
*   DM: so for createBufferMapped, if it runs out of shmem we allocate non-shmem memory, if that fails we throw exception?
*   CW: don't think you can run out of shmem specifically, comes out of process-local memory.
*   JG/KN: think you can.
*   AE: sounds good. That sounds like impl detail anyway.


## Copying of depth24plus formats [#652](https://github.com/gpuweb/gpuweb/issues/652) (yes, again) ([summary](https://github.com/gpuweb/gpuweb/issues/652#issuecomment-646917611)) (Kai, Dzmitry)



*   Part 1: Allowing the following copies in core:
    *   depth24plus &lt;-> depth24plus
    *   depth24plus &lt;-> depth24plus-stencil8<sup>depth</sup>
    *   Buffer &lt;- depth32float
    *   Buffer &lt;-> depth24plus-stencil8<sup>stencil</sup>
*   Part 2: Allowing copies which require format conversions ([#652 (comment)](https://github.com/gpuweb/gpuweb/issues/652#issue-588761676)):
    *   Buffer &lt;-> depth24plus / depth24plus-stencil8<sup>depth</sup>
        *   Is the buffer format depth24unorm or depth32float?
    *   Cross-format T2T copies (probably should ignore this for now)
*   Part 3: (If buffer format is depth24unorm,) allowing copies that zero unused bits ([#652 (comment)](https://github.com/gpuweb/gpuweb/issues/652#issuecomment-644349841)):
    *   Buffer &lt;-> depth24plus / depth24plus-stencil8<sup>depth</sup>
*   Part 4: Allowing copies which (may) require clamping float values ([#789](https://github.com/gpuweb/gpuweb/issues/789)):
    *   Buffer -> depth32float
    *   Buffer -> depth24plus / depth24plus-stencil8<sup>depth</sup>
*   Part 5: Proposed formats and extension formats
*   KN: this is about copying, and maybe writeTexture too.
*   KN: Part 1 is about things that should be supported and have fairly clear answers.
    *   There are a bunch of reasons below why these are the only ones in the list.
    *   RC: for buffer kind, how big of a buffer do I make?
    *   KN: same as any color copy.
    *   RC: do I save room for 24 bits or 32 bits of depth?
    *   Note that the last copy in part 1 above from is only stencil aspect of D24+S8.
    *   KN: if no concerns, we'll move on.
    *   All agreed: we do this.
*   KN: part 2: copies requiring format conversions.
    *   Depth aspect of D24+ formats.
    *   1) could not allow these. 2) allow these, say buffer format is D24unorm, or D32float. Would do conversions into that format.
    *   3) allow users to spec which one they want; seems unnecessary.
    *   Should we allow copies that need format conversions? How useful are these? Don't know the answer to this. Could say "maybe later" if we don't have an answer right now.
    *   DM: if users really want it they can do it via an intermediate buffer. Copy from buffer to other format texture, if we define those paths.
    *   KN: possible, have that here, was ignoring it. Maybe there's an argument to allow those copies and not copies into buffers. Not sure how useful.
    *   KN: if these are sampleable they can implement this by running a shader. May not be that imp't right now.
    *   CW: seems easiest to say we disallow this for now. See users' feedback.
    *   KN: SGTM.
*   KN: part 3: probably irrelevant unless we do T2T copies.
    *   About what happens if you do copies into buffer. Fill in zero stencil bits? Skip over this, talk about in the future.
*   KN: part 4: copies which may require clamping float values.
    *   Ignore second one.
    *   Copes from buffer into D32F. Have to clamp these values into range. On some APIs out-of-range values cause undefined behavior. Question: should we allow copies from buffer into D32F?
    *   Possible for users to do this with a shader by doing a render pass, read from storage buffer, write into depth value of fragment.
    *   RC: first one - buffer to D32F - seems it should be allowed because the browser won't have to do conversions.
    *   KN: we still have to do clamping to get the values between 0..1.
    *   RC: is that the same for other FP types like red textures?
    *   KN: no. Only D32F.
    *   RC: what are consequences of violating that?
    *   CW: undefined behavior.
    *   RC: if we have to clamp we have to clamp.
    *   KN: should we clamp, or disallow it?
    *   DM: clamping / conversion is extra work, users can do it. Can we defer decision until MVP?
    *   KN: think so.
    *   RC: SGTM.
*   KN: part 5: filtered out everything about extensions and proposed formats.
    *   5 of these.
        *   x-stencil8
        *   depth32float-stencil8
        *   depth24unorm
        *   depth24unorm-stencil8
        *   depth16unorm
    *   KN: feel free to read the full-length comment.
    *   DM: pretty sure we agreed on the stencil8 one.
    *   KN: think so. Doesn't complicate things.
*   RC: we may have to revisit all of this if it turns out that depth24+ turning into depth32f means that some content doesn't work because they need the higher precision.
    *   KN: hardware that can't support d24unorm is probably just mobile. If we standardize on d24 then people won't realize they have less precision than expected.
    *   RC: understand it's hard problem. Don't have better solution.
    *   KN: can have development mode which reduces precision of 32-bit depth values somehow.


## Development-only API features ([#814](https://github.com/gpuweb/gpuweb/issues/814))



*   KN: need to decide whether we want this class of features. Features / extensions that have API surface, show up in GpuAdapters features list, but only available if checking some flag in DevTools, etc.
*   KN: concern about detecting whether DevTools is open, but had in-depth discussion about that before.
*   MM: if hidden behind flag, doesn't need to be standardized.
*   KN: correct, but need to make decision. Informs what we need to standardize. List of things that might go into these. Pipeline stat queries, high-precision timers, synchronously thrown errors, other fingerprintable things. If we have this class, maybe pipeline stat queries would go into a separate doc.
*   DM: would like main doc to have things that are portable. Imagine we could have separate doc with dev specs.
*   MM: separate doc makes sense. Having that second doc be part of the WebGPU CG deliverables doesn't make sense.
*   CW: think it makes sense to do it here. Good thing that they work the same way across all browsers and have CTS tests. There's value in that. This group is the right place to standardize. Maybe not W3C recommendation.
*   MM: if behind a flag, what's the point of standardizing?
*   JG: the point is if we wanted to expose high-precision queries, and multiple UAs wanted to tie this into the debugging experience.
*   MM: they can do that but it's not part of WebGPU.
*   JG: it is though. Agree charter doesn't have this as a deliverable.
*   KN: point of dev-only API features is to allow applications to use that APi surface. They don't show up just in DevTools. Having some form of standardization is valuable. Three.js could have debugging interface for this stuff. Useful if that ran on multiple browsers. Should have some form of standardization even if not very strict.
*   MM: another way to do this is to have Three.js' developer tools interact with DevTools.
*   CW: thought you can't tell whether DevTools was open.
*   KN: you can write DevTools extensions.
*   JG: We’ve talked about before that you can do various things to tell whether some parts are being accessed. Like if source maps are fetched. So I don’t think we should read the W3C tag so broadly immediately. It is a different issue whether or not that standardization happens here. It makes sense for it to be here as WebGL extensions are in the WebGL group even though not all UAs intend to implement them.
*   KN: There’s a separate question of whether we should even have such features. Some of them we would never want to expose normally, but others like pipeline statistics queries.. it’s not absolutely necessary to put it behind a development-only flag. Could be a normal extension on a case-to-case basis that’s in the spec. Particularly relevant for pipeline statistics. 
*   KR: Based on WebGL, some extensions were discovered later to be too powerful for ordinary extension use. The specs were updated to say they would no longer be generally available. Makes sense to do something similar here. Maybe these could be specified as an enable-able feature when you create the adapter.
*   RC: Is it still the case in some browsers that if devtools are attached your code runs slower b.c. JIT doesn’t run or code is interpreted?
*   KR: May still be the case, but these are bugs we should file and fix.
*   KN: Devtools has perf tools so if you’re running those it shouldn’t get worse.
*   RC: Okay. If some only work in dev tools, then we should be able to lie about devtools being open. Some content could detect you’re running devtools and just stop working.
*   CW: That was the concern about why we can’t have it, but if it’s an explicit step to check a checkbox, pass a flag, or something to enable, then it’s the same as putting a breakpoint and the JS library noticing your page was paused for 10 seconds.
*   MM: I think it’s worth.. why are the members of this group even here? What are we trying to standardize / accomplish? If a browser wants to ship a feature to fingerprint users, they can just do that. The reason Apple is part of the group is to ensure whatever we come up with doesn’t allow fingerprinting or make privacy worse. If the question is should we standardize APIs that invade privacy, we should say no.
*   JG: I don’t think anyone is really talking about that. We are here to standardize cross-compatibility across browsers. There’s interest in cross-compatible APIs that may not be exposed to the general web.
*   CW: Nobody in this group wants WebGPU to force browsers to implement additional things that could help fingerprinting or privacy invasion, etc. These extensions in particular are very much optional like other features in WebGPU, and we see value in exposing them if the UA wants them to be exposed for its own purpose like debugging. This is not privacy invading because the user explicitly opts into it. Second, for the Three.js example, there’s value in having the extensions, if available, match between browsers.
*   DJ: If possible, the way to do this would be to expose the API from within the developer tools, not because they’re open.
*   JG / CW: that’s what we have proposed.
*   CW: If you open the dev tools AND click a checkbox that enables the development-only features..
*   DJ: I mean the JS api is only callable from the dev tools. Like console.trace.. etc. There are ways browser vendors are collaborating on loading scripts from within dev tools without changing the web-facing API.
*   KN: I think this is really complicated here. In order to implement pipeline statistics queries in this way would be to inject scripts into the page to shim the entire API to add additional entrypoints which it then reads back in the devtools extension. It would have to forward all the commands into devtools and run all WebGPU in devtools instead of the web page. That is very invasive, and we should not require such a thing.
*   DJ: It’s not as invasive as I think..
*   KR: It’s pretty hard to shim WebGL..
*   KN: Not just talking about a shim. I’m saying we have to take ALL the WebGPU commands, forward them to another process, and execute them, and forward them back. This code has to run inside devtool
*   CW: Would it help that instead of a checkbox you have to call GPUDevice.enableDevExtensions(), and then the page has access?
*   DJ: That would be fine to me as long as the Web-facing API isn’t changing (page content hasn’t changed).
*   KR: they way WebGL has done this in the past: for potentially powerful extensions, spec them as WebGPU extensions, under the assumption they will be exposed on UAs that can support them and not expose too many bits of entropy, and then if it’s discovered that this is dangerous, we then go back and restrict them. They will be portable between all browsers, etc.
*   MM: Think we’re largely repeating ourselves. When this came up last time, our browser strives to implement the whole spec, and I don’t see the point in standardizing something if browsers aren’t going to attempt to implement it.
*   CW: Will you try to support textureCompressionBC on iOS where it isn’t available? There will be capabilities that aren’t available.
*   MM: The difference is the motivation. If there is a piece of hardware missing, then our hands our tied. This is not the case here.
*   JG: For high precision timer queries, technically nothing is missing from the hardware, but the browser doesn’t have a way to expose it in a safe way. We have a project for it, but until that happens we can’t implement it. Are you asking that there needs to be a horizon for all UAs to implement something?
*   MM: The APIs that all browsers strive to implement should be resilient to privacy concerns, and we shouldn’t spec things that are irrelevant to major implementations.
*   KR: Knowing how long draw calls are taking are highly relevant to developers. They need to optimize shaders and may do it on multiple platforms, using development facilities. They are very relevant and should be standardized.
*   MM: Not objecting to this. exposing timers is just as fine as exposing timers in Javascript. The question is whether they should be high resolution.
*   JG: Right now the answer is you can tell this answer if they explicitly enable them in dev tools. I think we should all move forward with concrete proposals. Don’t think there’s more philosophically.
*   KR: Agreed on concrete extension proposals.
*   DJ: I would add also put thought into whether the proposals could exist purely within the dev tool environment. At least have that as a thought.


## Can we simplify the data uploading primitives to 2 at most? (mapBuffer, createBufferMapped, writeBuffer) (Ken)



*   Skipped - doesn't have a bug


## Follow-up on when GPULimits are enforced [comment on #409](https://github.com/gpuweb/gpuweb/issues/409) (Austin)



*   AE: editors decided that there are certain per-stage and per-pipeline limits. Enforced on pipeline validation. makes sense. Concerned: would like them to be enforced on bindgrouplayouts too. Otherwise can make BGL that exceeds # of per-pipeline layout limits, and that's not an error. Then make BG from BGL, huge, not an error. While making impl pretend this isn't an error and not allocate stuff, would be better to not have this magic inside the impl. Not possible to use such a BG / BGL with a valid pipeline because it exceeds limits.
*   DM: somewhat relevant to usage scopes I've been specifying. Can create BG that violates usage rules, and we're not spec'd to generate an error. Issue Austin raises is different, don't want to go into driver undefined behavior or go above its limits. Seems reasonable.
*   AE: ok, so we should validate this.
*   CW: esp. because BGL creation is cold code, so can put as much validation there as we want.
*   KN: unfortunately doesn't avoid any validation later on, because pipeline creation has multiple BGLs, but think this is a good idea and will simplify impl significantly.
*   CW: if no concern can do that.
*   (all agreed)


## Case for optional GPUBindGroupLayoutEntry entries [#851](https://github.com/gpuweb/gpuweb/issues/851) (Dzmitry)



*   Discussed this day 1. Anything left?
*   DM: don't think so.


## Introducing a GPUEncoderBase (Dzmitry/Brandon) [comment on #785](https://github.com/gpuweb/gpuweb/pull/785#discussion_r428266148)



*   Spec issue, don't think we need to resolve here.


## Fancy render pass attachment load/store/readonly (Kai/Brandon) [comment on #831](https://github.com/gpuweb/gpuweb/pull/831#discussion_r436195260)



*   Spec issue, don't think we need to resolve here.


## Where to store encoder state in the spec? (Kai/Brandon) [comment on #850](https://github.com/gpuweb/gpuweb/pull/850#discussion_r437053808)



*   Spec issue, don't think we need to resolve here.


## Feature Policy



*   RC: how do people feel about putting WebGPU under FeaturePolicy to mitigate fingerprinting? Web page says, I approve this iframe of using WebGPU. Didn't do this for WebGL, but WebXR, etc. do have this thing. "Powerful APIs". What about making WebGPU one of these?
*   KN: different from WebXR. WebXR affects global UI. Probably lot of fingerprinting potential in WebGPU we can't get rid of. Think it's a good idea to minimize that risk by doing FeaturePolicy.
*   CW: feels like base GpuDevice should expose little fingerprinting in addition to what you can get with Canvas2D.
*   KN: agree.
*   CW: ideally the base WebGPU device is available for everything. Example: start to see 3D ads using WebGL. Could use WebGPU with this basic device. Don't need fancy stuff.
*   RC: OK. Curious to hear Apple's opinion. Then I opine that this doesn't switch the adapter.
*   BJ: we did this for WebXR because engaging with those devices is disruptive for the user. Beyond fingerprinting, how people will use this API is good to consider. Remember hearing stat that some large percentage of WebGL usage is people injecting bitcoin miners via iframes. Feature policy would not just do blanket block for any iframe, lets you say "I'll let this iframe use WebGPU", so ad network would have option to say the embedded iframe can/can not use this tool. Can suppress known bad actors. Or you signed up for image-only ad campaign. Puts more control in hands of embedding page. Embedding page may not let you have this feature, and you'll have to deal with that. Like the idea of base-level functionality being everywhere, but I like having the switch available.
*   KN: is there also a top-level control the page using the ad network can control? Thought there was an HTTP header about this.
*   MM: There is. CSP is something else, but pretty sure there's something like that.
*   KR: In WebGL when trying to restrict the high performance GPU, we found that lots of ad networks inject into the page, and don’t use iframes. 
*   KN: expect if we had the HTTP policy tag it'd apply to the whole page but not iframes.
*   MM: why is WebGPU different than WebGL?
*   KN: exposes more powerful features for bitcoin mining. :)
*   JG: interesting question, should we have powerProfile, and high/low/software distinction.
*   MM: canvas 2D is accelerated, too, uses the GPU.
*   JG: not all the time. Only on Windows on Firefox.
*   BJ: you could do software renderer, lets you do the same things. Think it would be good if WebGL had a similar knob. Hard to do this for an API that's been out there for a while. Should be based on how much power the API puts at your fingertips.
*   CW: not clear cut. Talk about this again.


## Explainer



*   CW: we should go for TAG review at some point. The spec as it is is not digestible right now. What explainers should we make for the TAG and people in general? Error handling is non-JavaScripty for good reason.
*   DM: explainers need to be separate docs? Or non-normative sections?
*   RC: explainers are for people who've never written a shader in their life.
*   KN: up to us whether we want to keep it up to date forever. If it's in the spec will have to be kept up to date.
*   MM: should probably go through same review process we've established. If this group produces it we should review it.
*   CW: this group puts WebGPU up for TAG review, or is it a W3C member that puts it up for TAG review?
*   RC: just open issue in TAG's Github repo, point to your repo. People don't know things about shaders, pipeline layouts, bind groups, etc. Should have an explainer in this doc, walks them through, holds their hand.
*   KR: an Explainer.md or similar side-by-side to the spec seems reasonable for that.
*   
*   
*   
*   
*   
*   
*   
*   
*   
*   
*   
*   


## Agenda for next meeting