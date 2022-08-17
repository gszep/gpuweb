# WGSL 2022-08-16 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** ds

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-08-09 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/166WwDXGgrMOjiWF62fQq8CSDDYjgoV4toay4iOlQpcs)

 

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
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * David Neto
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Price
    * Rahul Garg
    * **Ryan Harrison**
* Intel
    * Hao Li
    * Jia A Chen
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
    * **Tex Riddell**
* Mozilla
    * Jim Blandy
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
* Dominic Cerisano
* Dzmitry Malyshau
* Eduardo H.P. Souza
* Jeremy Sachs
* Joshua Groves
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson


---


# 📢 Announcements


### Call for Editors



* Announcement email went out!
* WebGPU API spec editors:
* Kai Ninomiya (continued role)
* Brandon Jones (continued role)
* Myles Maxfield (former WGSL spec editor)
* WGSL spec editors:
    * David Neto (continued role)
    * Mehmet Oguz Derin (new role)
    * Alan Baker (new role)
* Thank you all!


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


### FYIs and Notable Offline Merges



* [Add a data-flow analysis for local variables in uniformity](https://github.com/gpuweb/gpuweb/pull/3298#top)#3298
* PR up for recursive descent grammar appendix


---


# ⏳ Timeboxes (until 11:15)


### [Precision requirements for atan2 appear to be too restrictive for some backends #3209](https://github.com/gpuweb/gpuweb/issues/3209)



* KG: Like MM point we shouldn’t care too much and figure out what the bounds are. Folks either us the builtin or roll their own. Can fixup later. Just need a PR to update the bounds even though it makes the bounds worse. Or, maybe do something else? Friction is we define a lot of other precisions in terms of atan2 as it is the most precise, but hlsl it isn’t
* MM: Mostly editorial change.
* KG: Agree
* MM: Question, what is the best way to frame in the spec? Calculate individual ULP bounds for each function, think the answer is yes.
* KG: Agree, think we should mark editorial and leave to editors
* GR: THough conclusion was we would look at the operations it bounds into and the precision guarantees and decide the precision
* KG: Did that in the comments, divides y by x and calls atan.
* AB: Difference inheritance but separate issue where we need to say inherited from allows optimization on the math, similar to Vulkan. Not strictly that formula, but refactoring of the formula.
* KG: When saying in terms of atan2 we mean in terms of error bounds. Free to have a more precise acos, does not’ need to have same bounds
* AB: Other instructions too, if you refactor distribution has to be valid. Cant’ just check 1 formula in spec, can list one but has to allow mathematical refactorings
* MM: To understand, saying if the shader has single operation then we could have strong bounds on ULPs but if shader has more then 1 op that flow into each other then all bets are off? Is that what you’re saying?
* AB: x^2 - 1 == (x + 1)(x - 1) should be valid refactoring to use the RHS and give a similar result. RH is running into this as we test in the CTS
* AB: Not strictly editorial as we’re loosening the spec.
* MM: Making the point that we we compute the bounds of the LHS and RHS they might be different and that has to be legal
* AB: Yes.
* KG: **Sounds good. I look forward to PRs.**
* MM: Think thats fine but calls into question why we have error bounds if that op might not be in the shader
* AB: If no error bounds can just return 0 and have a correct function. Want something to constrain the implementation and this is a practical limit.
* MM: What is purpose of listing error bounds? Trying to help authors and give idea of what can be relied on, sounds like with error bounds in spec they can’t be relied on, so not helping anyone.
* KG: Don’t think that’s true. The truth is between those two somewhere. Don’t have absolutely free reign to blow everything apart and put together in any order. In general they do help, just not perfect and I think that’s probably Ok. If you had algorithm that did FFT and need sin and cos to work with precision and if you need more precision do something else. If you mix up math too much with too many optimizations that’s on you. If you take sin and give result it should be within the bounds.
* KG: Think we keep going with this for now and accept editorial and we’ll take a look as we come up with error bound expansions. Another way, is it better to not have them at all? I don’t think so, better to have something even if not perfect.


### [Set a limit for maximum array size #3078](https://github.com/gpuweb/gpuweb/pull/3078#top)



* (PR for review.)
* (DN: Are we agreed on the numbers.  Wasn’t clear to me from notes from last time.)
* AB: Discussed before and came back with min of max type language and break up limits slightly. Have updated PR to do that when DN last looked split out workgroup memory as api sets specific limit and link that to the API. Otherwise it is the min acceptable limit and breaks out cases in a better way. Question if folks like those numbers.
* MM: Like numbers. API limits are max not min (for workgroup memory).
* AB: Think that’s OK. Just means the min == max here. Links against the spec so you’d go read that for more detail.
* KG: By minimum maximum what do you mean?
* AB: Lowest limit that your implementation (say tint) has to compile. Or like Naga has to support that many elements in array. Above that might be a compile error. What we were saying before was it would always be an error. Hard to define those cases just right. Trying to protect against asking for silly large values (as a fuzzer does) and we’d like to cut that exploration off. These limits are practical and probably don't’ need to go above them but tooling can support if it wants.
* MM: Think this is reasonable don’t understand why workgroup memory is aprt as it’s just max, seems different class.
* AB: Can remove it if think it’s better. We don’t list storage buffer so maybe ok to not list workgroup
* MM: Purpose of API limit is there is no device that will ever go beyond that limit. So saying it may go beyond the workgroup limit contradicts API side limit. As last time, can have 1 of these arrays but second one may fail, so kinda meaningless but don’t want it to be a blocker.
* KG: **PR approved. Good to land**.


---


# ⚖️ Discussions


### [invalid expressions in const-expr and override-expr should be error (no later than pipeline creation time) #3253](https://github.com/gpuweb/gpuweb/issues/3253)



* MM: In discussions, runtime is more important then compile time behaviour. Think the compile time behavour is dependant on runtime. Compile time behaviour depends on runtime behaviour. Want to split issue and discuss runtime first.
* KG: What about the runtime behaviour?
* MM: Not ok to have so many operations that are undefined for roughly half or more of the domains. Made a list of functions that fit that criteria. Would like to make them more defined. Would pick some value for them to have a total domain.
* KG: Seems reasonable.
* AB: Get into all sorts of issues like how expensive is it to support this behaviour
* KG: Key should be focusing on the in bounds should be what is expected. Out of bounds doesn't’ matter so much as long as consistent.
* AB: Matters if you have to treat it the same way. If doing test of select operation all the time using builtin. Expect sqrt to be fast, but is much slower as we’re testing the domain.
* MM: We can get into details of that, think case is clear for inverse trig. Going to be slower everywhere so adding clamp or select wont’ be observable. Those are obvious win, can discuss others.
* KG: Yup. Think we’d want proposals for what to do.
* MM: Strawperson proposal is implementation fo sqrt is actually sqrt(max(x, 0))
* KG: That or abs.
* MM: Right, would be sameish cost, would be fine also
* AB: If x is close to zero, such it rounds, you’d have to take 0 out of the domain. Something just around zero do you round the x?
* KG: SHouldn’t need too 
* MM: Not for sqrt. For functions with asymptote at zero may not may not. Sqrt is defined at 0 to be 0.
* KG: Same for logarithms
* AB: They have an asymptote
* KG: invert sqrt and log and log2 would be infinities
* AB: Why is nan only issue? We can’t represent infinity. They’re just as undefined.
* KG: Don’t think they’re just as undefined. My view is that when you write 1/0 in WGSL something should happen. What should happen is try to gen infinity and if it doesn’t support then we figure out what happens. We should reduce these to what happens when you divide 1/0 or -1/0.
* AB: Why is that different from sqrt(-1) if don’t support nan ti will do op and give result.
* KG: Because infinity is less undefined then nan.
* AB: Don’t know what that means
* MM: Different thought, more concerned about functions which according to IEEE produce nans (or non-normal) for around half or more of domains. View infinity as special case as 1 bit pattern. For sqrt half of domain would produce non-normal number.
* KG: Things making non-normal numbers there isn't’ much you can do with those. Tainted forever. Infinity has more use. Will get use where have revision of WGSL where infinity is real. (haha). Infinities are supported now.
* AB: When you get there I think you’ll get real Nan as well.
* KG: THen what do you want?
* AB: If infinity is undefined then why define nan? Why not just say it’s undefined? If someone mis-uses a math why is that an issue? Do the training wheels go on for nan but not other things? Don’t understand why so special
* MM: Training wheels for functions which are not normal for half or more of input
* AB: Seems arbitrary.
* KG: It is.
* AB: It’s just a judgment call
* KG: We’re in the business of making calls. One might be to weight the evidence and say we don’t or say we do it based on incomplete evidence. Think these things are just not super important to fix. Problems folks already have. Existing code will continue to work. Would it be nice to do better? Probably. If we do better in the future, folks who’s code works continues. We’ll be normalize code which isn’t supposed to work. How strongly do folks want to have to grind out the consensus?
* AB: What described isn't’ quite right. If given behavior, can’t given nan on the future as it maybe incompatible. Not something we can just change the other way. Leaving undefined, restricting to make a single answer is OK.
* MM: In the future when we rely on IEEE 754 and turn off fast math that will be hardware dependent. There is hardware without non-normal numbers. So has to be an extension so text of extension can say changing behavior of ops to return nans as we can no represent them.
* KG: Want to chart course to future where we can ignore nans and infinities that didn’t work. Want a future where they just work when we have support. Would be nice if that behavior could be zero overhead which is to say that absent drivers severely misbehaving we give them ops and they give us results and when we ask for IEEE version, they give IEEE results.
* MM: Think that’s consistent. When the future “make math work” according to IEEE extension teh compiler would not max or round. Fits naturally.
* KG: People then have porting issues at that time?
* MM: That’s point of extension, it’s opt-in and behaviour of program changes.
* KG: Ok with that.
* AB: My opinion, not doing anyone a favour by defining these things.
* MM: Motivation is both JS and webassembly are more defined then what we’d have without the changes we’re discussion.
* KG: They have nan and infinity
* MM: Webassembly has nans but no defined bit pattern so acos(2) can’t rely on the bit pattern but can rely on it being nan and flow naturally from there. JS does define the bit pattern. For us without these changes who knows what you’ll get which is very different then prior art on the web.
* KG: Different from CPU prior art. GPU side we just call the functions and you get what you get. In a way, asking to do better then WebGL.
* MM: Yes, asking to be better then WebGL.
* KG: Don’t think it’s necessary to be better.
* AB: Agree with that, haven’t see lots of complains that folks have this problem with webgl.
* KG: **Think we should table this for now**.


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* AB: In middling of writing up JB’s analysis but didn’t get ot post draft. As DN mentioned we’re in favour. For things on table think it’s most forward looking and consistent.
* MM: We don’t understand it so can’t really make comment on it. Was surprised that the arguments against inout appears to be about proposal not made, tried to clarify to make more clear.
* AB: inout is historical at this point and not forward looking. Think there are lots of sharp edges that don’t get solved so don’t think it gets us all the way there. If you pass inout as array from storage buffer, does it copy back, does every invocation copy back? Issue from TR of inout of workgroup shared memory not working well in HLSL. How do you limit inout so it’s only applicable in places. In future with real pointers don’t want to carry inout. Rather slowly move to real pointers. Limited now and build on top of it.
* MM: The argument about HLSL inout as WGSL inout wouldn’t use HLSL inout. For the array argument the spec would describe as if. If spec has no alias then it’s ok, if aliasing’ is possible then as to copy in/out. Thats what the name means.
* TR: If it doesn’t use HLSL inout I thought the justification was to lack of HLSL pointers and use inout. Doon’t understand what it would use instead
* MM: Polyfilled. Wrote an example, function that takes inou would argument return to turn into struct and add in fields that does copies out. Caller would call and then decompose return and assign the outputs over the inputs.
* TR: This is instead of using HLSL inout it would do that if you used inout in wgsl.
* MM: Reason is claims that HLSL inout is not well ordered. Proposal makes well order behaviour.
* TR: Well ordered under aliasing.
* KG: Even non-aliasing conditions.
* TR: How is it observable under non-aliasing?
* KG: Maybe it isn’t?
* MM: Don’t think it is
* GR: by not well ordered you mean not written down?
* MM: Don’t know much about HLSL, in previous meeting raised possibly of WGSL inout matching HLSL inout. If behaviour matched then could just implement on top. Someone said HLSL inout has defacto behvaiour but docs don’t say that. So adjusted proposal.
* GR: Defensible decision. There is defacto behaviour across compilers
* TR: For alias?
* GR: For order of operations. Pass same variable into two inout the order is consistent.
* TR: Between FXC and DXC.
* GR: But again, not specified.
* TR: Not in detailed HLSL spec ….
* MM: The direction I’d like to go is have something be well defined in WGSl and if compiler believes it can implement the behaviour by using HLSL inout then it can. But the language for WGSL would be defined in terms of behaviour and not platform API features.
* AB: **Propose we delay as JB is not here**.
* KG: Table it.


### [wgsl: language evolution: managing rollout of core (essentially sugar) language features #3149](https://github.com/gpuweb/gpuweb/issues/3149)



* Previously: TODO: Shadowing overloaded builtins?
* MM: I thought we had a resolution on this last week. Are we not done here?
* KG: If we did not in minutes
* MM: Previous resolution, **because user types and user functions can shadow builtins we don’t have to solve now we can solve later**
* KG: Even for overloaded
* MM: Yes. **Any user function shadows all overload builtins**.
* KG: That’s fine. Didn’t make it into the minutes, so didn’t see it. **Resolved**.


### [Accessing an "invalid memory reference": Trapping behavior · Issue #3272](https://github.com/gpuweb/gpuweb/issues/3272)



* KG: Don’t know where to start. Guess one thing to mention is that I didn't’ think this used to be a good idea but see why it could be useful. Wrote out some examples for how this might effect to choose to implement translations. AB had example where you could do different transform to get optimization in the clamping case. Think what I’m worried about is, I believe optimizing compilers are very smart, at the same time don’t want to force ourselves to write smart optimizers. That’s why trapping seems neat. Easier to prove something traps and bail to preserve a fast past then it is to optimize a clamp required by everything executes behaviour.
* MM: Didn’t understand any of these code samples
* KG: Idea is max in is user program and we’re trying to translate to hardware.
* MM: Ok. maxin clamp, discard1, discard2 are potential compiler products.
* KG: Yes. In writing them out it felt like the trap simpler fast path optimized version was more straight forward then clamping. Which means it’s easier for a compiler to optimize away the clamps for most of the code and therefore have an easier path to safe but fast version of code executing vs requiring the operations to still take place but in a safe way. Counter balanced by that this does make the portability question worse. So, judgment call of is it valuable enough to take the portability hit for code that is doing the wrong thing.
* AB: Non local effects make me uncomfortable. Not a small hit, one out of bounds access i get nothing out of frame buffer so debugging becomes much harder. Feels like this would hurt users
* MM: That makes sense. Think the reason this is ok is because in other languages any out of bounds is worse that program stops running. The whole thing crashes or GPU reset or black screen which is very worse. Portability is way less urgent because the out of bounds will almost never happen. 
* AB: Don’t know, see shaders rely on out of bounds. Don’t buy it won’t happen
* KG: Especially given this is guaranteed for HLSL programs.
* AB: Common cases on windows. Doesn’t handle every corner on D3D but common ones
* KG: Out of bounds read/write is well defined
* TR: Depends on how resources are bound and the kinds they are.
*  KG: Something webgl also protects you from. Porting that code would be vulnerable.
* MM: One path forward, because this is option, compiler does not have to use it, would be legit to have browser implement and other browser not. Then one that does would see if it causes real world problems and then would stop doing it.
* KG: LIke letting the “bazaar” work it out. Think we’re in stage of spec development to see that happen. Would love to let implementations explore that space and get data to come back with how it works. Implementation experience would be valuable.
* AB: With non-local, something in vertex shader goes out of bounds you don’t render anything to screen. So far from effect of where it’s observed. If restricted to stuff on buffer would be more plaitable then whole program is broken. To me this is saying why not just make it undefined. That’s what this request is. Don’t list things just say it’s undefined.
* KG: Trying to do better then undefined
* MM: Not undefined behaviour. Does not mean it stops in well defined place with well defined accesses
* AB: It doesn’t stop
* MM: Stops at access
* AB: Program keeps running, still does derivatives, etc. Every invocation in workgroup stopping or draw call? Is everything in this code requiring everything is uniform? There are lots of concerns here and wish there was more data to open door. This isn't 1 or 2 behaviours this is very large due to non-local effects
* KG: Would love to know more which is why i want it investigated.
* AB: Think it’s fine to investigate but don’t think we want to change spec.
* KG: If folks did investigate and came back and said it was cool and didnt’ have problems. If we would do spec change then, then we’re at stage of spec development to go gather that data. Given where we are in spec dev you can do it. Don’t want to say go investigate and come back to say cool and then we say don’t want to do it anyway. If data came back as positive and was cool do we want to do that adn is there chance enough that might be the case and it did come back as good idea we’d want to continue.
* AB: If some way to signal it happened would be more comfortable. Lack of signal to user is troubling. Think was also JB’s opinion.
* MM: **One other thing, we can add this in and remove after we ship (any of us ship) but can’t not add this now and add later** 
* DS: Why not? We just add it later
* KG: Because we shipped the spec that way and can’t change the spec later. Hope would be that we would be able to give some signal to user i don’t know how much overhead that would be all the time. Easy to do in a debug mode in devtools. Could recompile shaders with more information to say this invocation had to bail because write to external destination before discard. Think there is a path towards communication to authors. Don’t think it’s feasible at runtime but during development.
* MM: Think that makes sense. Could say that when devtools is open a trap also writes into a separate channel that devtools record.
* KG: Even non-normative. 
* MM: Spec doesn’t mention and wouldn’t add new api on how to get error state. Behaviour is up to devtools of UA.
* KG: Would love to see this explored more.
* AB: … and compare to what ti would take to create the report and the cost. If it could always be recorded that’s more palatable. 
* MM: Could you write that in the issue. Don’t want to go down path that’s slightly different.
* AB: **Will add comment to issue.**


---


# 📆 Next Meeting Agenda Request



* Next week: Tuesday, August 23, **11am-noon** (America/Los_Angeles)
* 