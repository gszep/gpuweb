# **WGSL 2021-04-27 Minutes**

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
    *   **Kai Ninomiya**
    *   James Darpinian
    *   Brandon Jones
    *   **Ryan Harrison**
    *   Sarah Mashayekhi
    *   **Ben Clayton**
    *   **Alan Baker**
    *   James Price
    *   **Rahul Garg**
    *   **Ekaterina Ignasheva**
*   Intel
    *   Yunchao He
    *   Narifumi Iwamoto
*   Microsoft
    *   Damyan Pepper
    *   **Greg Roth**
    *   Michael Dougherty
    *   **Rafael Cintron**
    *   **Tex Riddell**
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
    *   **Jim Blandy**
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
*   Timo de Kort
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



*   (In a month?)
*   


## 

---



## Timebox


### [Support NumWorkgroups builtin #1671](https://github.com/gpuweb/gpuweb/issues/1671)



*   DM: It is in SPIRV, is it also in MSL?
*   MM: yes
*   AB: Also in HLSL WorkgroupID 
*   DM: Related: [https://github.com/gpuweb/gpuweb/issues/1590](https://github.com/gpuweb/gpuweb/issues/1590) 
*   DN: Interested in finding out more about status here
*   MM: No strong pref, emulation would be fine
*   DM: Think should keep it, but need clarification from apple on when+where this is available
*   GR: (spirv-cross polyfills this on hlsl)
*   MM: Should we implement this or require users to? It seems like as a builtin, it would be at least as fast as a user polyfill
*   DM: Does the system ever decide this?
*   MM: Not today, but we might (probably will) want to add something like this in the future
*   KN: From tensorflow.js, originally did 1x1x1 and expected compiler to batch them, but it doesn’t do it automatically. Would be good to help users out here, even if we can tell them “oh just use 8, 8 is good”
*   DM: Don’t know how they’re using workgroup memory, but we can estimate based on register usage
*   AB: Not sure how you’d estimate reg usage. Could poll vulkan and offer something to authors, e.g. make it a multiple of subgroup size.


### [Update grammar for constexpr type constructors #1675](https://github.com/gpuweb/gpuweb/pull/1675) 



*   _MERGED_


### [Add invariant attribute #1673](https://github.com/gpuweb/gpuweb/pull/1673)



*   AB: Intent is to be vague.  Vendor documentation is very wish-washy. Don’t want to rat-hole down this path too far.
*   MM: Can we map the meaning to specific keywords/concepts in underlying languages?
*   JG: Can do that non-normatively.  We know it’s true.
*   RM: Non-normative note sounds good.
*   MM: Would like to get something more than what’s there.
*   AB: Non-normative reference is easy.  I can take another crack at writing the normative text. E.g. something about the cone of influence….
*   JG: Want to make it clear that consistency applies within the same entry point too.
*   AB: Suggest consistency of different pipelines.
*   JG: It’s not what underlying APIs give us.  Ideally want someone to come away with a cosmic feel of horror. The guarantees are pretty low. E.g. same pipeline under different conditions.
*   AB: Could make invariant the default.
*   MM: Metal team found it caused pessimization on performance.  So it’s opt-in for Metal.
*   JG: ESSL 3 spec has some background/history/motivation.
*   JG: I’ll propose text as followup.  Please add the non-normative reference.
*   AB: What I wrote is a reduction/simplification of underlying languages.


### [Add function restrictions #1674](https://github.com/gpuweb/gpuweb/pull/1674) 



*   AB: The contentious part is whether you can pass a handle by pointer.  I left that in.   Otherwise, the restrictions reflect basic SPIR-V.
*   MM: At office hours, discussed that maybe if we allow texture pointers, it might break some of the analysis we want to do
*   DN: For texture filtering? (yes) How does that leak into WGSL?
*   MM: If possible to pass by ref, at the point that you compile, the pipeline layout tells you the inner type of the texture and whether it’s sampleable. If textures are named specifically, we can do this validation.
*   DM: Yes we need to know what textures are used by which samplers
*   MM: Can we add more types for this?
*   DM: We had that proposal earlier, and we didn’t want that at that time
*   DN: What’s proposal for passing tex/samplers to functions?
*   MM: By pointer would be great. If we want to keep the analysis, I think we wouldn’t be able to do this, rather you’d have to name it. I wish we could fix this. Statically determining tex/sampler at compiler time is valuable.
*   KN: I thought it wasn’t just analysis, but also some hardware needs to be able to determine this.
*   JG: Well, spirv isn’t really that flexible. Can determine static-use
*   KN: Could do e.g. unrolling to figure these out
*   MM: Let’s see if we can figure this out offline
*   DN: Can we land this without the contested bits?
*   


### [Relax the out-of-bounds specification #1649](https://github.com/gpuweb/gpuweb/issues/1649)



*   This should be needs-spec
*   MM: Can we talk about OOB access derailing the whole draw call? (yes, but next week)


## 

---



## Discuss


### [Need a proposal for specialization constants on the API side. #1608](https://github.com/gpuweb/gpuweb/issues/1608)



*   DN: Proposal from email:
    *   In writing the above, there may be a path to "why not both", if we want it.
    *   Thinking out loud, here's an example:
    *   
    *   [[override]] let loopIteraitons: i32;                    // Must be set by name "loopIterations"  in the API
    *   [[override(foobar)]] let myMangledName: i32; // Must be set by name "foobar" in the API
    *   [[override_id(0)]]    let blockSize: i32 ;             // May be set by ID 0 in the API, or by name "blockSize”
*   MM: If porting, probably use ints, if writing WGSL, probably want strings. Three different ways, I think they’re good and have value.
*   DM: Proposing allowing both? (either) Can we have less of design by committee and just pick one?
*   KN: This is what metal uses, right?
*   MM: Yes, though they all have ints, you just don’t have to use the int instead of the str
*   AB: Is there strong reason to allow renaming?
*   MM: A weaker one is that it’s an optional level of indirection.
*   AB: Already have indirection with ints?
*   MM: Yes, but the weak reason is that if you have a typo your shader is less likely to run.
*   JG: Describe typo?
*   MM: If you use e.g. 3 instead of 4 will run, but be wrong. Rather, “Myless” will more reliably be an error earlier.
*   JB: Similar to function args by position vs by name?
*   AB: But why allow renaming?
*   MM: We can compromise and drop `override(foobar)` for now
*   DM: Converging on just `[[override]]` and `[[override_id(n)]]`
*   KN: So we get a record where the key can either be the name or the number?
*   MM: Can the key be a non-string?
*   KN: JS record keys have to be strings. What this means is that `override_id(3)` is really a key of str(‘3’) anyways.
*   AB: let’s use [[override]] and [[override(0)]] (rather than [[override_id(0)]])
*   Everyone: ok


### [consider pulling 'access' into the type of a pointer/reference #1604](https://github.com/gpuweb/gpuweb/issues/1604)



*   


### [Add the uniformity analysis to the WGSL spec #1571](https://github.com/gpuweb/gpuweb/pull/1571)



*   Anything new?
*   RM: Not this week. One thing to add, have been talking with metal team, and they feel that the various levels of uniformity would be handleable with the analysis.
*   MM: Trying to get a handle on the kinds here, so that we can have a better understanding of which of these passes need to be done.


### [Support NumWorkgroups builtin #1671](https://github.com/gpuweb/gpuweb/issues/1671)



*   (again)
*   JG: Leaning in the direction of having the builtin even if we sometimes polyfill
*   DM: Yes, only on one API, and might be more efficient for us to do than authors
*   Resolved: yes


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-05-04 (like normal)
*   Out-vars proposal? (jgilbert?)
*   [Relax the out-of-bounds specification #1649](https://github.com/gpuweb/gpuweb/issues/1649)
*   Access qualifiers [#1604](https://github.com/gpuweb/gpuweb/issues/1604)
*   Will have an email convo about passing textures to functions, hopefully more on this next week
*   