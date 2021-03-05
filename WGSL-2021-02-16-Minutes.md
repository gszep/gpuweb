# **WGSL 2021-02-16**

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** Dan Sinclair

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** 11am PST

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send Dan Sinclair a Google Apps enabled address and he'll add you.



---



# **📋 Attendance**

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



*   Apple
    *   Dean Jackson
    *   Fil Pizlo
    *   **Myles C. Maxfield**
    *   **Robin Morisset**
*   Google
    *   Corentin Wallez
    *   **Dan Sinclair**
    *   **David Neto**
    *   Kai Ninomiya
    *   James Darpinian
    *   Brandon Jones
    *   **Ryan Harrison**
    *   Sarah Mashayekhi
    *   **Ben Clayton**
    *   **Alan Baker**
*   Intel
    *   Yunchao He
    *   Narifumi Iwamoto
*   Microsoft
    *   Damyan Pepper
    *   **Greg Roth**
    *   Michael Dougherty
    *   **Rafael Cintron**
    *   Tex Riddell
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
*   Kings Distributed Systems
    *   Daniel Desjardins
    *   **Hamada Gasmallah**
    *   Wes Garland
*   **Dominic Cerisano**
*   Joshua Groves
*   Kris & Paul Leathers
*   Lukasz Pasek
*   Matijs Toonen
*   **Mehmet Oguz Derin**
*   Pelle Johnsen
*   **Timo de Kort**
*   Tyler Larson
*   Eduardo H.P. Souza



---



# **⚖️ Agenda/Minutes**


## Meta


### Office hours call



*   JG: No time set yet, will do this week.



---



## Timebox


### [Entry point arguments and return values #1426](https://github.com/gpuweb/gpuweb/pull/1426)  



*    DN: Looked at PR today and overall looks good. Some terminology things, but don’t want to block but seems like the right thing. Improvement and 90% of the way there.
*   MM: Agree
*   JG: Ship it.
*   DN: Ship it.
*    DM: Land and followup later?
*   MM: Land it
*   DN: Land it.
*   


### [Remove the requirement to have entry points #1340](https://github.com/gpuweb/gpuweb/pull/1340) 



*    JG: Having trouble getting consensus but maybe able to push out past MVP and focus on other issues, sound find?
*   MM: Try for consensus?
*   MM: Think DM has made a fairly compelling case why it’s valuable. Haven’t heard a compelling case about why to not allow this.
*   DN: What I’ve come to understand is disconnect on when we should be able to gen lower level code from WGSL. ShaderModule or PipelineCreation. If we should put features in the language which require waiting until pipeline creation. Against that, want features to align so we can validate as soon as possible (createShaderModule). Fundamental disconnect. Comes up for extensions being per entry point or per module. Resolving that may point this in the right direction
*   MM: By “generate as much code as possible”, are you talking about SPIR-V or all browsers on all OSs?
*   DN: Should cover as much as can be exposed. Some won’t be as easy as others, HLSL needs deferment, DXIL maybe able to do that easier. Would be fine to have that for HLSL if DXIL can generate it earlier.
*   JG: For this request, could a potential implementation assume we could generate SPIR-v at shaderModuleCreate, we could generate all the SPIR-V needed at that time, but if you don’t have any entry points the SPIR-V for the pipeline is the empty string. So, you’re generating at shader module creation, but impossible to use.
*   GR: Seems like motivation is to validate the shader, but a lot of authors pack a number of shaders into a single src file, the entry point determines which one you’re running. Can’t really be validated because there are things that aren’t valid for a given shader stage. How you’d remove dead code, etc.
*   MM: Expect validation of shader stage specific code to state the rules as if a barrier is reachable from a compute entry point then that is a compile error. If there are no entry points then you can put barriers where you want.
*   GR: So, the compiler has to trace all the call stacks to determine if any end in compute shader
*   JG: Maybe, but no entry points no trace
*   GR: But apart from that.
*   DM: Yes.
*   JG: One of the objections is that you can’t use the shader without entry points. If the issue is you can’t create the shader module as there is no valid SPIR_V. Then the SPIR-V is just no SPIR-V. That’s all that’s needed. Potential way to implement this if you wanted.
*   DN: I can’t use a module that has no entry points. THere is no linkage module, no way to glue wgsl srcs after given to implementation. We can make a pipeline from the stage, but that must have had a shader in it. Don’t want to spec for code that i’m going to dead code eliminate.
*   MM: Dead code still has to type check.
*   DM: You could have one entry point with lots of issues, but won't’ reach them from that entry point
*   DN: I can’t use the thing you want to validate, what’s the point
*   GR: Can’t validate a shader without an entry point
*   JG: You can fully validate. There are 2 phases for validation, 1 type checking, 2 entry point validation. Only things used in frag entry point work in fragment. You always do the first, the second for entry point behaviours loop over the entry points. Exit immediately as you’ve done them all and all (0) entry points validated successfully. Question is if the user wants no entry should we allow the shader module for it even though it can’t be used in a pipeline.
*   GR: Are we introducing a pitiful that users can fall into given the intended use. Don’t see it that useful, it’s probably harmless, until we get complaints that the pipeline won't create.
*   MM: Thought experiment, consider a function that is never called but has a type error?
*   GR: You can validate that with an entry point? Not really validation, if that falls under the definition that’s fine
*   JG: Folks want to talk about whole shader module validation. If they want that as a prerequisite then that’s OK.
*   DM: Agree, what’s missing is the use cases. When would the user want no entry points. Not when they write the shader, but when they have multiple entry points but filtered them out. Preprocessor or something that filtered out. If they’re filtering out the entry points then filtering out the pipeline creation. But forcing them to do the extra checks if entry points factored out then redundant
*   AB: Just sounds like a different error place. Why not just get a null back from createShader?
*   JG: Difference is functional. I generated all shaders for this module and if i need them i’ll create the pipeline, but never create the pipeline so never use the shader module. If you mark that as illegal then you have to move back up and decide how to handle the missing shader module. Create a fake shader module in SPIR-V.
*   MM: Like a zero input/output compute shader that does nothing? \
JG: Or system with different entry points per lights and use 0 lights, so create no entry points. Does this shader module fail to create if it never uses it?
*   MM: Interested in making the shader module return null? That maybe OK, could be a middle ground. You’re allowed to have this function return null, but not expose an error.
*   JG: Would match SPIR-V.
*   DN: Imagine that is the billion dollar webgpu bug. Expect that to trip folks up. Making a choice on what’s more likely to trip up on, and will we guide them the right way of an error vs un-usable object.


### [indexing is 0-based, and array size is at least 1 #1410](https://github.com/gpuweb/gpuweb/pull/1410) 



*    JG: Just needs review, has PR. Sounds good, just extra spec text
*   MM: Looks good, should merge.
*   DN: Will fix typo.
*    


### [Restrict contents of array and struct #1411](https://github.com/gpuweb/gpuweb/pull/1411) 



*    JG: A PR, seems like folks are ok.
*   MM: Reviewed earlier, looks good to me.
*   
*    
*   
*    


### [Atomics proposal · Issue #1360 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1360) 



*   (initial socialization)
*    AB: quick overview, after looking at other languages, came up with this proposal. Comment from MM that this may want to wait on pointers. Important part is the usage example, other than that, adding a new type to facilitate translation to metal which is more restrictive than HLSL or SPIR-V.
*   MM: Appreciate that the type was added, and was bracing for an argument. Interested in the allow_uav_condition. Can some describe where it’s needed and what it does in HLSL?
*   GR: Not sure.
*   MM: Don’t think it’s urgent, but could delay. GR can you take an AI?
*   GR: Who added it?
*   MM: It’s an HLSL language annotation?
*   GR: No it isn’t, it’s to account for something in HLSL but don’t know what it is.
*   AB: Listed in note in compareExchange. So brought it over in the survey but don’t know what it’s for
*   JG: Looks like an HLSL thing. \
DN: Listed in the for statement as a qualifier on something. Loop shader termination condition to be based off of …. (see link)
*    [https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-while](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-while) 
*   GR: Not familiar with, will look into it.
*   MM: Would be valuable to know more. Notwithstanding, I think the proposal is great and should land ASAP.
*   JG: Prioritization vs other stuff?
*   MM: Think the cost benefit trade offs are in proposals favour. All 3 APIs roughly agree, so impl isn’t super hard, seen in a lot of shaders. Think they should be in v1.
*   DN: Brandon was converting examples and barriers and atomics cropped up pretty quickly in compute shaders. Atomics for compaction rounds. A fundamental building block.
*   MM: You’re on a parallel processor, atomics are useful. 
*   DM: Is that in tint yet?
*   DN: No we wait for spec first.
*   DM: Looked at the proposal, seems reasonable, maybe more feedback once implemented.
*   JG: Portability, repeatability problems? Question is just the reality we’re stuck with here, I’m only bringing up for completeness. Will cause bugs that don’t reproduce.
*   MM: Agree but the cost/benefit tradeoff is for this. This makes it easier to write non-portable algorithms, but allows writing algorithms you couldn't write anyway.
*   JG: Maybe we need tsan for WebGPU.
*   DN: None of the APIs are good about details like forward progress and this lets you play and discover. Agree with MM’s comments.
*   JG: Sounds good.
*   DM: Do these need to accept a pointer to an atomic?
*   AB: Still open for discussion. Written this way because to me it was important to spec no copying was being done, has to be a ref of some kind. Want to clarify ptr or ref is fine, but not in spec yet.
*   MM: Propose we get started with anything for the function signatures. Get it ready, put in spec, run with it and when we figure out ptr/ref we come back and adjust. Don’t think the exercise of adjusting the proposal will be super hard.
*   DM: So, you want this to be a type, but the type would make no sense if it’s used in a struct that is local to the shader? It only makes sense for storage buffers/ \
AB: Workgroup and storage.
*   MM: Correct. In metal you can’t take an atomic op and pretend it’s an int. You can’t add 5 to it. Only thing you can do is the set of atomic functions.
*   DM: If only used in those blocks or structs, don't we need to make an attribute of the field instead of part of the type?
*   MM: Depends on how ptrs end up working
*   DM: I mean how we declare them, could it be an [[atomic]] decoration for a field.
*   AB: Goes with if the pointer is passed to a function do you carry the decl?
*   MM: If i have a helper that takes a ref to an atoci how do we represent
*   RM: Need to make clear what’s an atomic and that the loads and stores don’t look like normal loads and stores
*   MM: +1
*   DM: If it’s a type, can I declare a struct with these types in them? \
MM: In metal you can, you can call functions on them
*   JG: On the struct?
*   MM: No, on the field.
*   AB: One minor point is the question of volatility. Good to say they translate to volatile for now, and add a keyword later. Would add that now that they do, metal requires that now
*   JG: Says we’d have to translate it on metal, but not elsewhere. So, implementation detail. Are you suggesting we allow users to set volatile.
*   AB: Yes, either need to make it volatile or let users set it
*   MM: Vote we don’t add volatile
*   JG: Could be part of our atomics stuff
*   AB: That’s fine too.
*   JG: Needs specification.
*   MM: Will we do the thing DN suggested about atomic_load and store and polyfilling on SPIR-V
*   AB: HLSL you need to fill them all
*   JG: Idea has a big +1. Seems like a win to just give folks the polyfill unless there is extra cost. If you want to do an atomic load on HLSL this is how you do it, and we can just do it in the builtin.
*   MM: Think it’s valuable for complex math ops that can’t be expressed as an atomic. You want to do a load and store and make them distinct but as a type you can’t just pretend it’s an int and do the math. So, I think it’s valuable.
*   


### [Barriers proposal · Issue #1374 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1374) 



*   (initial socialization)
*    JG: Sounds useful if you have atomics, but maybe useful otherwise
*   AG: Workgroup storage it’s useful, do preprocess and other invocations can read
*   MM: Scatter gather without atomics
*   RM: Issues about this topic a few months ago. How does this proposal match the thinking at the time?
*   AB: DN pointed at the issues and tried to account, but failed to link the right issues into the pull request.
*   MM: Angle brackets.
*   JG: Schedule for next week? Is it germane to the issue here?
*   MM: I think it can be adjusted as a post-process.
*   JG: Inclined to want to see spec for this as well.
*   MM: Think this is a good proposal. Exactly matches the barriers in WHLSL.
*   JG: Do folks want more time to think, or needs spec?
*   DS: Needs spec sounds good
*   JG: We’ll do that 


### [Float literal scientific notation? #1432](https://github.com/gpuweb/gpuweb/issues/1432)



*    JG: On the prioritization spreadsheet. 3e8 is the speed of light. Should we have it?
*   AB: Thought this was already in there, and hex float was accepted but not spec’d. 
*   JG: Sweet, then done. If we can point these at the issues where they’re being tracked we can put in the spreadsheet that we’re tracking them.
*    


### [Float hex literal notation? #1433](https://github.com/gpuweb/gpuweb/issues/1433)  



*   MM: Are we going with the grammar c uses? I hope so
*   AB: That’s what the PR does
*   MM: So you copy / pasted C
*   AB: Pretty much
*   MM: Thumbs up. 
*   


### [does textureSampleCompare assume level-of-detail 0 in non-fragment shaders? · Issue #1319 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1319) 



*    (tabled)
*    



---



## Discuss


### Priorities and Agenda for VF2F



*    JG: Next week. VF2F. Bummer no webgpu meeting to talk scheduling, so don’t remember what they were talking about but can put together a list of the things to talk about. Have pointers and refs, said will do proposal, will do for next week. Will figure out what we’re doing with points. Hoping we can figure out how to get to the point where we have an immediate path to MVP for WGSL. Sounds like almost there when I go and look through priorities doc and project card columns. What else is on folks minds that we need to discuss. What do we need to figure out for MVP in particular shaders you write today may not work post-MVP and want to get to MVP where your shaders will work moving forward
*   [https://docs.google.com/spreadsheets/d/19jeGI3uLwVQryUmW12rxaWJe4FTbFBwVi-uEGdwhLQ8/edit#gid=0](https://docs.google.com/spreadsheets/d/19jeGI3uLwVQryUmW12rxaWJe4FTbFBwVi-uEGdwhLQ8/edit#gid=0) 
*   DN: Function call and pointer parameters. Think there is a path forward, needs to be written down. Overall, in pretty good shape
*   AB: Vertex and frag shader interface matching.
*   DN: And IO location counting.
*   MM: More to discuss about uniformity analysis or done talking about that and needs to be written?
*   DN: Assumed we’ll take what RM did and port forward to syntax we have now. Someone needs to write words
*   MM: RM agree?
*   RM: Said I’d do it and still plan to do so. See no difficulty with it, just have to take the time and do the work.
*   DS: Buffers, size/align, etc
*   DN: BC is writing spec text for change, may not be ready for next week, but we’re all in agreement
*   DM: Could talk about array strides
*   MM: Would like to talk about generating metal source at create shader module time. Probably tangential conversation about what gets validated when.
*   JG: Relatively large discussion about validation and what that means for when you can create things and extensions and how that’s going to work.
*   MM: Right, got to nail that down.
*   AB: Using specialization constants on attributes for things like workgroup size is important for compute.
*   MM: Thought that wasn’t possible in one of the APIs.
*   JG: Always polyfillable. At least consensus it isn’t an MVP thing
*   DN: Couldn't remember.
*   JG: Feel like we could ship without and be sad
*   MM: Have to be literals in HLSL and possibly other languages, so good enough for us for MVP
*   AB: You get a preprocessor there, but don’t have one in WGSL.
*   JG: Javascript has a template strings. Other stuff that folks writing WGSL have run into and would like to change, good question for Brandon.
*   DM: Would like clarity on block decoration, left unresolved for now.
*   MM: For fp16 do we all agree how they should work or is there discussion that needs to be had?
*   JG: Think we need a proposal
*   AB: What about numerical precision in general
*   JG: Very related.
*   MM: Is there discussion to be had, can’t we just say as strict as it can be?
*   JG: THink we just have to say that.
*   DN: That’s a large barn door that’s open.
*   MM: What I meant was, can’t we do research and find, out of the 3 apis whatever requirements are most permissive, and put those in WGSL. So, the spec will have numbers but are the most relaxed numbers.
*   JG: Suspect the specs, like hte GLSL module for SPIR-V, when you call sin it will give you an approx of sin without air bounds.
*   AB: Not sure with D3D but vulkan is reasonable in describing precision. Less than other things, but not terrible.
*   MM: Compatible with what I said, for some operations the most restrictive we could be is no restrictions
*   JG: WebGL is similar, you get what sin gives on the platform and if its’ bad we’ll polyfill. No other requirements. Have CTS tests which test trig and builtin functions. Must come within bounds and if they’re out of bounds Angle polyfills them
*   MM: So map from driver -> Polyfills
*   JG: Yup. And the CTS says what it means to be 3 parts per byte off.
*   MM: 3 ULPs
*   JG: On a 256 uint.
*   JG: Other things useful to discuss, subgroups pushed post MVP. Do we want to talk about image blocks and tiled rendering? How sad if we don’t have a solution? \
MM: Not too sard. General philosophy: don’t want single vendor extensions. At least the direction we’d like to go.
*   JG: Hope we can find a way to surface this in a useful way in all backends eventually. Fine to not ship in V1.
*   DN: Agree with that. Would like a smaller thing to ship earlier and iterate. Smaller steps and faster feet.
*   AB: With subgroup if they were uniform subgroup ops maybe able to spec something
*   MM: Had that discussion, but there is a gpu where we want to run WebGPU which has a different behaviour for uniform subgroup operations. Found the same behaviour in M1 GPUs but that’s a bug.
*   GR: There are a lot of bugs in implementations
*   DN: No spec, no bug.
*   JG: Main thing left in the prioritization doc is issues Apple added about little bits of WGSL that could be nicer and we could try to get through some of them. Some aren’t that important some are high priority from Apple
*   MM: Most important is 2-pass compilation issue.
*   JG: Will call that one out. Feel like I’d be positively bad without multiline comments.
*   MM: Our thought for things like brackets for array types. Not super important but getting right from the beginning makes everyone happier.
*   JG: Will try to burn through the list and see where it fits on the roadmap next week. Should be quick.
*   MM: Normal call is cancelled?
*   JG: Yes, F2F overlaps with the WGSL call. Consider this call cancelled and we’ll do the F2F instead.
*   MM: Did we get to a conclusion on whether WGSL needs a plan for expansion issue?
*   JG: Feel like part of the validation topic. But, no, I don’t think so.
*   DN: On next meeting agenda is extensions? Which I think is the same thing
*   JG: That would also include future versions. Anything we wanted to add that would not be backwards compat. Broad topic, but something for next week
*   MM: Namespaces? Are we done talking about namespaces
*   DN: Could be part of the solution
*   MM: So discussion is not just about extensions then.
*   JG: Right. Depends how we feel like we’re going to solve it will advise how we end up needing to solve it. If we don’t solve it, don’t need namespaces, but if namespaces are how we solve it then namespaces become p0.  Will collate and gather into next weeks agenda document.
*    
*   



---



# 📆 Next Meeting Agenda



*   VF2F next week!
    *   Ptr and refs?
    *   Extensions?
    *   Pipeline-overrideable workgroup size: [https://github.com/gpuweb/gpuweb/issues/1442](https://github.com/gpuweb/gpuweb/issues/1442) 