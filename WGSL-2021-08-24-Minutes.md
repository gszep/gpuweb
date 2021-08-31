# WGSL 2021-08-24 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** Jeff Gilbert, David Neto

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Myles C. Maxfield**
    * **Robin Morisset**
* Google
    * Alan Baker
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * **James Price**
    * Rahul Garg
    * **Ryan Harrison**
    * Sarah Mashayekhi
* Intel
    * Narifumi Iwamoto
    * Yunchao He
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * **Dzmitry Malyshau**
    * **Jeff Gilbert**
    * **Jim Blandy**
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Dominic Cerisano
* Eduardo H.P. Souza
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Pelle Johnsen
* **Timo de Kort**
* Tyler Larson


---


# 📢 Announcements


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)
* MM: Did the APAC one happen? (yes)
* MM: Is it weekly? In lieu of the week’s office hour? (additional, weekly)
* MOD: It’s weekly.


---


# ⏳ Timeboxes


### [Restrict statements from matching empty string #2050 ](https://github.com/gpuweb/gpuweb/pull/2050) 



* (already merged)


### [Require storage texture access #1996 ](https://github.com/gpuweb/gpuweb/pull/1996)



* DM: Grammar was inconsistent. Can either fix the grammar, or change it to not be optional. My preference would be to require it to be explicit
* DN: +1, can change our minds if we require it for now
* (consensus: merge)


### [ Missing atomicSub function #2019 ](https://github.com/gpuweb/gpuweb/issues/2019) 



* DN: We were avoiding polyfills here, but everyone has it but hlsl, so let’s just polyfill there?
* MM: Sounds good, browsers’ polyfill would be just as good as the authors’.
* Consensus: Needs spec!


---


# **⚖️ **Discussions


### [Use brackets for array types #854](https://github.com/gpuweb/gpuweb/issues/854)



* (We should come back to this [this week] after more thought)
* RM: I have a proposal for this one. Seems like the main issue is with multidimensional arrays, and whether this should have C or java semantics. Maybe we should just forbid this for not? E.g. require using array&lt;> syntax if you want multidimensional arrays, but retain syntax sugar for single-dimensional arrays.
* MM: It’s subtle too, since we’re not forbidding multidim arrays, but rather it forbids writing `foo[A][B]`.
* RM: And it would just be forbidden in the grammar, and we could have a nice error message if people tried to do this.
* DM: Could this argument be applied to e.g. pointers, where we might be pressured to use c-style pointer syntax?
* MM: I think the cost-benefit trade-off is different, so it wouldn’t be compelling there.
* DM: Well, e.g. Mozilla uses a lot of array&lt;> style containers instead, so the c-style familiarity argument is weaker in practice
* MM: MSL has int[5] and Array&lt;int, 5>. Int[5] has reference semantics, and Array&lt;int, 5> has value semantics
* GR: In HLSL, conceptually int[5] is copied in and copied out, but optimizations make it more efficient than this would seem to suggest
* DM: I feel like we don’t even have a majority among our backend languages.
* MM: Most shader source code that I’m aware of is written in HLSL or GLSL. That’s what I’m leaning toward.
* JG: I like the sugaring proposal, especially with the restriction on multidim arrays. There are a bunch of languages people use int[5] in, so it’d a decent desire to allow this sugaring.
* [...]
* DM: Where does this leave us for stride on the type vs on the var?
* MM: I think the sugar is just for the common cases, and you can just spell it out if you need e.g. stride.
* DM: If HLSL is by value, I’m fine with this being that way in WGSL
    * Edit: indeed [by value](http://shader-playground.timjones.io/968327b868a3bbe15cb8686c929bae6e)
* DN: I think HLSL is like GLSL, where it’s copy-in-copy-out, with all its edge cases. I think I’m somewhat concerned about making choices based on how HLSL behaves. I think cribbing from how other languages behavor “usually” is risky, but I’m not disagreeing with DM’s conclusion.
* DN: I think we’ll need to discuss this more internally.
* DN: Stepping way way back, I think there’s a split between being like JS or TypeScript, and that this is sort of related to that?
* MM: This proposal doesn’t change the reference/value semantics of arrays. This proposal is just changing how to spell the type. No semantics change. I believe we have also forbidden the kind of aliasing that causes your hesitation.
* Come back to this next week.


### [[wgsl] Support compile-time-constants (constexpr) · Issue #1272](https://github.com/gpuweb/gpuweb/issues/1272) 



* MM: [highlighting details from his proposal]
* DM: Implementation burden. Eg. all the constant folding.
* MM: Depends if it’s observable. E.g. constexpr that sizes an array.  Compiler is required to compute the value at compile time.
* JG: Stops short of user functions as constexpr.
* MM: Right. Would be a big can of worms to tackle for now.
* MM: This proposal doesn’t mark a function as constexpr. It’s just a list in the spec.
* DM: Nonportability is expected.  It’s not intuitive.
* MM: Logic is you already get different results for different machines anyway.
* MM: If you limit to integral, then it would be portable. But it would be unwise for shaders.
* BC: CTS tests have to accommodate both constexpr and not.
* DM: It’s counterintuitive. When I define constexpr, I expect it to be “just a number” outside the program.  E.g. if pi is different. People don’t expect that.
* MM: So you’d want a rule that the compiler _must_ evaluate at compile time, and that it _must _be the same.
* DM: Just trying to see tradeoffs here.
* JG: Do worry about requiring too much exactness.  E.g. would limit the implementation for something like matrix inversion.
* DM: Inversion is an extreme, but other cases exist.
* MM: stuck to “math”-like things.
* JB: Can always add to the list later.
* BC: Texture offset parameter was a motivator.
* MM: Cases where it’s used: array sizing; case statements; offset to textures; index into array-value (let-declared array).
* DN: I’m concerned that we don’t have a lot of CTS tests for what we already have, yet we’re proposing a bunch of new tests for this
* BC: I do think that the complexity add for CTS tests would be minimal, since we should just be able to toggle between constexpr and not.
* MM: So argument is that set of testing beyond the toggleable bit isn’t huge? (yes)
* BC: It’s definitely significantly more work for us to do for MVP
* MM: Reason this came up was for dynamically sized workgroup thing, yes? 
* DN: Heard from internal discussion that because *this* doesn’t include overrideable constants, then that makes it less attractive to me.
* MM: What did you want to solve?
* DN: Want to size the workgroup data array sizes by the pipeline-overrideable constant for the workgroup size.
* MM: You would like this proposal more if pipeline-overrideable were constexpr (yes)
* BC: I think constexpr also makes things much nicer for texture fetch offsets too.
* DM: Didn’t we discuss before that constant offsets could not be pipeline-overrideable? 
* DN: Yes, so there’s sort of two levels here.
* JG: `pipelineexpr`?
* MM: Could take every pipeline-overrideable constexpr and make its dependencies also pipeline-overrideable in our implementation backends
* DN: Yeah, maybe?
* MM: Not sure if that’s implementable, though.
* JG: Are we ready for writing spec text?
* MM: Based on this convo, not yet.
* JG: Should we come back to this next week?
* DN: I think we should see what this looks like in implementation, as well as discussing more internally. I don’t know if we’ll have time to do enough implementation here to have feedback soon. Further, as DM said, maybe this isn’t M enough for MVP.
* GR: If this is primarily about texture offset, well tex offset isn’t super important, so it’s worth weighing how useful it would be vs how much it would contort the language.
* MM: There are four usecases, so it’s not just tex-offsets.
* DN: All four are actually ints, for what it’s worth
* MM: It would make me some-frowny-face about not having floats because of the proposed performance benefits of compile-time
* DN: I’m not that convinced by the perf usecase
* JG: Sometimes people do float math and then pull an int out of that, so floats may be useful even if only ints are needed for constexpr usages
* GR: Tex offsets can’t be pipeline-overrideable right? (yes) If that’s what we’re looking at, that’s a problem. Tex offsets needing to be [very primitive? Literal?] is a really old restriction
* BC: I do think tex-offsets are useful, and I have used them in the past
* DN: Clspv translates OpenCL C to Vulkan SPIR-V. CL C has pointer-to-local argument that is sized more dynamically (actually at enqueue time but in Vulkan would be pipeline-time.).  Clspv remaps those to module-scope workgroup variables with a specializable size, then informs the application (sideband data) to say “set this size to the array size you wanted; rather than what you would have set a enqueue time”. So that’s proof that the mapping works one way; I suspect strongly that a useful feature can be mapped the other way (so Metal style to Vulkan style), with the restrictions we already know we have to have.  I want to think more about MM’s idea of elaborating out the overridable constants and doing some of the constexpr math in the pipeline creation step. (So put part of the “compiler” in the pipeline creation…)  Want to think over the next week.
* JG: Let’s come back to this next week
* 


---


# 📆 Next Meeting Agenda



* Next meeting: 2021-08-31 (like normal)
* [https://github.com/gpuweb/gpuweb/pull/2069](https://github.com/gpuweb/gpuweb/pull/2069) Fix description of mix builtin; fix fma (works on vectors too)