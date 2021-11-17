# WGSL 2021-11-16 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon Americas/Los Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Myles C. Maxfield**
    * **Robin Morisset**
* Google
    * **Alan Baker**
    * **Antonio Maiorano**
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * James Price
    * Rahul Garg
    * **Ryan Harrison**
    * Sarah Mashayekhi
* Intel
    * Jiajia Qin
    * Jiawei Shao
    * Shaobo Yan
    * Yang Gu
    * Zhaoming Jiang
    * Yunchao He
    * Narifumi Iwamoto
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * Rafael Cintron
    * **Tex Riddell**
* Mozilla
    * **Dzmitry Malyshau**
    * **Jeff Gilbert**
    * **Jim Blandy**
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* **Antonio Maiorano**
* **Dominic Cerisano**
* Eduardo H.P. Souza
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Michael Shannon
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


---


# ⏳ Timeboxes


### [Go from attribute_list * to attribute_list ? in the grammar #2263](https://github.com/gpuweb/gpuweb/pull/2263)



* RM: **cancelled/closed**, as David and Kai have convinced me that the current grammar is better for code generators.


### [Should WGSL expose the concept of coherent buffers? · Issue #1621](https://github.com/gpuweb/gpuweb/issues/1621)



* Related to #2055?
* AB**: **Indirectly related to 2055.  The way the mapping works, is all buffers are “coherent” by default. (Tint doesn’t implement that yet).  Question: do we want option to be non-coherent buffers, which is the default in OpenGL and D3D.
* AB: I think Metal is always coherent?
* MM: Discussed with Metal team. It’s maybe exactly correct to say that buffers are always coherent. Don’t need atomics to pass info within a workgroup..  Correct prog without atomics to do normal writes to device buffer and workgroup memory, then do a barrier with correct flags.
* AB: That matches what mapping says now.  And later fix says you can’t do inter-workgroup communication.
* MM: That’s correct for Metal.
* AB: GL and D3D make it optional for “coherent” to be specified, as there is some cost to it.  Coherent by default does match programmer expectation, but it’s up to us whether it’s optional.
* MM: coherent by default is the right design for WebGPU for now.  Could consider extension to allow non-coherent for buffers. But not make the cut for MVP.
* JG: Would want to have to go out of your way to declare it as not coherent. (likes “incoherent” , haha.)
* AB: Could call it “private?
* **JG: sounds like consensus: coherent by default, and consider late an extension to allow non-coherent.**


### [Resolve issue: No other chara himcters count as blankspace #2282](https://github.com/gpuweb/gpuweb/pull/2282)



* DN: We still have vtab/linefeed, but let’s not add more (e.g. unicode) separators
* MM: I’m fine with this. Not aware of other languages that allow more separators, and we shouldn’t innovate here.
* **Merge**


### [wgsl: should NUL characters be permitted in source code? · Issue #2294](https://github.com/gpuweb/gpuweb/issues/2294)



* DN: Saw this while reading the treesitter lexer, which uses a null char as a terminal sentinel. Seems not useful?
* **No, needs spec**


### [A block comment must affirmatively end before end of source #2295](https://github.com/gpuweb/gpuweb/pull/2295)



* DN: Wasn’t  clear in previous consensus.  Requiring affirmative closing makes it more reliable/easy to glue together snippets.
* **Merge**


### [wgsl: add unary plus? #2279](https://github.com/gpuweb/gpuweb/issues/2279)



* MM: Not valuable.
* **Let’s close.**


### [Is it okay that isInf() and isNan() can return false for infinities / nans? #2270](https://github.com/gpuweb/gpuweb/issues/2270)



* JG:  (gives background). Fast math can make isnan() turn into always-false. (!)
* JG: Like Robin’s idea of removing now, and later adding an extension that adds preservation of inf and nan and than adds the functions back again.
* MM: Philosophically adding false information is worse than not adding information.
* DM: Can we possibly detect these on fast-math?
* BC: Even if you polyfill, the compiler can still assume it doesn’t happen, and will melt it away.
* JG: I think that even “loading directly from memory” isn’t enough
* MM: Yes, can still get false negatives.
* DN: So when translating spir-v, when I see these opcodes, what do I do? Inject false? (JG: yes)
* MM: I think it’s up to implementation. Like, you can (try to) check the bits, that’s up to you. But rather that we shouldn’t volunteer known-flakey builtins.
* AB: Would we mind if multiple extensions added these same builtins?
* MM: Which multiple extensions?
* AB: I could see multiple precise-math approach extensions might both re-introduce these.
* MM: It would be unfortunate if we ended up with e.g. three subtly different extensions for precise math.
* **AB: We should figure out what our requirements for releasing/exposing extensions are. (AB to file issue)**
    * **(Filed: [https://github.com/gpuweb/gpuweb/issues/2310](https://github.com/gpuweb/gpuweb/issues/2310))**
* MM: Did I hear DN say we have is_normal?
* DN: Yes, since zero is non-normal. And there are also denormals. Denormals are not normal.
* JG: So don’t some systems maybe-flush subnormals?
* AB: Yeah, though if you load from memory it probably is preserved, just not for arithmetic.  Is_normal is basically “is exponent not one of the extremes”.
* MM: So doesn’t that put us in the same spot as isinf?
* JG: It sounds like it’s more reliable than isinf.
* AB: Review of all platforms.  Can flush demormals on arithmetic operations.  Vulkan phrases is “these operations preserve denormals”.. HLSL says “data movement” preserves denorms. Metal may have similar language. The language on underlying implementation is “implementation may assume that nans and infinities do not occur”. And with that the compiler will affirmatively remove function calls like isnan, isinf. My testing shows that it’s actually sometimes reliable and not always.
* **MM to file issue on whether to keep isNormal**
* **Needs-Spec for removing isInf/IsNan**


### [Allow identifiers to start with _ · Issue #2298](https://github.com/gpuweb/gpuweb/issues/2298)



* DN: Reporter seemed ok with dealing with their transpilation case, after we explained our reasoning.
* DM: I think it would be better to allow this, especially since so many other languages allow this.
* RM: I think if we allow _ we should instead reserve _ _ 
* GR: We could always continue to reserve $
* AB: Conflicts with frexp, modf results that have user-unnamable structs. Would have to change those.
* ...
* JG: It’s a papercut, but we can ship with it and things will work fine. Can we postpone discussion?
* **Retarget for post-v1**
* DM: Can we do a census on our corpus for this pattern?
* MM: I can do that
* MM: Just ran it, on 2000 files, 800 lines appear to use this pattern
* DM: This sounds important then
* JG: People use it, but our question is not “will people use it if available”, but rather “will people be sad if they can’t use it”, which is subtly different.
* JG: Anyone can re-add this to a meeting agenda if they want to re-discuss this.


---


# ⚖️ Discussions


### [Fixes to memory model for barriers and atomics #2297](https://github.com/gpuweb/gpuweb/pull/2297)



* Updates:
    * Had to weaken barriers because in Metal threadgroup_barrier only provides a memory ordering between invocations in the same threadgroup. (That’s not clear at all from the docs; OpenCL 1.x has the same problem.)
    * Other updates were editorial.
    * (David was tempted to land this directly…)
* AB: Effectively this is the only way it can work.  Documents a limitation in an underlying platform.
* RM: Agree, should land it unless someone sees an issue with it.
* TR: There’s a comment at the start saying translations to HLSL will over-synchronize buffers. Don’t think that’s true? Or something else.
* AB: Maybe over-synchronize is the wrong word.  HLSL memory scope is larger than what WGSL will offer. 
* TR: So because UAV is device memory shared across a whole device, it may eventually flush to other workgroups, but WGSL doesn’t guarantee as much as that.
* AB: Difference is that in Metal you won’t get that flush across workgroups, but in HLSL you will get that flush across workgroups.
* TR: So you’re saying you can write code that can access locations in the same global buffer, but will be independent?
* AB: If they are non-atomic accesses, and at least one is a write, then it’s a data race. And that’s undefined behaviour.
* AB: In my mind a control barrier has 4 essential parameters
    * Control scope:  workgroup: which invocations wait for each other.
    * Memory scope: what set of innovations will see the flushing of memory operations. Could be workgroup, or could be device.
    * Storage semantics: what kinds of memory does the cache invalidation/flushing apply to?  Workgroup memory or device memory or image memory.
    * Ordering:  e.g. Relaxed (for atomics), or acquire-release.  (used in barriers, and C++11 atomics.)
* TR: There’s different scopes for UAVs.  there are “withSync” variants.
* AB: My understanding of the “withSync” variant is that these are control barriers in addition to memory fences.
* TR: There should be a barrier intrinsic that does UAV barrier at workgroup memory scope, except it’s not as granular for buffer types.
    * AllMemoryBarrierWithGroupSync
        * AB: That’s shared and device 
        * TR: Yes
    * GroupMemoryBarrierWithGroupSync.  
        * AB: I missed those in review.
    * TR: DXIL may have more options than HLSL does.  The HLSL level doesn’t expose the UAV with workgroup memory scope.
        * AB: Ok, so that’s why we didn’t notice it.  We’re generating HLSL in Tint, and have too-big memory scope.  We use DeviceMemoryBarrierWithGroupSync (?)
        * AB: In future we’ll use the more narrow one.
* JG: Sounds like we’re** Approved to Merge**
* MM: After this, how many variations are we going to have in WGSL
* AB: Two, though internal feedback says one is poorly named.
* MM: What’s the difference between them?
* AB: Workgroup vs device memory
* 


### [[proposal] Changes to overridable constants · Issue #2238](https://github.com/gpuweb/gpuweb/issues/2238)



* Previously:
    * JB: Can we defer until DM is back next week? (yes)
* **Revisit next week**


### [Literal unification, bottom-up #2227](https://github.com/gpuweb/gpuweb/pull/2227)



* Previously
    * RM: counter-proposal is in the middle of being written, requests an extra week to finalize it
    * [https://github.com/gpuweb/gpuweb/pull/2306](https://github.com/gpuweb/gpuweb/pull/2306) posted yesterday
* RM: Goal is that each literal eventually gets a WGSL type inferred for it. As if it had been written with an explicit suffix.
* AB: Think we can discuss philosophical differences in whether operations on constexpr can happen in a more-precise arithmetic space.
* RM: Agree that’s my entire issue with the 2227 proposal.
* BC: I gave a lengthy reply discussing this.  The example Robin gave was that a reordering of expressions can behave differently based on quantization to f32.  Goal of using higher precision space is to avoid this quantization.  The proposal to deal with higher precision space is that people writing the expressions don’t have to worry about the quantization. You care about it as a mathematical expression being correct, not about how it’s quantized mid-way through an evaluation.
* RM: Objection to that is when going from an expression that is all constants, to some using variables, can completely change the semantics. If you say developers find it easy if everything is infinite precision, I disagree. They have to think about precision issues.
* MM: They’re not infinite precision actually. Using “doubles” doesn’t solve the desire to have infinite precision.
* BC: Go uses 256-bit numbers for this type of thing.  Not sure if in a 32-bit world that we’re in now, doubles would *not* be enough.
* MM: Example: maxfloat * 1000 / 1000.
* BC: Would suggest if you overflow the high precision internal type, then that produces a compile time error.
* MM: Internally, Robin said 2227 proposal would have two distinct languages: operating on ideal types and concrete machine types.  Seems counterintuitive for developers and implementors.  If rule is that intermediate causes overflow is an error, that’s very different than what happens on GPU: enforces that it’s really two different kinds of arithmetic.(And hence bad)
* BC: I don’t disagree they are distinct. They are different arithmetic spaces, and that’s a good thing.  I posted about my thoughts in the issue.


---


# 📆 Next Meeting Agenda



* Next meeting: **NOT 2021-11-23, but rather 2021-11-30** 11am Los Angeles time
    * Do we want to meet nov23? US Thanksgiving week
    * Apple reps will be absent
    * **Let’s cancel nov23, and plan for nov30**
* #2305 [wgsl: Add vector and matrix constructor overloads that infer component type](https://github.com/gpuweb/gpuweb/pull/2305)  [wgsl](https://github.com/gpuweb/gpuweb/issues?q=is%3Apr+is%3Aopen+label%3Awgsl)
* 