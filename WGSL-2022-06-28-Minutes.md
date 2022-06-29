# WGSL 2022-06-28 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon** Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-06-21 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1kQpi7uKqaZ8vS3F9JwNe_hanBGL18Fff1xhIsBHhVYM) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Daniel Glastonbury
    * **Myles C. Maxfield**
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Price
    * Rahul Garg
    * **Ryan Harrison**
* Intel
    * Hao Li
    * Jiajia Qin
    * Jiawei Shao
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * Zhaoming Jiang
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * **Jim Blandy**
    * **Kelsey Gilbert**
* Connecting Matrix
    * Muhammad Abeer
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* **Dominic Cerisano**
* Dzmitry Malyshau
* Eduardo H.P. Souza
* Jeremy Sachs
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* Mehmet Oguz Derin
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
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


### FYIs and Notable Offline Merges



* Next week on July 5th is APAC meeting


---


# ⏳ Timeboxes (until 11:30)


### [Set a limit for maximum array size #3078](https://github.com/gpuweb/gpuweb/pull/3078)



* Previously: [https://github.com/gpuweb/gpuweb/issues/2118#issuecomment-1117637250](https://github.com/gpuweb/gpuweb/issues/2118#issuecomment-1117637250) 
* AB: Something for discussion, internally forced us to think a bit more.
* KG: Why 64k
* AB: exactly. Thought that was in notes from meeting, so started there. Do we count bool or abstract number differently? Reading notes, read as minimal maximum but implementation could choose to do more. Spec doesn't’ say that for limits. Worthwhile distinction to make, otherwise why not tie to api side buffer side so if that changes, you only change once. Should agree all implementations must support at least this but they can also support more.
* JB: So, implementations can’t reject things smaller then this
* AB: Right, must have at least 64k bytes of elements. Separate issue for number of elements limit which would be separate. Rephrasing limits section as must support this, and above it is implementation defined. Then pick number everyone is happy
* JB: Desire is to require implementations to support this but device could reject if it doesnt’ support
* AB: Right, always possible for a runtime error. Do best to compile, and have tests, but can compile where you don’t run out of memory.
* JB: Basically lower bound on implementation restrictions
* AB: Right. Gives users guidance of at least this amount is supported.
* JB: Seems reasonable.
* KG: So, it’s optional?
* AB: No, must support least this array size. If you choose to support more that’s Ok. but you can’t support less then that.
* DS: Comes with portability issues, yea?
* AB: Well, OOM isn’t portable. If you stay within bounds it’s portable, above it you’re on your own. Implementation may warn about going above it.
* KG: So, ideally these are minimal maximums. Should rename section
* AB: Yes. Also do we want a separate number for bools / abstracts.
* KG: If we run into it … Have we run into it?
* AB: Fuzzer found bools. Abstracts haven’t been hit by fuzzer yet but assume it will. 
* JB: So, arrays of abstracts as intermediate values in expression and higher up gets converted to array of concrete type?
* AB: Also agreed to abstract const arrays, so just declare them
* JB: So, constant which use at different spots which has a different type at each use?
* AB: Yup
* JB: Didn’t catch that
* AB: That’s true for any const. Where it’s used is where you convert.
* JB: Right, just an aspect i missed. Thought it was concrete when named.
* KG: With understanding calling this group minimum implementation defined maximums I think this is fine.
* DN: One thing, storage buffers can have very large arrays so this limit is about a constructable value but might have storage buffer with 1 million elements but can’t load that into a let. That’s the cutoff. So, carve-out for buffer contents being larger is fine.
* KG: Is that true for the actual sized storage type?
* DN: Think it’s 100 or 256meg is what you can bind to a buffer. Shouldn’t be limited by this
* AB: That is the minimum max on Api side
* MM: We can say author can make array this big and that’s cool, but if the author makes a lot of arrays of that size it can fail
* AB: Lets impl pick limit internally and just stop trying. If have lots of memory and thing is giant can just say not supported
* DN: Should be able to write CTS test to create array of 64k and it should pass and behave correctly
* KG: Something this big should work. Feels like 64k is not enough for everyone. Need to be able to have larger arrays for storage things. That seems clear. Otherwise ban people where if they want bigger it has to be dynamic, which is too limiting and can only be one array
* DC: Is this binding size of array being passed in? Or, internal array size as well
* DN: Mostly about internal array sizes
* DC: So, applies to any array benign bound going in?
* MM: This is different, there is a limit for that, but that’s not this. That’s an API side limit, not WGSL limit.
* KG: We don’t have storage location annotation on arrays right
* AB: Correct
* KG: But that’s what we’re getting at here (don’t want to add one) but same way we ban ptr to things based on storage this seems similar. 
* AB: Can phrase as this array instantiated in the following address spaces, if that’s preferable
* KG: Think it would be. It’s too clear folks will want more then 64k  with arrays coming in with stuff so should already say it’s a limit for private and stuff like that. Maybe just say storage and uniform are different?
* AB: Uniform is worth double checking. A fixed size array that’s really big would FXC have issues as a uniform or just local variable. Don’t know off hand.
* KG: So, what words should we use for 64 k limit, not storage not uniform, private function?
* AB: Private, function, workgroup. Think there is another workgroup limit. Better to phrase as here are the ones it can use
* DN: Don’t want creation time array expression which isn’t in storage but should be limited by this constraint.
* KG: Creation time array?
* DN: Creation time ;constant of array type. Ones we allowed last week.
* KG: Cool. Think we’re set there
* MM: Did i overhear it’s impossible to bind something of … array without wrapping in struct?
* AB: No
* MM: Great.


### [Rewrite explanation of number tokens #1454](https://github.com/gpuweb/gpuweb/pull/1454)



* Do we still want this? It sounds like it has some non-editorial parts?
* MM: This is closed.
* DN:  All superceded, except for the TODO.  For the TODO, I filed [https://github.com/gpuweb/gpuweb/issues/3103](https://github.com/gpuweb/gpuweb/issues/3103) as editorial issue.
* 


---


# ⚖️ Discussions


### [uniformity analysis + basic tail-branching #3007](https://github.com/gpuweb/gpuweb/issues/3007) 



* DN: Google agrees to implement merge-return in SPIR-V backends. Trust the experts in MSL and HLSL that things are supposed to work as promised.  Accept this solution until testing proves otherwise.
* DN: Talked off line and willing to go with what others were asking to do. For SPIR-V do merge return transform so problem goes away. So we change uniformity analysis to match. MSL and HLSL it should just work. We’re willing to go with this and defer to see if we can break it but not blocking for now.
* AB: Think we should invest some effort in getting good testing for  uniformity code.
* KG: Alright. Other thoughts? … That’s consensus then


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* MM: Interesting comment about how HLSL defines that it always uses copyin/copy out even on device with pointers it’s not allowed to use them. Followup question, does HLSL define order of copy-in copy-out?
* GR: Not officially. Studies show FXC and DXC seem to do same thing, but not exhaustive testing. Referring to if the order of params is the same?
* MM: Yes. The reason talking about ptr vs copy-in/copy-out is they have different behaviour, but if the order is not’ defined there is also some behaviour that is possible. So, even if reasoning is wrong it maybe that claim is still correct
* KG: Which direction did we leave off …
* MM: If remember correctly, DN said some stuff that I was confused about. Looking for DN to make position statement from first principles.
* KG: Confused which direction we’re talking about … Who wants what
* MM: We want to go as far as we can towards ideal of aliasing as well defined. Understand we probably can’t get there due to some backend language requirements we can’t work around. Want to get as close to that as we can.
* DN: We don’t think that is a good idea. Don’t think ?? problems until now. Folks write HLSL code and they get by, don't want to close of future with more featureful pointer based programming model. Don’t want to increase the compiler complexity more as they’re already complex. More complexity is more bugs and more compile time which is an issue with games and graphics apps. We’re happy with spec modulo clarifying understanding.
* MM: When people write HLSL they don’t have ptr type, they can’t make int and have ptr y pointing to it. Think I’m saying that argument in so far as constructs present in HLSL don’t think i agree with local pointers in a function.
* AB: So you want as far as you can with alias, where is the point between aliasing and restrict? What is half alias?
* MM: Something like compiler can assume they alias if they’re ptr type. But pointers created inside a function, assuming  they don’t point to thing parameter points too, may alias. And by may alias you get well defined behaviour if they do alias
* AB: So, compile may assume or must assume they alias?
* MM: Must assume they may alias.
* AB: That’s just supporting aliasing which I don’t know how to do in HLSL. Don’t know difference between aliasing and restrict
* MM: In HLSL there is no such thing as local ptr type. If someone does in WGSL then all uses of pointer have to be replaced with name of thing it points to. So, thing proposing is naturally expressible in HLSL.
* GR: Right, so then you have a and b pointers to c. Pass a, b to function ad in HLSL they both become object c. Is that right so far?
* MM: They get copied in/out
* GR: copy-in/out in random order, no guarantee which order
* MM: Right.
* GR: We have discouraged folks from doing this in hlsl, passing in the same thing in two names.
* MM: Right, so that is complimentary to behaviour defined. For function params it is according to spec. Describing local variables, no calls, just locals. Locals aliasing has well defined behaviour.
* DN: In WGSL you can make a let ptr which is a name for a ptr inside a variable storage. In that function you can stack that up, they all alias each other. They all have the same root identifier. So, spec is saying that’s all OK and compiler has to sort it out inside the function body. Compiler is expected to do that analysis. You can write through one and read through one and it should work as expected.
* MM: Really? Read spec and didn’t come to that conclusion
* DN: That’s where root identifier is introduced. Didn’t go through rules about let to ptr, did say they have the same origining variable but didnt’ same say root identifier. Root identifier is tracked back var declaration or function parameter ptr type. Can make that more explicit, that root identifier LHS becomes root identifier RHS. Everywhere seen as originating variable could also be root identifier. Maybe concrete examples of thing want to be true and make sure spec says matches. As long as in line with what was said. Having read spec and not getting this as said is a spec bug
* MM: Expected to see distinction between local var of ptr and param of var. 
* DN: By local, you mean let?
* MM: Yes
* AB: A function parameter aliases the let, alias only in one direction isn’t true. With multiple operations and one is a write they have to use the same root identifier. If a let is based off a param, as long as using the param of the let that’s fine. But if based on global or another parma with same originating variable then it goes wrong.
* MM: Maybe better if try to write example code. 
* KG: Let’s do that async and come back to this next meeting


### [should WGSL allow shadowing of builtins at module scope #2941](https://github.com/gpuweb/gpuweb/issues/2941)



* KG: Maybe just coming back around to namespase don’t seem like a bad idea. Was worried about them for a while. Main new thing here is idea of having builtins dumped in global namespace by default but could passively overwrite them with user functions and would pickup user function but if you wanted to you could ask for the old builtin by name, deliberately specify std.min() and get both worlds.
* MM: Interesting,as that was what DG said internally. 
* GR: If we redesigned HLSL we’d probably do this as well. So think it’s a good idea
* MM: To add namespaces?
* GR: To add namespaces for buildings
* KG: And use it by default?
* GR: Yea, just adding my +1.
* KG: Other things for ideas, was idea of explicit versions for std library builtins. So actual releases could have std.22 from year 22 and next year have std.23 including std.22 + more. So, could get explicit and say only initial release things. Also talked about compiler warning, like linting task you’re using builtin you didn't’ qualify. Opt-in warning that some may like, but probably not worth an error or enable.
* MM: Bunch of different topics here. One of views we have that is strong is we don’t want the website to tell the browser i’m using a particular version of the compiler so please don’t expose anything not in this version of the compiler. GLSL did it this way, if you said 410, then things in 420 were not to exist. Feel strongly we don’t want that design as it requires n versions of the compiler.  For things that are not breaking it’s not worth it.
* DN: Saw BC and KG nodding in agreement
* BC: main thing, don’t want compiler to know every version. Main thing I don’t want is maintaining a full history of everything added and having to test with everything enabled/ disabled. Avoid the 2^n problem with testing. What is apples opinion of a high water mark to prevent accidental usage of bleeding edge features
* MM: Didn’t understand so no position
* BC: Imagine webgpu api and compiler are separate components. Mental model is you have WGSL given to compiler and there is a minimal amount of effort in compiler to maintain a feature used to highest version watermark. For any given feature used in WGSL we maintain the highest version.
* DS: For any feature X, compiler knows “X was introduced in minor version 23” for example.  And as the compiler encounters feature X in WGSL source, it remembers “23 was used”.  And it takes a max over all those minor numbers. Then in the end the compiler reports the highest minor number N encountered.
* BC: Compiler always builds at it’s highest level and just reports what highest level it found was. So don’t have to remember everything
* GR: This is what HLSL compiler does.
* AB: MOtivation is that new sugar that we release it rolls out to browser at different speed. So, you opt-in so you know it’s guaranteed it’s going to work everywhere and can transition when I want too
* MM: You don’t opt-in to the version. Sounds like in this model the browser tells you want version you’re using and you can then do what you want with this knowledge.
* AB: Could be done that way, but it comes down to we think there should be something on the API side where you specify the WGSL version and the default is 1.0. If you want more features you have to opt-in. Internally folks have been very firm, not adding features without people knowing. You have to opt-in.
* MM: Confused now, as AB and DS seem to be saying different places. World of WGSL teling user what they used is different from browser saying
* AB: If the api says allow highest 1.0 then wgsl says 2.0 it can issue an error
* JB: Could do what MM heard by including watermark in compilation info. Then no enforcement and user decides what they want to do. Folks always say they want to target this SDK to get a handle on audience. Apple and Android APIS do that. 
* AB: We think it’s important that you opt-in to something more then the default. If you don’t do anything on API side you get 1.0 WGSL until you specif. How do you do this so it’s nice on tooling. Initially thought of GLSL way of version string in each shader but seems overkill. Can just say I’m going to use this version and it works everywhere. Don’t have to worry about it working on other platforms as know what they support
* JB: So don’t want it on the user to verify they used too much?
* DN: That’s correct.
* MM: Don’t understand what GLSL does and what’s proposed? In one author says i”m targeting this version of the language but saying in the API instead of language
* BC: From end user pov they seem similar. But implementation side by enforcing the shader only gives high water mark, only as an output, not input, guarantee backwards compat with the language. Everything compiled before must continue but we track must advanced feature used. Don’t think the justification has been prosperity explained, the user always specifying the language feature allowed is to prevent someone from writing a shader that is only supported by FF and doesn’t work on other browsers as it doesn't’ work on other browser yet. A seat-belt to make sure they don’t opt-in to to high things
* GR: Very similar to HLSL. You say SM6.6 and the compiler tracks an dif you use something higher compiler fails with error. To show how easy it is to use a feature that isn't’ supported talking about things like passing existing types to existing builtin functions. We could add in future versions and it gets used accidentally.
* MM: Want to contrast with JS as last week but got cut off. JB said JS is versioned.  Not quite what getting at. If you use ES2016 you can’t ask. The only version enforced is `use strict`. ES2016 isn’t enforced you don’t ask for it. So the JS model is pretty good.
* BC: You could if we did expose in the API give me the most recent version of WGSL support and they could feed it back into as the requested version. Goes against most of what’s asked for, but you could do it. Also increases fingerprinting I think
* KG: Not worried about that
* MM: Already observable.
* KG: No more being exposed. It’s a conscious choice to always just use the things.  Only thing for feedback, would prefer if it was in-band vs out of band. Find having to hold shader source to representative shader and a tuple (src, version) don’t like having to do that and would prefer if it was a src file annotation.
* AB: Some folks internally who prefer that as well. Not sure if that’s a GL background thing. It then needs to be in every shader. Some advantages like copying code off web and know everything about it. If generating tonnes of shaders lots of duplication, vs just appearing in the API.
* BC: Big games/projects have 1000’s of shaders and moving to more recent shader do you force everyone to update their shaders? If it’s generated, does it matter if it’s in the shader or the API? 
* JB: Not sold on markup in shader text. Based on plan knowing the version isn’t necessary to interpret the source. You know what the src means without know what version it’s marked as. You only report high watermark and verify it’s what is expected.
* KG: Come back to this next week.


### [wgsl: function param that is pointer-to-workgroup is problematic if the store type contains an atomic #3067](https://github.com/gpuweb/gpuweb/issues/3067)



* DN: Problem if user defined function taking func param which is pointer to workgroup. If that workgroup variable has an atomic in it you don't’ want copy-in/copy-out as the thing you called could do an atomic modification and copy-in and out is the wrong thing to do. Atomic has to operate on the locations.  So, a problem implementing this with a copy-in/out environment and we want that in HLSL. So, can either say can’t have ptr to workgroup as func param where workgroup has atomic, or avoid sharp edge and disallow ptr to workgroup. Prefer second option and just eliminate the case
* MM: HLSL has atomics and copy-in/out, what does HLSL do?
* AB: Run into issue as atomic requires storage qualifier so can’t do that on input. So can’t do on function apram, have to access the var (at least when I tried in HLSL)
* KG: So have to use global variables?
* AB: Yea.
* GR: That’s correct. Global or group shared.
* AB: Meant module scoped.
* MM: The problem trying to solve is someone wrapping atomic in function and the signature of wrapper is same as atomic op then what they think will happen, won’t happen.
* DN: Right.
* MM: If did same in HLSL it wouldn’t happen.
* AB: It’s a compile error in HLSL.
* MM: One option is forbid, what’s other?
* DN: Both forbid, but how complicated is the carve out. Complicated but precise is forbid ptr to workgroup when store type contains atomic in hierarchy. Simple option is forbid ptr to workgroup all together.
* MM: Kneejerk, prefer first one. Can imagine ptr to workgroup in helper function, seems legit. We know types in compiler and can do recursive search.
* KG: Sounds like tolerable carve out.
* JB: Option 2 forbidding workgroup pointers is the one we could extend to specific later. It is more in line with keep it simple now and refine later but agree with MM that sounds like it would be pretty useful in the non-atomic case. More complicated carve out sounds better.
* AB: Already have restriction to pass ptr to workgroup is to pass a ref to the entier variable. Kind of limits usefulness. All vars have to be the same type to be compatible. Not as useful as part of this var and part of that var. They’re literally the same type
* MM: Understand what saying, but still seems like a use case I can imagine folks wanting.
* JB: If we add this we’re going to run into folks with compute shaders in WebGPU and will get feedback on how this is annoying
* DN: We may, objection is about dev ergonomics and how hard is it to explain and remember. Not technical, but people problem.
* JB: Already have user defined function clause, which is only getting longer.
* KG: Think good warning messages will help
* MM: Think argument about human problem makes sense. Think the claim I’m making is I could image that more the 50% of users of ptr to workgroup will never use atomics in them anyway.
* DN: Poster Child for atomic is sorting with a stage and barrier and swap. Double buffering situation. Good example of helper and pointer to workgroup. So, buy that 50%.
* MM: Another example is matrix multiply, don’t use atomics but want workgroup memory as it’s faster.
* DN: So go with more complicated carve out.
* KG: Sounds like it.


---


# 📆 Next Meeting Agenda



* Next week: **APAC!** Tuesday, July 05, **5pm-6** (America/Los_Angeles)
* [https://github.com/gpuweb/gpuweb/issues/3111](https://github.com/gpuweb/gpuweb/issues/3111) length builtin missing f16 and AbstractFloat overloads
* Escape Mechanisms in Identifiers [#2810](https://github.com/gpuweb/gpuweb/issues/2810)
    * DN: Recommend closing this. Don’t keep it in PostV1.  (But only discuss if Oguz is available)
* Why have literal suffixes at all?[ #2664](https://github.com/gpuweb/gpuweb/issues/2664)
    * DN: All that remains is the idea of local context where you could say “abstract floats resolve to f16 before f32”. I would like to push that conversation to post-v1.
* 