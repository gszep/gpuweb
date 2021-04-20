# **WGSL 2021-04-20 Minutes**

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon

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
    *   **David Neto**
    *   Kai Ninomiya
    *   James Darpinian
    *   Brandon Jones
    *   **Ryan Harrison**
    *   Sarah Mashayekhi
    *   **Ben Clayton**
    *   **Alan Baker**
    *   **James Price**
    *   Rahul Garg
    *   Ekaterina Ignasheva
*   Intel
    *   Yunchao He
    *   Narifumi Iwamoto
*   Microsoft
    *   Damyan Pepper
    *   **Greg Roth**
    *   Michael Dougherty
    *   Rafael Cintron
    *   **Tex Riddell**
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
    *   **Jim Blandy**
*   Kings Distributed Systems
    *   Daniel Desjardins
    *   Hamada Gasmallah
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


### Office Hour



*   **Wednesday 10am-10:50am**
*   **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
*   Everyone welcome
*   Mass calendar invite will have been sent out
    *   If you still need an invite, add your email here:
        *   [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)


### VF2F



*   


## 

---



## Timebox


### [Require that vertex shaders return a position? #1567](https://github.com/gpuweb/gpuweb/issues/1567) 



*   MM: Makes sense to start this direction, can add flexibility later. No one’s begging for it, let’s be strict for now.


### [arrayLength acts on pointer-to-runtime-sized-array #1330](https://github.com/gpuweb/gpuweb/pull/1330) 



*   Already fixed!


### [Add transpose() builtin #1625](https://github.com/gpuweb/gpuweb/issues/1625) 



*   MM: yes, please
*   JG: agreed


### [ Support mix(vec, vec, f32) #1588 ](https://github.com/gpuweb/gpuweb/issues/1588)



*   Yes, add this overload


## 

---



## Discuss


### [Need a proposal for specialization constants on the API side. #1608](https://github.com/gpuweb/gpuweb/issues/1608)



*   [Discuss in WGSL group: should specialization constants be identified by name instead of by numeric ID?]
*   MM: Because these are per-entrypoint, the compiler can generate ids e.g. for spir-v.
*   DN: Concern: Both MTL and spir-v, spec-consts are identified by id not names, so we’d be moving away from prior art. Worried that e.g. and engine will then need to adapt to names instead of spec-const-ids. When round-tripping between shading langs, we’d need to preserve the mappings too.
*   DM: Moving away from prior art as undesirable is convincing to me
*   MM: Metal supports both ids and idents
    *   [https://developer.apple.com/documentation/metal/mtlfunctionconstantvalues?language=objc](https://developer.apple.com/documentation/metal/mtlfunctionconstantvalues?language=objc)
*   MM: Argument for using strings: JS gets more readable, don’t have some arbitrary number.
*   JG: Do we want both then?
*   DN: I want to talk to customers about it, see how they feel about it.
*   MM: I could see a language translator being able to provide the mapping it uses
*   DN: Yeah, though translator isn’t being used in isolation, but rather within context of an engine.
*   MM: Let’s continue next week.


### [Specify WGSL as UTF-16-encoded text. #1626](https://github.com/gpuweb/gpuweb/pull/1626)



*   MM: Technically correct: “We don’t need to specify an encoding”. But, we may want to spec an encoding for ecosystem reasons. Tools and standalone shader files might otherwise pick randomly, to the detriment of the ecosystem. We could simplify this by blessing a particular encoding
*   DN: I agree, as long as the encoding we choose isn’t “bad” :) Also let’s be careful about the language we use here: Strong recommendation without saying that we need/use it for WGSL in the browser?
*   BC: But why should the language forbid other encodings? All we need for WGSL is “we support unicode”, and tools are welcome to do what they want. Forbidding in the language spec feels like overstepping the bounds of specification here
*   DN: Ok, I think we should “allow” other encodings
*   MM: Ok, but I’d like to 
*   BC: “tooling MUST accept &lt;encoding>”
*   DN: Can we agree that tools MUST support utf-8?
*   MM: Related question, error messages have offsets, which are in code units. It’s ok if the diagnostics are in utf-16, even though the page is utf-8.
*   BC: Diagnostics position/offsets are always in the units that correspond to the encoding provided for the shader source. So they scale the same as the source presented.
*   MM: Sounds good.
*   BC: Ok, what about identifiers?
*   MM: Probably in a year or so?
*   JB: Rust just recently had an RFC for this
*   MM: Post-MVP though, even when we talked about this long ago
*   BC: Yeah, prefer to keep that post-mvp
*   MM: Unicode in comments probably? Even if everything else must be ascii?
*   JG: Do we have unicode comments today?
*   MM: Yes, and furthermore it’d be extra code to reject ascii.
*   DN: Straw man “UTF-8 is always a valid encoding for a WGSL program”
*   


### [Add the uniformity analysis to the WGSL spec #1571](https://github.com/gpuweb/gpuweb/pull/1571)



*   RM: Sorry, been sick. New patch soon to add things that people had been requested, like “is this point/line in uniform flow”. Nothing to report right now though.
*   DM: Talked about with AB: One problem is that reconvergence is at the draw-call level. (AB: Compute is workgroup) So we wouldn’t be able to treat flat interps as uniform still?
*   AB: Difference between uniform flow and uniform vals
*   MM: Whatever we do for UA, we have to respect that the APIs don’t give us better reconvergence guarantees.
*   DM: I think it’s more complicated than that, where we may want better/stricter analysis. Can look later into making it more flexible.
*   MM: I’m excited for a tool that colors lines green/red for non/uniform.
*   DM: Just waiting for RM then? We have *an* alg implementation, but not RM’s.
*   AB: If I had a tool that broke up an implicit lod lookup, and moved the derivative out into UCF, is that a valid transform? Or would we have to reject it?
*   RM: I think if this is a tool, then it would have been accepting non-spec WGSL, and transforming it into spec-compliant WGSL
*   TR: I think you’re talking about the same transform that fxc does, where it may hoist out the derivative. But it wouldn’t convert implicit to explicit
*   AB: I think Vulkan specs implicit based on explicit, though implicit may be faster
*   RM: The spec would declare it as non-valid WGSL and browsers would reject, but a tool could accept a non-WGSL thing and transform it.
*   AB: So anything passed to createShaderModule would need to respect the spec here (yes)
*   DM: Do we need to talk about private globals that naga/tint generate?
*   RM: I think it would be useful to have a keyword that requires UCF, and I think this would satisfy that problem.
*   GR: Should we generate an error for non-UCF, or should we transform it so that it is?
*   RM: non-UCF would be an error
*   MM: This is an opt-in/assert.
*   RM: This would also help with global variables if we could annotate them requiring that they are UCF-only
*   GR: Reminds me of “what level of uniformity are we talking about” Per quad? Per call?
*   RM: Like DM said, can run UA on different levels, but we need to agree on what level(s?) to require/run.
*   GR: I find it some confusing that “uniformity” here is different compared to “uniform” buffers.
*   RM: Is this different from our native APIs? I think everyone is confusing
*   TR: If we have `uniform`, it should maybe have an adjective prefix, like quad-uniform.
*   TR: Also we don’t have subgroups yet, so should we talk about subgroup-uniformity there? (maybe?)
*   TR: With the assertion, if they don’t have a way to ask for an always-uniform value in subgroup (‘first-lane’?)
*   AB: I anticipate an ext for dynamic uniformity
*   MM: Many different names for make-uniform, but because it only work on a simd-group, not sure it makes sense to add now, since it doesn’t match the scope of uniformity that we’re working on right now. I think we should only add simd-groups later, when we’re ready for the whole feature.
*   JG: So we have diff levels on U, and I think we can just run the UA on different levels of U, and I think we’ll want this for our different levels of U.
*   RM: Yeah, that works, though I’m not sure myself what levels we want/need, but UA will work. (as long as we don’t have toooo many U levels!)
*   AB: Derivative requires quad-uniformity But in SPIR-V once you’ve diverged, you only reconverge for the whole invocation-group which is essentially the whole draw.
*   TR: We also don’t have a make-quad-uniform thing, either, so we generally can’t anyway
*   AB: I don’t think other specs make this guarantee either.
*   JG: What about on de-convergence? Can you go from UCF to non-Invoke-U but still quad-U?
*   AB: Yes but it falls apart quickly.  \
// uniform control flow \
If (non_uniform_cond) { \
  // known to be quad uniform \
  If (non_quad_uniform_cond) { \
    // … \
  } \
  // no longer quad uniform in SPIR-V \
} \
// uniform control flow


### [consider pulling 'access' into the type of a pointer/reference #1604](https://github.com/gpuweb/gpuweb/issues/1604)



*   (tabled)


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-04-27 (like normal)
*   Out-vars
*   