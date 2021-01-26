# **WGSL 2021-01-26**

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
    *   **Dean Jackson**
    *   Fil Pizlo
    *   **Myles C. Maxfield**
    *   Robin Morisset
*   Google
    *   Corentin Wallez
    *   **Dan Sinclair**
    *   **David Neto**
    *   **Kai Ninomiya**
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
    *   Rafael Cintron
    *   Tex Riddell
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
*   Kings Distributed Systems
    *   Daniel Desjardins
    *   **Hamada Gasmallah**
    *   Wes Garland
*   Dominic Cerisano
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


### Another virtual F2F?



*   WebGPU main group wants it.
*   JG: Talking in WebGPU call about another VF2F. Good idea, rough consensus, no dates yet. Not everyone here is there, sound good?
*   DS: Go for it


### More meeting time?



*    JG: A lot of discussion we don’t get too, can’t even schedule all the things to talk about. The F2F could help with this. At least 2 weeks notice for the F2F but depending on how far out, would it be useful to have more meetings / extend this meeting? Should we take some of the Monday WebGPU time and dedicate it to WGSL? The WebGPU group has been taking all their time as well. Would people be interested in a supplemental meeting for a week or two? Is that too many meetings?
*   KN: Are there editor meetings
*   DS: No
*   KN: Not sure how much time folks would have for a dedicated editing meeting, but has been helpful on the API side to defer to the editor meeting. May not make a decision but figure out what needs to be done to make the main group more effective. Maybe instead of more time, we try to have a smaller group of folks looking at the outstanding stuff and figures out what can be resolved or what the target of the discussion should be. Could help move some of the agenda items along.
*   JG: Probably a good way to split out some call work. In particular, taking things on the call that end up as discussions but we want to see a better version. Keeps the on-call time smaller.
*   KN: Lots of useful things we could do in that meeting, even if not strictly something the editors would resolve, can wrangle the agenda better.
*   MM: Sounds good to me. Good to try and probably make it a habit.
*   JG: Can be open to whoever wants to show up, supplemental office hours session. Will send out a doodle to figure out a good time during the week.
*   MM: No pref either way, but should be clear if such a meeting would be part of the CG and adhere to the patent policy or code of ethics or just some folks having a call.
*   JG: Any preference? Imagined as a continuation of the CG call.
*   DN: We have a flexible framework, let’s not make an ad-hoc thing.
*   MM: Sounds good.
*   JG: Harder to sell if part of full WG, but the CG is pretty flexible. 



---



## Timebox


### [Make all textureLoad functions on sampled textures require the level #1301](https://github.com/gpuweb/gpuweb/pull/1301)



*    JG: Various opinions on the issue, an open PR.
*   MM: Was down on this last week, opinion tempered, probably fine.
*   DM: Concern with the PR is some functions don’t have the level like the multisampled texture loads. Not 100% sure about those. Not very tidy to leave some functions behind. Should be consistent everywhere. Though dont’ think we’d ever have level on multisampled.
*   MM: Not trying to match fantastical imaginary tech is better than being tidy.
*   JG: Have other cases where we don’t and probably won’t support level but we have it. 1d textures are so similar to 2d we should just have it. MS textures are different enough.
*   MM: Started discussion with Metal about 1d textures.
*   DM: Then could be mergeable now.
*   JG: Merge to move forward, can revise later.
*   BC: Don’t think MS textures would ever have mip levels. Don’t think anything in the world supports it.
*   JG: Something we can do in post if we felt we wanted to change it. Think we’re happy to merge now.


### [Bools are IO-shareable, but only for builtins #1246](https://github.com/gpuweb/gpuweb/pull/1246)



*   JR: Is a PR
*   DN: Requires polyfill on vulkan. Readings not so bad, reading would have to detect you’re writing and translate the output
*   MM: Also not hard, yea?
*   DN: Need to know the right var. If it is a pointer to a helper then we need to know.
*   DM: From SPV perspective, if you have a pointer to the output storage class, then you know it can’t be bool.
*   DN : Currently yes.
*   DM: Then not too bad, can forbid for now and make it available later.
*   MM: For now, with logical mode, we know what all pointers point too.
*   JG: Seems implementable.
*   DN: In a helper function ..
*   MM: Would have to specialize the helper
*   DN: Everything we’ve done has avoided inlining as the solution to the problem.
*   JG: Propose we try to do it, and if it doesn’t work we remove it.
*   MM: Another solution, we take advantage of the representation of pointers being unspecified so you accompany if this pointer points to the thing with the pointer.
*   JG: Easier on metal then spirv.
*   MM: In metal you can literally pack, in SPIR-V would need 2 values.
*   DN: Would handle this before SPIR_v code gen. Would have to think through the implementation.
*   JG: Rough consensus.
*   MM: Which bool outputs?
*   DN: We have bool input for builtins.
*   MM: Right, so don’t have to solve just yet.
*   JG: True, unless we allow assigning front facing.
*   MM: Don’t think any api allows that.
*   DN: The diff of opinion is for user variables, not the builtins.
*   MM: Don’t think we should allow for user variables. Just builtins.
*   DN: That’s what 1246 is trying to change, trying to allow for user variables to be bool
*   MM: Doh.
*   JG: Can we re-title this. Not really what the title says
*   DN: Started with bools not io sharabale, then allowed front-facing to be bool. Then tough we should extend to everyone.
*   MM: So if you wanted to vtx shader output as a bool, you’d just write a 1 or 0 int and the rasterizer would do the thing and the frag shader would read a 1 or 0
*   DM: Interpolation question is separate, relates to other things then bool
*   JG: Think you should use interpolatable things instead of doing with bool
*   DM: Can’t require it on integer either
*   MM: There is flat shading, for flat shading this isn’t strange.
*   DM: Are we planning to allow structures to be varying between shader stages
*   MM: I’d say no
*   DN: I’d say yes. You scalaraize it and everything gets a location
*   MM: Would make a different perf argument. Usually performance is sensitive to the number of these streams. Should be hard to accidentally output 700 of them
*   DN: Isn’t the issue not how it’s packaged but the number of words going across?
*   MM: Think it should be more typing to output many varying rather than one varying. Think it would be too easy to have a struct with a billion fields that they output. Easy to write that program, less performant for the machine.
*   DN: So, the conclusion was we shouldn’t allow structs to go across?
*   MM: Yes
*   DN: Thought the question is if we would disallow a mis-match of the data
*   JG: Thought it was the struct thing
*   DM: People want to store bools in structs and want to pass between shader stages.
*   JG: Would be convenient.
*   DN: Related question. With polyfil, does bool -> int match?
*   MM: Yuck
*   JG: Everything has to match
*   MM: Already agreed everything has to match. Structs are defined by their name, not content. Two structs with the same fields and different names, do they match?
*   DN: Extensional vs [...missing]
*   JG: For the purposes of this PR, sounds like rough agreement to take at least this, but expand later for bool variables. This is needed now for front_facing.
*   MM: Would be a soft thumbs down on allowing bool outputs from vertex shaders.
*   JG: Will file an issue on that
*   DN: So DM Will you file a post MVP enhancement?
*   DM: Sure.


### [does textureSampleCompare assume level-of-detail 0 in non-fragment shaders? · Issue #1319 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1319) 



*   DM: Why only about compare. Regular texture simple can’t do other mipmaps outside frag shaders.
*   MM: So options are not having an arg and assuming 0 or having an argument and making the programmer type 0. Validation rules would say in a non-frag shader it has to be 0. Possibly as a constexpr?
*   DN: I think that’s what I understand
*   MM: Think we’re going to need the second one, regardless of if we have the first. Should have a function that takes an argument. Reason is we have these shader modules with entry points and helper functions, legit for someone to want the same sample call inside a helper and call that helper from 2 different entry points. Another question, should we also have an overload that doesn’t take this argument. The overload would work in both frag and non-frag shaders. For frag shaders it would be level = 0.
*   JG: That’s what I don’t want. Want it obvious where it won't happen. In cases where the SPIR-V compiler takes GLSL and you said texture but we’re going to set level == 0, and would be nicer if that was explicit.
*   DM: Have machinery with derivatives to fill out the level. Machinery shouldn't be randomly used. Function doesn't need to be ImplicitLOD like SPIR-V, but we need the params clear that it’s an implicitLOD instead of being magical.
*   JG: Prefer explicit first
*   BC: Problem is texture sample, most common case is implicit. Texture sample compare most commonly is explicit. Either pattern will feel clunky on one side.
*   JG: Good point.
*   DM: Does it matter if we just call texture implicit. If common to call from the frag shader like that?
*   GR: Not a lot of support for derivatives in compute shaders.
*   JG: Will come back next week. Wil try to make a concrete proposal to talk about next call.


### [[wgsl] Consider allowing `_` ident for struct members · Issue #1310 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1310) 



*    [tabled]


### spell out when an implementation must reject a shader module ([#1241](https://github.com/gpuweb/gpuweb/issues/1241))



*   [tabled]



---



## Discuss


### Removing `ptr` and adding `ref` ([#1334](https://github.com/gpuweb/gpuweb/issues/1334))



*   DM: Lot of discussion about ptr semantics that DN write. Some disagreement on how this should be rolled out. Outcome of that will change refs. 
*   MM: So, alloca
*   DN: I think alloca is a poor solution.
*   DM: Argue.
*   DN: Alloca is a function which returns a pointer which we can’t do in general so looks funny. Pointer implicitly escapes. Magical thing that happens to storage when you leave scope.
*   KN: alloca is dynamic stack allocation, variables are static stack allocation
*   MM: There is another argument because the alloca argument has to be a type and we have no concept of functions accepting types. This isn’t malloc as we need to be more type safe.
*   JG: Preference is the bare c-style. You declare a variable on stack and it gets memory. If you want to refer to it by pointer take the ref and use that. Existing rules for pointer validation. If you want to access, explicitly dereference and get the thing pointed too. I think that’s a useful way to surface what's happening under the hood for folks who are familiar with that. Lot of existing literature. Working in the constraints of something like c++ refs. Think it's OK to say we have different rules for when you can/can’t assign pointers and can’t do pointer math, that’s fine. Phrased in terms of c-style pointer syntax makes the most sense to me. Has the most prior art. Less confusing then what we currently have with var. Fear it’s more verbose and annoying.
*   DM: If you have syntax to declare vars on the stack you can take a pointer from, what can you do with them? Is it special syntax for special syntax? If you can only access a pointer why not hide behind initialization for the pointer? \
JG: If you combine into 1 step, get var today which I find unsatisfying.
*   DM: I meant an alloca function which we don’t have
*   JG: alloca has other problems since no-one knows how it works. Would be hesitant to use that and as KN said, dynamic not static. Try to emphasize if you want on stack put on stack, if you want to pass pointer to something on stack then use that
*   DM: Would like those objections written down on the issue somewhere. Feel like it’s easy to argue for alloca being the solution here.
*   JG: I think there is consensus alloca is not the way to go.
*   DN: I agree. Wrote previously that the lifetimes of an alloca look very magic and don’t gain anything out of it.
*   MM: Don’t think we have anything to lose by trying to type up the arguments more formally cand coming back a week from now.
*   DN: Lost on where we are, why are we talking about alloca and not ref? Ref is, I think, reasonable but I think it makes lifes easier. Saw DNs argues that there is another keyword but it’s another keyword for another concept. Also reduces typing.
*   MM: One question, in a world where we have real pointers (spir-v variable pointers) will we have `ref ref ref ref ref`.
*   DN: Spec says a var is a decl of storage for a name of a certain type. A ref is going to be a const declaration of a pointer value. The value of the pointer does not change. The type is a pointer type which still exists. Just a different way to declare a const of pointer type. Could have ptr -> ptr. Could have a ref to storage which holds a pointer which is a ref of a reference. The thing that exists is pointer types and pointer values. Ref is shorthand for declaring.
*   MM: Trying to think of the code I’d write if I wanted to make a ref to a variable and then the next line is what do I write when I want to deref.
*   DN: So, Brandon had a particle example. “18 days ago” comment had an example of ref into a storage buffer of type Particle then derefs a few times. That should work.
*   MM: So, there is no deref operator
*   DN: Right
*   MM: When he mentions Particle on the right, the type is Storage
*   DN: No, the type is ptr&lt;Storage, Particle>. Still ap pointer type
*   MM: Just implicitly deref’d
*   DN: Gets implicitly deref’d when used in context requiring a non-pointer result.
*   JG: Not favourite way to describe pointers to things. Where some of my concern comes from which is why it’s more a traditional approach. See this is easy to write and usable when you understand it, but hard to bolt onto existing understanding, have to learn this way on how it works. Harder then if we had a c-style, this is a pointer&lt;Storage, Particle> and it points to the thing.
*   MM: In a world with variable pointers, if I wanted a ref that references the variable named particle and I wanted to store through the outer ref to change where particle points too, is that possible?
*   DN: No, the ref declaration is a constant declaration for Particle.
*   MM: K, so with variable pointers, pointer can not be redirected to things
*   JG: We’d still have pointers, ref is just a shorthand. If you wanted to re-assign pointers you’d have to use the pointer syntax. Then when variable pointers could reassign that thing. We don’t today so can’t reassign but the pointer syntax would still exist.
*   DN: Might be mis-lead by the name variable pointers. In Vulkan that means other things than re-assignability.
*   MM: Ok.
*   DN: Thing that gets wrapped up is we don't have explicit deref of a pointer. There is no unary * from c. Maybe that is the thing causing weirdness and confusion
*   MM: If i added `ref particle2 = particle` it would not be a ref to a ref, I’d have two things pointing to the same storage.
*   DN: Correct. Which is like c++.
*   MM: Yes, it is. I C. Ok, figuring out how this works now. So before it would have been impossible to have ref of ref. That would be a validation rule, we’d just disallow it. Here, we’re fundamentally driving into the language by removing from the type system and replacing with a different token. You just can’t have ref of ref of ref. There is no way to type that in the language.
*   DN: C allows storing a pointer and copying it. WGSL design is that pointers are not storable. You can’t put them in memory and pass around. YOu can form a constant pointer value and point into the inside of something, and pass that around, but only as constants. Can’t store/modify it as data.
*   MM: So, if understanding, this is the same as `inout` in GLSL, but you can put this on a local var instead of just params?
*   DN: If you think of each statement as each statement as a function with `inout`, yes, but there are limitations on aliasing.
*   MM: Getting into Monads
*   DN: Your entire local store is a Monad.
*   MM: Totally.
*   DM: Is the need that refs cover, is it not covered by your pointers PR. \
DN: It is
*   DM: Coud have a pointer arg in a function and mutate the pointer
*   DN: Ref is a shorthand for the pointer type. So when DS makes code with a const and assigns to the const, it’s not that it’s constant it’s that it’s a fixed point in storage.
*   DM: In a function you aren’t typing const.
*   DN: True, not sure what the primary use case was, but same solution.
*   MM: One note, Swift has something similar. In swift, when declaring a class as `class foo{` but could say `struct foo{`. Would think they’re the same but they aren’t. Things as `class` are references. Things as `struct` are by value. There is at least one language with this kind of thing.
*   KN: C# also does that
*   MM: Warming up to this. Would like to discuss with RM.
*   DM: Feels unnecessary in the presence of pointers. Can postpone to post-MVP.
*   DN: True, could be pure syntactic sugar.
*   MM: Distinction between ref and ptr in wgsl is similar to c++. Don’t think devs struggle under the weight of those concepts
*   DM: They aren’t? Would disagree.
*   MM: With the distinction between them
*   DM: Especially mutable arguments, where you can’t see it’s being mutated when you call the function. Don’t think that's a good style.
*   KN: Chromium changed their style to allow mutable references for non-null parameters.
*    JG: Good place to stop, let’s get some more comments on the issue. We talked about earlier, if we linked or inlined the minutes onto a bug they’d be more surfacible
*   MM: In other groups, IRC bot that speaks github API.
*   DS: Should cleanup ahead of time as spelling is bad …
*   JG: Can clean it up. If these notes, and if you don't want something minuted ask for it not to be, should we post them?
*   MM: Inlining is good.
*   JG: Will try this week and see how it looks.
*   MM: Put inside a details element so it doesn’t get super long.
*   JG: Would like to be able to ctrl-f on the page. Hopefully that can see into a details element. Will try that, not too much work to do manually.
*   MM: Could also click on details elements and then cmd-f.


### [Provide shader inputs as arguments to entry point functions · Issue #1315 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1315) 



*    DM: Background, when we write WGSL shaders there is a common problem that you have to repeat the varying between stages because of in vs output storage classes. Can’t use the same name as WGSL disallows that. Becomes tedious and unnecessary. Looking at target languages, HLSL has input/outputs as arguments and resources as globals. SPIR-V is only globals, and MSL is all arguments. Full spectrum is available so we can pick what we want. Think following HLSL for in/out as arguments and resources outside is the correct middle ground.
*   MM: So, vertex shader outputs as strut and frag shader accepts struct as param?
*   DM: Yup.
*   MM: And there are global variables, one in and one out both as struct type?
*   DM: Yup.
*   JG: Like the idea of sharing the struct between the two.
*   MM: One question, are we going to punch through nested structs?
*   DM: I think the nesting 1 level is for MVP don’t think we need more? Meaning you can pass structs around but not structs in  structs.
*   MM. If vertex shader outputs struct with struct in it, legal or not?
*   DM: Don't think it needs to be legal for MVP. Depth of 1 is fine for solving the case and being forward compatible.
*   DN: You said in/out variables, I thought this was marking as arguments to entry points and return values. And the location attributes would be on the members of the struct right?
*   DM: Yup.
*   DN: If you re-use then the same locations  for both input/output
*   MM: You don’t have to do this though, I could have 2 distinct structs it would match using rules today. Do the in/out storage classes disappear?
*   DN: Output probably has to stay but not input? Output is read write?
*   DM: Seems like they could disappear.
*   MM: That’s what metal does. You don’t have read/write outputs. Outputs are return values. If you want read/write use local var and copy into output
*   DN: What happens on discard? Maybe that’s too detailed and should discuss on the bug.
*   JG: Let's come back to this next week.
*   MM: Seems like a good direction, like it in general.



---



# 📆 Next Meeting Agenda



*   Meet 2021-02-02
    *   Atomics proposal: [https://github.com/gpuweb/gpuweb/issues/1360](https://github.com/gpuweb/gpuweb/issues/1360) 
    *   Barrier	proposal: [https://github.com/gpuweb/gpuweb/issues/1374](https://github.com/gpuweb/gpuweb/issues/1374) 
    *   1315
    *   1334 has some consensus as MVP we can look going forward?