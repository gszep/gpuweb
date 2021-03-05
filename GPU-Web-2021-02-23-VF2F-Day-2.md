<!-- Output copied to clipboard! -->



# GPU Web 2021-02-23 VF2F Day 2

Chair: Corentin (WGSL = Jeff Gilbert, David Neto)

Scribe: Austin, Corentin, Kai, Myles, Ken, (...others, add yourself)

Location: Google Meet


## [Agenda / Minutes for Day 1](https://docs.google.com/document/d/1Csyz-MF9u2iw7WmjENTCr4BUbxnTgPk9l0u7ux6DSL4)


## [Agenda / Minutes for Day 3](https://docs.google.com/document/d/1EaCJCXsD-uhbmpBnRqPiemmM9V965yRhoE6yboceO9A/edit#)


## Tentative agenda



*   WGSL
    *   Barriers #[1449](https://github.com/gpuweb/gpuweb/pull/1449)
    *   Ptr syntax again - #[1456](https://github.com/gpuweb/gpuweb/issues/1456)
    *   Extensions again - [#600](https://github.com/gpuweb/gpuweb/issues/600)
    *   More of workgroup size specialization [#1442](https://github.com/gpuweb/gpuweb/issues/1442)
*   API
    *   Shadermodule creation
    *   Texture initialization woes
*   First public working draft (last hour, with dean)

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
    *   Ryan Harrison
*   Intel
    *   Yunchao He
*   Kings Distributed Systems
    *   Hamada Gasmallah
*   Microsoft
    *   Greg Roth
    *   Rafael Cintron
    *   Tex Riddell
*   Mozilla
    *   Dzmitry Malyshau
    *   Jeff Gilbert
*   Chas Boyd
*   Francois Daoust
*   Francois Guthmann
*   Matijs Toonen
*   Michael Shannon
*   Mehmet Oguz Derin
*   Timo de Kort


## Barriers [#1449](https://github.com/gpuweb/gpuweb/pull/1449)



*   DM: We may eventually want barriers on texture, and we would have to introduce a storage class just so you can do barriers. This is a little weird, on top of the fact that the suggested syntax uses variable templates. It’s irregular to do that, and we don’t need to introduce the new syntax.
*   AB: We can. I think the first proposal for this was something like a bitmask, but we don’t have constexpr. We can do that or have multiple parameters if this is not the most palatable grammar. I can understand that. I don’t think there’s argument on what the barrier is doing, so it’s mostly the syntax/
*   DM: Can we introduce at least a limited constexpr?
*   AB: I think you would have to describe most of the rules around constexpr. We could also make it take a single constant value, and we define what the values are and what they mean - effectively defining a bitmask.
*   MM: Why are there angle brackets at all and not a bunch of distinct functions?
*   AB: I had mentioned this in the proposal issue, but when we discussed in the WGSL meeting, it didn’t seem like anyone was pushing for that. We could do something similar to HLSL though where they name all the different types of barriers in the function name. allMemoryBarrier(), sharedMemoryBarrier() - workgroups, deviceMemoryBarrier() - buffers, textures;
*   DM: That seems like the most straightforward solution. When I was thinking about it I was wondering if there’s a perf cost to consecutive barriers vs combined.
*   DN: There’s actually a correctness problem if you don’t combine them. workgroup and pipeline barriers can (reorder?) themselves, so sometimes need to be combined.
*   AB: Not sure if that’s true. It’s different if you put operations between the barriers, but the consecutive ones should be ok.
*   DM: What do HLSL programmers do. If they need more than one, do they just use All ?
*   AB: I would guess so. Usually it’s just device stuff or workgroup stuff. There is not a diverse set of combinations. You don’t get texture barriers separately. If we had that eventually, the HLSL translation would be overkill.
*   MM: If we need every combination, then angle brackets are not right. Probably a single function which takes an integer parameter, and each bit is a barrier.
*   AB: Sure and we can make it constexpr.
*   RM: Seems fine. We have other places in the grammar where we take a constant integer. I guess we … function-like syntax, so people can’t pass something arbitrarily.
*   AB: I guess it would have to be a parsing / validation error.
*   MM: Oh, I see. If we want to say the function is magical and you can only pass a 1, 2, or 4, then it really shouldn’t look like a function.
*   AB: 1,2,4 or OR of those values.
*   MM: Right, but if you have a helper function that returns an int, and you want to disallow passing that value into this barrier function.. we shouldn’t allow that.
*   JG: We already have places that have to take constant ints.
*   DN: Like the constant offset on a gather.
*   DM: We’re already there; it’s not going to change anything.
*   MM: SO we need a real concept of a constexpr. 
*   JG: There’s general consensus for it, but we’ve kicked spec down the road.
*   MM: I guess another way to say it: if we have constant offsets, we already have a constexpr. It’s just very simple.
*   DS: If we’re taking constexpr, does that mean HLSL backend will need to parse the enum apart and generate a bunch of commands..? It’s not hard, but something we need to take into account.
*   JG: Yea.
*   AB: Need to do that for Metal too.
*   DS: It’s easier to parse the template and just look at the names.
*   JG: I think I see what you’re getting it, but it seems like it’s more of a concern about it being difficult to do the constexpr thing. I think we need the constexpr thing for other reasons. If we need to handle this, it’s a thing we can do.
*   DS: For the full constexpr route, this makes sense, but originally we weren’t discussing this.
*   AB: for constexpr, you also need to be explicit about if this includes specialization constants. Those wouldn’t be fold-able, so some const expressions wouldn’t be allowed everywhere.
*   JG: I think that’s okay. They’re a different class of const-ness.
*   BC: I thought we were proposing before that constexpr was going to be a specialization thing based on the offset.. if we do this we need to specify
*   DN: We already have 4 points at which values become fixed.  We might have const-expr flavours for each of those.
*   JG: See that.
*   AB: Other option is to continue to kick this, add HLSL-style naming, and add a unified barrier in the future if we’d like to.
*   JG: I would be okay with that. If we have vestigial sharedMemoryBarrier and later it’s just memoryBarrier(SHARED), I’m okay with that.
*   DM: So we agree we need constexpr, and this solution would work with constexpr, can’t we just have them in a minimal form?
*   JG: Sort of inclined to wait for constexpr, and if we finish that before v1 we can change barriers, and that’s fine.
*   DN: Having a computed xpression, even if static, is a little less usable because you need to remember what the bits are. I would probably go down the road of fully elaborating the different names, for now, and revisit after constexpr.
*   AB: So a full union or just workgroup and device-level barriers? (do we need all?)
*   MM: MSL doesn’t have all.
*   AB: Yes, that surprised me because they use memflags, but they can’t be combined.
*   JG: instead of all, can you just list them all consecutively?
*   AB: Yes, the only place it’s problematic is if you put operations between.
*   JG: I’m inclined to say that for now, if you want ALL, just write all the barriers.
*   DM: I’m confused. Didn’t David say it’s not the same as separate calls?
*   DN: Alan came back and said if you have proper barrier-barrier sync, it’s okay.
*   DM: What do we need to do to have proper synchronization?
*   DN: Need to have a memory model, and have that rule in the memory model.
*   AB: Would just say barriers always affect each other. Regardless of the storage classes they affect. Ex. a workgroup barrier affects a device barrier.


## Workgroup size specialization #1442



*   DM: Think we can close it. Yesterday we weren’t sure we can do it on DXIL, but yesterday we found out it’s fine.
*   RC: DXIL solution is to re-emit everything?
*   DN: Yes, as Tex pointed out, those specialization constants can be used in places that affect other parts of the code. And since DXIL is low enough, you have to re-emit again.
*   AB: .. when we emit the DXIL it will be defacto preserved.
*   DM: No re-emitting, You just touch the DXIL and it’s ready.
*   JG: This commits us to generating our own DXIL.
*   AB: That, or re-emitting HLSL. We’re committing to a tooling burden.
*   TR: I agree if you don’t do optimizations based on those, then it’s fine
*   RC: What does re-emitting mean - once and do it again? or the one place where we emit we update.
*   JG: As many times as we specialize it, we need to send it to the DXIL compiler.
*   RC: I don’t want it to be the case that we have to strcat and printf a bunch of times.
*   TR: I would expect we have our AST or some other IR at pipeline time,
*   JG: Is generating DXIL an approved thing? Are we expected to use the D3D12 API in this way?
*   TR: Microsoft won’t object. You do need to run it through our validator and have it signed. AFAIK though, know one is directly generating DXIL.
*   RC: Keep in mind for WebGPU compat, there is no DXIL. You need to go through HLSL.
*   MM: If, not when.
*   RC: I personally want it, but I agree the conversation is for another day.
*   DN: So our current tint flow uses #defines in HLSL, then the specialization sets the defines, and sends it to the compiler.
*   MM: Is your long plan to skip HLSL entirely and go directly from WGSL to DXIL.
*   DN: We’d like to go to DXIL within Tint. For compat, we’d like to go through HLSL.


## Extensions Again [#600](https://github.com/gpuweb/gpuweb/issues/600)



*   DN: Based on feedback, I simplified this to `enable(ident);` that’s it.
*   CW: I think that part was mostly non-controversial. The discussion yesterday was also about interaction with API-side extensions.
*   MM: We should not keep arguing about that part and take it case-by-case. A philosophical discussion is not productive. When we have shading-language-only extensions, we can talk about where they go.
*   DN / CW: Sounds good.
*   DN: Okay will make a PR then.
*   JG: We should clarify that - because the wording doesn’t say you can put it anywhere.
*   DN: It’s at module-scope.
*   AB: Need to declare it before it’s used.
*   MM: There is an issue I opened elsewhere about making the order not important. I would like to discuss ordering another time. But that hasn’t happened yet, so we can keep it ordered for now.
*   DN: Will write a PR.


## Ptr Syntax #1456



*   AB: Doc of examples?
*   JG: My experimentation ended up showing that.. wasn’t sure if the example would be super useful. I took the boids example and made all the drefs explicit and tried to const a bunch of things that weren’t const. This is the sort of transformation we’re expecting the compiler to do. The user says “var” and we emit snapshots of constants as necessary. \
And that’s fine. I don’t love it, but I think I want it to be more low-level than it ought to be. \
MM had said earlier that the load/store opts are the bread and butter of compilers. Something we should expect to do.
*   DN: Local variables are a different beast enough. Can be separated between local scope and module scope. That’s a complexity we should walk into knowingly. \
It seemed to me that how an ident was declared: variable or const ptr. That’s what indicated if you need & and *: (&myvar, myvar), (myconstptr, *myconstptr). \
It seems to me that no longer are we selecting the operation to perform based on type, but instead we have contextual information about whether something is declared as a var or a const ptr. I think that’s isomorphic to the rvalue and lvalue. Not sure, but that’s in my head right now. \
My hunch is that we’re circling around a solution which is less uniform on the inside, but more usable on the outside. If that’s the way we want to go, we can do it, but we need to do it carefully.
*   DN: in DM’s example, there’s a non-uniformity of what it means based on context.
*   JG: To me, what “var” desugars to is a ref type that is ptr. So when you use & on a ptr type, you get the ptr it contains. But when you deref with “.”, you get subsequent ptrs.
*   DN: So it sounds like there’s a synthetic type the compiler uses when processing, and at the machine-level that doesn’t exist because we’ve desugared it.
*   JG: In SPIR-V, all pointers. OpVariable, OpPointer based on the variable.
*   RM: couple of Qs.
*   RM 1. Not sure why we’re inventing something new and not reusing what’s in C or C++. Already has this idea between a ptr and a local value.
    *   DN: Not everyone is versed in C++
*   RM 2. somewhat related. Lots of arguments on what “var” desugars to. I would say that from a type point of view, vars have to be ptrs with all of the normal operations if there’s a context in which the programmer needs to know about it. If the programmer never needs to explicitly deref it, I don’t see why the programmer needs to care.
    *   DN: When I’m thinking about actions on memory, I better know what actions are provoked in the expression of the language. It’s very useful to look at a statement and know exactly what operations are happening.
    *   DN: Even my original ptrs proposal requires the programmer to remember they’re chasing down a pointer. C/C++ had a memory model that was bolted on backwards. You get into rough corners unless you’re very careful. My answer is that it’s really important for clarity where exactly accesses are occurring and what locations they’re affecting. 
    *   JG: Think it’s more important here than in C/C++.
    *   DN: Local variables only in thread, so in that sense don’t interact with memory model, but nice to keep the rules consistent with other types of memory instead of special casing. Strive for simplicity of being as implicit as possible without hitting myself in the face while writing the code.
*   RM: Thank you for clarifying the main goal is to make it clear where loads and stores happen. That’s an important point to evaluate proposals with. I’m still not entirely clear. If we have a simple for-loop, do we need loads and stores, or is it something that is done with temporaries. Not sure if we need ptr drefs for simple things like that.
*   JG: You need it for SSA.
*   MM: Because internally SPIR-V uses ptrs everywhere doesn’t mean WGSL programmers need to type stars everywhere
*   DN: If you take the address of a variable in a for-loop, then you need storage. As soon as you combine features like that, then you need it.
*   JG: When you loop back, you have to update the value of i.
*   RM: You can with Phi. Compiler will insert phi.
*   DN: Yes it will.
*   RM: Even if we add ptr derefs, it’s not clear to me whether they will correspond to loads/stores with local variables. Those are a bit special because the compiler will eliminate those.
*   DN: We have as-if semantics. If compiler can prove it doesn’t need load/stores and can use phi instead, don’t have to worry about it.
*   RM: Just wanted to talk about that, because you said it’s important for the programmer to understand where loads/stores happen. If the compiler will remove most of them, then the syntax that shows there’s load/store is misleading.
*   JG: That’s fair. There’s a proposal in the air about when you’re doing ptr operations, they really are loads/stores, but if you change the value directly, then it acts like a reference. And if you never take the address of it, then you never load/store.
*   MM: Think we’re bending over backwards to make it like SPIR-V.
*   DN: DXIL, AIR do it this way.
*   MM: People don’t write those.
*   DN: People do optimize based on register usage and occupancy, and we want them to be able to do that.
*   MM: Being able to store to variables is a core competency of a programming language. Making that hard is not great.
*   KN: Want to go back to something David said a while ago. We should avoid making local and private stuff special. \
I would phrase it as: \
  if we wanted to have something that looks more like C/C++ for local, it would be that we only allow vars in local scope, or maybe private scope. So we have an extra affordance which is “var” when you want this implicit behavior. It gives you an allocation, and not a pointer. It’s automatically loaded and stored. If we don’t use “var” for other storage classes, then we don’t have a weird disconnect for other storage classes.
*   TR: I agree. As far as I can tell, this sounds like a good option. Forcing ptr syntax for anything local is really ugly and would be hard on the programmer. But also expanding local things that are variable to require ptr syntax.. I think that confuses people between what might turn into SSA form and what access other memory scopes. “var” really is a special snowflake.
*   AB: It’s var of function or private, that’s special, right?
*   TR: Yes, but the other suggestion is to only use var for those cases.
*   MM: If this group wants to go in a direction where the behavior of local variables is different from that of resources, ..
*   `(*myResource)[17] = myLocalVariable[18]`
*   MM: How would you type check this kind of statement? What is the type of the left side and the right side? It’s an interesting problem, but it requires a lot of thought.
*   KN: To clarify what I’m proposing: `myResource` would be declared as `const myResource: ptr&lt;...>` not `var`.
*   TR: Right, var is only used for local variables.
*   RM: I like where the proposal is moving. I think it’s important to have a separation from things from the outside, and things which should behave as local variables.
*   MM: Kai’s explanation makes sense to me. Perhaps because I’m familiar with metal which treats things as ptrs. What about: `*(my_foo.x) = 5` where my_foo is a resource. What this proposal is saying that the “.” is more like an -> and an &. I think it would be a mistake to use this type of syntax. Would be a mistake to make . and * not work as people expect.
*   JG: One of the difficulties here is about not having more concrete alternatives. It’s good to get opinions on whether things feel good/bad weird/not-weird. But it would be good to have a proposal of how we would like it to be, when we said “i don’t like how it is here”.
*   MM: In this example, if the user wants to store to my_foo.x, they would say (*my_foo).x = 5; or my_foo->x = 5;
*   RM / JG: Why not have arrows like C / I like the arrow.
*   JG: ..
*   DM: I feel like we’re lost in the discussion. I thought we’re coming from a particular problem: inout is hard. I was designing based on that. The issue I filed addresses the need without all the local variable stuff that no one complained about until now. What are we solving here? Shouldn’t we state those problems first?
*   DN: Pointers were never properly explained, and once we used them more, it was a problem.
*   DM: But no one complained how you use a uniform buffer in WGSL. Myles said he wants my_foo.x, and that’s what we have now.
*   JG: I disagree. This is where we’re supposed to discover and resolve our disagreements. Just because people have said something is workable, doesn’t mean it’s the best solution.
*   AB: If you want to turn interfaces into const:ptr&lt;>, it has far-reaching consequences.
*   RM: I’m a bit confused because I first heard Kai’s proposal for a strong separation. Then I heard Jeff talking about how vars are really refs with ptr-like semantics. Different understandings of the same thing.
*   JG: Sounds like a concrete proposal would help.
*   DS: Brandon has written a lot of WGSL. Maybe he has thoughts here.
*   BJ: Not a whole lot of feedback for this particular conversation, I don’t know the underlying DXIL/SPIR-V/minutiae. What I found I wanted when writing code was simple easy ways to say, I have this structure nested 5 levels deep, I want to be able to get a ref to an inner struct to reduce the depth, don’t care about syntax. \
	uniforms.lights[0].position.x; \
	uniforms.lights[0].position.y; \
vs \
	var pos = uniforms.lights[0].position; \
	pos.x; \
	pos.y;
*   DN: That was useful. Thank you, Brandon. \
Stepping back: with Kai’s statement about having a different affordance, I think we can have the separation more clearly, but not all the way down to something that forces you to rewrite all your code.
*   &lt;break>
*   JG: I feel like there’s a proposal that would be palatable that just needs to be written. A good example of what it looks like might be good.
*   DM: Where globals are pointers? or something else.
*   JG: Among other things, yes.
*   DM: We can follow up with that separately. We don’t need to block the pointers on this.
*   DM: The only other major concern about #1456 is that the . syntax is unusual.
*   MM: The . on a pointer type to get a subelement of the struct.
*   CW: It’s a thing in C++ for ref types
*   JG: But these are ptr types.
*   MM: So the explicit ptr type is going to be const where the type is ptr&lt;T>, and the implicit one will be “var”
*   JG: Sort of, yes. The details are more complicated than that, and will be more crazy.
*   DN: We have syntaxes that denote 3 kinds of things: references and pointers and values. At machine level, one goes away and devolves to more primitive things.
*   CW: Okay, so can we have rough consensus on the direction that we can write down?
*   JG: Feel I can. Think I have a proposal that will satisfy people. Not sure I can describe it well enough for a straw-poll.
*   MM: Can you explain it out loud?
*   JG: All pointers and references have C++ style syntax and are explicit. You can do ref -> ptr (&x) and ptr->ref (*x) When you use var and give it a name, the identifier names a thing of type reference.
*   MM: And the reason we use “ref” is because my_var might be a struct, and to do my_var.x = 5, it needs to be a reference types.
*   DM:
*   JG: If you wanted pointer to ref, we could use the -> operator, otherwise you could do the * deref.
*   MM: I think what you’ve described matches #1456.
*   KN: Is “ref” namable or just implicit?
*   JG: It doesn’t have to be and isn’t today, but I prefer that it were.
*   MM: If I just wanted an int, what do I do?
*   JG: I’d like “var i = 1;” and if you wanted a type: “var i : int = 1;”
*   CW: Scary to have an explicit ref type.
*   JG: I’ve found it better to have it be a for-real type. If it’s useful in specification, I’d think it’s useful for devs too.
*   KN: Sure, but if we have it, I don’t think we should allow it for function parameters.
*   CW: Let’s keep the conversation on #1456.
*   MM: So I understand: #1456 is pretty good. Jeff thinks we need the spec language for vars to use references, and we want some of the syntax about . and * to change.
*   KN: What does var x = 1; const y = x; do? Is y an i32 or a ref&lt;i32>?
*   JG: By default it would decay. My instinct would be for it to decay into an immediate with const.
*   KN: The problem here is that the type could be i32 or ref&lt;i32>. We previously tried to make it so there’s only one possible rvalue type. If ref&lt;T> is namable, it’s not obvious.
*   MM: I don’t think we should give `y` ref semantics here.
*   DN: Can always have const ref if we really want the reference.
*   KN: Yea, we can think about that, but we don’t need it right now.
*   JG: Will take AI to write it up.


## Sample w/out implicit LOD #1319


### [does textureSampleCompare assume level-of-detail 0 in non-fragment shaders? · Issue #1319 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1319) 



*   DN: Sample w/out implicit LOD?
*   MM: By default, the mipmap filter operation is ignored all except for the 0th level. That’s what the spec means “by default the 0th level is used”. It’s the default argument for creating a sampler.
*   DN: So it’s not the default of the sample invocation, it’s the sampler creation.
*   MM: Yes. I wrote a sample application and tested this.
*   DM: Can you do the same thing for depth so we’re sure about it?
*   MM: Ok.
*   DN: Linear/nearest is on the API side, but not on the shader side.
*   MM: It’s in both. In metal you can create it on the shader side. Default behavior is the same.
*   DN: But if I create on the API side, I don’t know what it is inside the shader.
*   MM: In MSL, you might even be able to ask the sampler though.


## [The order of top-level declarations should be irrelevant · Issue #875 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/875)



*   MM: Human write shaders in many files, commonly. In native shading languages, it’s fine because #include-like solutions. For WGSL, that doesn’t work. The most natural way to handle the use case is to ask developers to concat all the source, and we give it to the compiler. That sucks because order is currently important. They have to do the dep graph themselves and order it properly. One possible solution would be to allow concatenation in any order. The pass where you parse will be different from the pass where you resolve names. That would solve this problem. There was resistance last time. I’d like to make the point that most modern languages work this way.  \
 \
Another solution is to allow forward declarations. \
 \
General goal is to make it easy for people dump source and have it compile.
*   ??: Preprocessor?
*   RM: I’d like to add that order-independent is nice for organizing code or moving code around. I think there’s a reason most languages do it this way. Just nicer to use.
*   DM: Can we delay this till after MVP? It’s solvable by users, and we can always relax it later if we want to.
*   JG: My feeling here is yea, maybe not for MVP, but it might be worth committing to trying to do it for V1 and see how bad it is.
*   DM: I’m just not excited to do this work, and it doesn’t seem that important right now.
*   MM: I agree with the general idea of doing it later. I think it’s worth discussing earlier rather than later. In our experience, it’s not that hard though. In my super long vacation last year, I made a programming language, and it also wasn’t that hard.
*   DM: People don’t concat shaders outside some OpenGL scenarios. People just use #includes today and preprocess them.
*   JG: #includes is a bit of a red herring. It’s just one of the way it happens.
*   JG: I think order not mattering is useful even if you don’t have many files. I don’t like when the beginning of a file has to start with the middle-of-the-meet-grinder helper functions. I’d rather start high level and have details below.
*   MM: Agree. that’s why we think it’s best.
*   GR: If we push this off, is it going to break shaders in the future?
*   JG: Only if we had something like variable shadowing.
*   DS: We had some shadowing problems like this in Tint. where a function has a local which is later declared as a global.
*   GR: How would this be solved if order doesn’t matter? Always incorrect? Are globals considered always at the top?
*   Yes
*   DN: We’re about to have constexpr. With order independence, you’ll have to chase down a dependency graph.
*   MM: You’ll already have to do that, I’m just suggesting you do it in a separate pass.
*   JG: Or, if we force variables to be declared in order, but allow functions to be placed anywhere, I would be okay with that.
*   KN: Interpreted languages like JS have weird stuff with ordering. JS only parses the function when it sees it but doesn’t do lookups. \
Rust is a better example. It is compiled and doesn’t do late lookups.
*   MM: I’m just proposing taking two operations in a single pass and splitting them into separate passes. That’s it.
*   JG: I think it’s worth trying, but I’m hesitant to put it in the spec. This is not a criteria for MVP, but I would be happier if we did have it before shipping.
*   MM: I agree with that. We don’t have to do it now, we might as well.
*   BC: For what it’s worth, Tint already does this. We’ve had to do work to enforce the rules of linear declaration.
*   DS: We had to fix the way variables were handled, but not functions.
*   GR: Are shaders targeted for an earlier version just supposed to work in a later version? If so, then we can’t push it off. Because of declaring a global below a function.
*   JG: That’s solvable with an extra step w/ fixing shadowing.
*   MM: If Moz, Google, Apple is a good direction, we can add it to the spec, and if it’s hard, we can delete it.
*   DM: I don’t want to do this. I don’t want to introduce an IR just so I can topologically sort it. I’d rather focus on security and bounds checking.
*   DS: It’s not that it’s hard, it’s just the more and more easy things we add, the longer it’s going to take to get an initial version finished.
*   RM: So as long as we forbid shadowing, we can add this after?
*   JG: Other way around. Need to allow shadowing (across scopes).
*   RM: Compromise where we add this in the spec for MVP?
*   JG: Feel it’s antithetical to put it into the spec before MVP. I’d rather say we want to try it at some point, but not need for MVP.
*   RM: What I mean is adding the thing about shadowing.
*   JG: I’ll file issues about shadowing.
*   DN: That form of shadowing is more palatable to me than order-independence.
*   DN: Was going to also say that order-independence means you have to explicitly detect cycles.
*   (discussion)
*   DN: We also want fast and simple compilers. Multiple passes means e.g. blowing cache multiple times. Not sure if a real concern but would rather avoid it.
*   MM: Think many modern languages are a counterpoint.
*   JG: Break
*   DM: Heads up, filed a few issues about input/output, please comment. PR 1437, issue 1436.


# API


## First public working draft



*   CW: Allows us to trigger IP review and IP exclusions. And broadcasts on many channels that “Hey WebGPU is doing this thing, check out this draft”
*   DJ: I think it’s a good idea for the reasons you just mentioned. I don’t know how we would pick which bits are in, but idk how to pick.
*   JG: Can we put a big disclaimer on it? I still feel like there’s an implicit commitment, so is there a way we can say we still expect changes?
*   DJ: they all have a disclaimer. Don’t think it’s a problem. Just say that on Twitter / etc. Not necessarily the syntax or the API - but as long as it hits all of the major features we want that’s the important thing.
*   CW: should we consider doing WebGPU and WGSL separately?
*   DJ: don’t think so. Not sure whether they’re 2 separate docs, but should go at same time.
*   JG: +1
*   CW: for API I think we can say it won’t change too much. Hasn’t changed too much for the past year.
*   KR: Can we obsolete them automatically?
*   MM: yes.
*   JG: concerned about that. If spec is constantly moving target - rather we have Editors’ Draft, actively being developed, and just have that snapshot and don’ thave to think about multiple different timelines. Just do our development, do releases when we want.
*   MM: Editors’ Drafts often do not imply browser and vendor buy-in. Doing all our work as Editors’ Drafts sends the wrong signal.
*   FD: have been bitten in the past with Editor’s Drafts being much more advanced than published versions. Keep updating ED, never good enough to publish snapshot, have completely outdated WD in the end. Now, we aim to keep things synchronized (up to group, not mandatory). Would recommend to keep the WD synced with the ED.
*   CW: way to automate this?
*   FD: yes, sure. With Bikeshed, it’s automatable.
*   CW: then we should do that. No diff between WD and ED.
*   KN: our ED will never get “less ready”. Continuously improving.
*   JG: I don’t necessarily agree we don’t want to take snapshots, but OK.
*   DJ: there is a process to publish a WD. Is there a 1-click for Bikeshed specs?
*   FD: yes, can be 1-click. FPWD is a milestone. Then have to say you want WD synced with ED, and got agreement from everyone.
*   JG: Community group?
*   FD: Working group, because recommendation track.
*   CW: Put the “working group” hat on the call, would everyone agree to publishing the FPWD and automating the synchronization?
*   JG: FPWD activates the IP disclosures, right? (Yes.) Isn’t that fraught to automatically update the WD with ED?
*   FD: The patent exclusion period applies to the FPWD, not to updated WDs. The next level is CR (Candidate Recommendation) which triggers another exclusion period.
*   JG: OK, so this would give us early warning of IP exclusions. Fine. Those are my concerns.
*   CW: Any additional concerns? (No.) Let’s do it.
*   JG: do we want to defer at all to have a couple more things in?
*   DJ: think we need at least to have a draft and a week to say, let’s do it. Don’t want - publish it, then get IP exclusions. Not that it would help if they came in earlier, but better if they come in beforehand. Generally, our lawyers would like a few days’ notice. Prob won't’ have time to read it, but at least told them in advance.
*   KN: beneficial to have explainers before we do this? Writing them for TAG review. Helpful before FPWD, or does it not matter?
*   FD: doesn’t really matter. Sometimes you always feel you want to add new feature, introduction, etc. before publication. Better if easier to read spec, but not mandatory.
*   JG: what if we have known gaps? Know we haven't spec’d this at all? TODOs about it? Should we do that first?
*   DJ: think it’s fine to have huge holes. Should at least describe what the technology is, so that we talk about what the IP is. As for automated process: not sure it would happen on each push to the repo, each Friday, etc.
*   FD: easy way to do it is upon each push to main branch. But can certainly be set up as a job.
*   CW: we can just do it on each push. It’s simpler.
*   JG: I like the idea of keeping them in sync. In practice - one of my concerns is - don’t we do a lot of the work in the ED? This is us transitioning to working on the WD. (WebGL, we work on the ED all the time.)
*   CW: AIs: make first WD draft. Send to lawyers. Do FPWD, lawyers said it was fine, so go ahead.
*   DJ: this spec’s been in development, so think we only need a week. Not a month for example.
*   CW: can you make a FPWD?
*   DJ: yes. Just take the ED.
*   JG: thing we want to do - go through prioritization doc, put your feature in if you want it, and include stubs for those in the WD. That’d give us the most coverage in terms of IP.
*   CW: addition or not of large features can be large discussions.
*   JG: some we have consensus on unless they’re really hard. Type inference, constexpr. Things we intend to do but haven’t put in the spec yet.
*   CW: I was thinking multi-queue, fat video texture samplers, etc.
*   JG: safest way I’d say - depends on how much coverage we want.
*   CW: sounds OK - there’s always a second round.
*   JG: timeline?
*   CW: No WebGPU meeting next week. Think we can agree on the meeting the week after.
*   JG: I’d prefer one more meeting.
*   CW: OK.
*   JG: maybe doesn’t matter. Would like to get more things in the spec we’re pretty sure will be there.
*   CW: we can say March 15. Two real meetings from now.


## [Method of ensuring GPUShaderModules can contain MTLLibraries · Issue #1064 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1064) 



*   CW: comment thread is huge at this point. Recap:
    *   When we compile GpuShaderModule, want to create MtlLibrary. Reasons:
        *   1) Compiling MtlLibrary has a cost. Doing it ahead of time helps the app.
        *   2) GpuShaderModule corresponds to 1 MtlLibrary, don’t waste time recompiling them.
    *   Bunch of numbers for this, including with and without shader caching.
*   Various ideas about achieving this.
    *   1) Add hint or add’l into at ShaderModule creation time that gives the add’l information to create MtlLibrary.
        *   Concern: set of data required to create MtlLibrary at ShaderModule creation is not finite - not just the layout, not just the vertex state. It can grow arbitrarily large:
            *   1) we’re going to find driver bugs, and have to inject code for various reasons based on pipeline state
            *   2) we’re going to add more stuff to the spec that needs this data. workgroup specialization constant, for example. doesn’t apply to metal, but if it did, would need another compile. taking in constraint that ShaderModule==MtlLibrary is a big one.
*   MM: proposal isn’t ShaderModule==MtlLibrary. It’s that it *could* be 1:1. Author can press keys to maybe make that happen, but they don’t have to.
*   CW: OK.
*   JG: so we’d be open to something like - pre-specialize a ShaderModule for a given PipelineLayout or similar?
*   MM: yes.
*   JG: but optionally?
*   MM: yes, that’d be acceptable to us.
*   CW: OK, guess there’s 2 ways to do something like this that have been surfaced on the thread.
    *   1) Provide hints per-entrypoint about pipeline state at ShaderModule creation time
    *   2) Hints when creating render pipeline
    *   3) Batch creation of pipelines. If you create all the pipelines for a given ShaderModule - basically same info as hints, but since you make them all at once, can have the one MtlLibrary needed for the pipelines.M
*   MM: [on Oct 22, 2020](https://github.com/gpuweb/gpuweb/issues/1064#issuecomment-714308897) added couple other options. 1) One is what you described, when you create ShaderModule you pass other info. 2) Or add other intermediary object between ShaderModule & pipeline creation. 3) Or modify shading language so info’s part of the shading language itself, don’t need to supply anything at API level.
*   CW: option 3 seems really difficult given the potentially unbounded amount of data we’d need to pass. Today need to pass the layout, but could need more.
*   CW: example. To implement robust vertex access in Dawn/Tint, injecting code to load vertices from storage buffers in Metal. With Proposal 3, would need entire vertex state in the language.
*   MM: totally reasonable for us not to choose proposal 3.
*   MM: say we come to agreement here. There’s some way an author can make ShaderModules create MtlLibraries. Would you use this mechanism?
*   DM: we would use it.
*   CW: I think it depends on whether the data that was provided is enough for us to be able to handle all the codegen. Including the workarounds we need to do, etc. Also assuming enough engineering time.
*   JG: how palatable was batch creation of pipelines? Just create ShaderModule, parses and validates the code. Give us batch of pipelines, specialization might yield fewer pipelines / MTlLibraries.
*   MM: it’s our experience - devs create pipelines while the app’s running. But they try to create ShaderModules before that.
*   CW: if app doesn’t have data about RenderPipelineDescriptor at shader module creation - how does this work?
*   MM: in order to create MtlLibrary, don’t need everything in RenderPipelineDescriptor. Just some of it. And they tend to have that - based on experience looking at Metal apps.
*   JG: what if we unconditionally moved things interesting from RenderPipelineDescriptor to ShaderModule creation time?
*   MM: for us it’s not too early. You could have both. If you specify early, you can. If you do, you have to specify the same thing during RenderPipeline creation time.
*   CW: in spirit of consensus we could do this, but we probably wouldn’t be able to take advantage of it. After WebGPU is out for 1 year I guarantee we’ll find the need for additional data, and can no longer create the MtlLibrary at ShaderModule creation time even when people give us the info.
*   KR: There’s almost an uncountable number of graphics drivers bugs to workaround on various platforms, requiring us to patch shaders. It cannot be assumed the issues won’t come up in the future. Also as we aim to do more optimizations for things like video texture uploads, we may need more information in the shader.
*   MM: I think that makes sense. You’re right in the fundamental point, but I disagree with the scale. I believe that there will be bugs, and more than 0 will need shader code injection at pipeline creation. I disagree, however, with the conclusion that because there will be bugs, we should bake in the idea that it will be impossible to ever do a compilation at shader module compilation time.
*   KR: If you copy some of the fields from render pipeline descriptor to the shader module descriptor, you’re not going to duplicate enough of them to protect against future workarounds. 
*   CW: sample_mask might not be included in the first version. Say in the future we discover we need to emulate alpha_to_coverage, requiring us to know the sample_mask in the shader. So we need the foresight that this should be provided ahead of time in case such a workaround is needed.
*   MM: I don’t know why we would go from a world where we can compile sample_masks for roughly every shader, and then all of a sudden we can’t compile any of them. What could happen that could cause that?
*   DM: If we want to enable alphaToCoverage with sampleMask together at the API level, it requires shader modification.
*   MM: Isn’t that part of the spec? How do we just enable it.
*   CW: The problem is that I’m trying to guard against problems that we don’t know about yet because we haven’t implemented all the features, or driver / OS upgrades or drivers in the wild that cause problems.
*   CW: Since we don’t know what we need, then we need everything. And if we need everything, then a hint is nonsensical. Maybe that’s too purist, but that’s the general idea.
*   JG: It’s a compromise, and yes we’re disagreeing on the tradeoffs. In our WebGL engine, we don’t do shader recompilation for pipeline state. ANGLE might do it. For Gecko though, we do it other ways. But if it’s for driver bugs that we expect to be fixed, we should try to dream of a better world and fall short sometimes, rather than be very guarded or afraid of something else. If we can pick the good gamble, we should gamble on the good thing.
*   DM: Similar to what I wanted to say. Workarounds for driver bugs will potentially introduce slowdowns. This is just one of them. IIUC, if we exclude driver bugs from the equation, temporarily, is the pipeline layout the only thing we need? If it’s the only thing, it’s an easy thing to buy into.
*   MM: It’s the only thing today. CW is saying in the future, the set will grow.
*   CW: For Dawn/Tint, that set also includes vertex state.
*   MM: Agree with JG. I can see two paths: (1) There are some bugs somewhere we have to work around. When those bugs are present we have to delay compilation. Think this will be rare enough. (2) Maybe something catastrophic happens and we need a ton more information all the time. Seems unlikely such an event would happen after the spec is publish, and we wouldn’t have noticed it.
*   KR: Strongly disagree. We continually run into performance issues in WebGL, frankly on MacOS, where we go to extreme lengths to get it to run efficiently. We would need information like the render pipeline state to know that the modifications made are compatible. The techniques used are going to evolve over time - still running into them 10 years after the WebGL specification. So I disagree that the spec group knows what’s going on and can think of everything ahead of time. Shouldn’t only cherry-pick information we know we need now.
*   MM: Last sentence unexpected. Do you think authors should be allowed to provided arbitrary amounts of extra info at shader module creation?
*   KR: Thought that was under consideration. Point is we should not cherry-pick. If we want to do this we need to provide everything - entire pipeline descriptor.
*   CW: Two functions for the flexibility of the developers. Some preparsing and generation on one side, some flexibility on the developer side. Also for perhaps precompiling some library code.
*   MM: If we want an optional pipeline descriptor in shaderModule.create, that’s okay with us.
*   CW: Seems like everyone won’t hate passing in render pipeline descriptor at shader module creation?
*   DM: Don’t love it. You often don’t know what states you’re going to need. Surely the pipeline layout is known, but stuff like blend state, stencil ref, etc. That might not be known ahead of time.
*   MM: So such an API might be useless?
*   DM: It’s hard for us to specify in a way to give them a good idea of what they should expect.
*   MM: Can’t we just ask them to pass in what they have, and maybe we will use it?
*   DM: We can explore that.
*   KN: The only principled solution is to optionally support everything. The problem is that this give devs zero expectation of what they should attempt to do ahead of time.
*   MS: For the user, I am not sure what I would do where I could give it everything I do at pipeline creation. I’d rather have the batch creation. If I supply everything up front, I might as well make everything up front.
*   
*   
*   


## Tomorrow’s agenda



*   CW: Any WGSL remaining? Can we do API only? Maybe shorter?
*   MM: This issue (shader module) is the top issue for us.
*   JG?: Vec shorthands.
*   CW: Many things. e.g. video interop.
*   ...
*   CW: Probably continue shader module.
*   
*   
*   KN: Late addition but simple: [#1465 Make requestDevice not return null](https://github.com/gpuweb/gpuweb/pull/1465)