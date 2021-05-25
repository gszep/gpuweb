# **WGSL 2021-05-18 Minutes**

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
    *   Myles C. Maxfield
    *   **Robin Morisset**
*   Google
    *   **Alan Baker**
    *   **Ben Clayton**
    *   Brandon Jones
    *   Corentin Wallez
    *   **David Neto**
    *   Ekaterina Ignasheva
    *   Kai Ninomiya
    *   James Darpinian
    *   **James Price**
    *   Rahul Garg
    *   **Ryan Harrison**
    *   Sarah Mashayekhi
*   Intel
    *   Narifumi Iwamoto
    *   Yunchao He
*   Microsoft
    *   Damyan Pepper
    *   Greg Roth
    *   Michael Dougherty
    *   **Rafael Cintron**
    *   **Tex Riddell**
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
    *   **Jim Blandy**
*   Kings Distributed Systems
    *   Daniel Desjardins
    *   Hamada Gasmallah
    *   Wes Garland
*   Dominic Cerisano
*   Eduardo H.P. Souza
*   Joshua Groves
*   Kris & Paul Leathers
*   Lukasz Pasek
*   Matijs Toonen
*   **Mehmet Oguz Derin**
*   Pelle Johnsen
*   **Timo de Kort**
*   Tyler Larson



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
        *   


### VF2F



*   Survey soon!
*   


### API Meeting Takeover?



*   Next monday (May 24)
*   No, because it is a Canadian Holiday


## 

---



## Timebox


### [[wgsl]: textureDimensions() returns vec3 for cube textures · Issue #1345 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1345)



*   BC: Not strong feeling, but vec2 might be the worst of the possibilities, since w=h and 6 layers. It seems sort of optimistic to have separate width and height. I think we should either have it take a face id, or instead just return a scalar. However, that’s not what other APIs do.
*   JG: Why vec2 is a little bit nice. It’s forward compatible a little bit, at least more than a scalar.  If you swizzle out of vec2, the swizzle works.  But scaling up out of a scalar is not so compatible. Changing size of the vector would be breaking change (necessitating something different)
*   BC: Would be nice to take a face ID param to get select which face you’re getting the dimensions for.
*   BC: .. more ideas …  in the realm of very hypothetical here.
*   KN: If we had weirdo squashed cubes it would be a new type, not “cube”.
*   JG: I expected it to return width x width x 6.  But it’s actually width x width x width. Which is not obvious.
*   KN: Native apis return vec2 width x width. Should just follow.  … Hm. It’s not obvious. Cube_arrays
*   BC: we have separate call to return the array size. 
*   _Resolved: vec2, width x width_


### [wgsl: Fix typo in vertex_index #1687](https://github.com/gpuweb/gpuweb/pull/1687)



*   _Merged_


### [var: infer type from initializer #1550](https://github.com/gpuweb/gpuweb/issues/1550)



*   DN: Can live with everywhere but module scope (as DM wants)
*   RM: Can live with that.  Don’t know why module scope is special.
*   DM: Rust has inference only for local scope.
*   JB: Languages that do inference at module scope:  users suffer
*   _Consensus to go with DM’s proposal._


### [Limit pipeline constant IDs to between 0 and 65535 #1720](https://github.com/gpuweb/gpuweb/pull/1720)



*   RM: Can agree since it’s a limitation of an implementation.
*   JG: Can always do remapping, then IDs don’t matter.  But not enough functionality to be important enough to add that completely_._
*   _Consensus to merge._


### [Clarify out of bounds behavior #1722](https://github.com/gpuweb/gpuweb/pull/1722)



*   (out of bounds for non-memory values)
*   AB: If you have plain value for a vector, and extract OOB element, what is the result?  SPIR-V says “undef value”. But we can do better by throwing in a clamp on the index (for example). E.g. “get one of the in-range values”.
*   DM: Robust buffer access behaviour is very close to undefined value anyway.  If we have this for other pieces for memory access, why introduce a stronger claim here only.
    *   Can return anything in the bound memory range, or 0-value
*   JG: Don’t know it’s exactly the bound memory range. Not super confident that RBA will cover case of loading vec3 but reading the 4th element. Get garbage from the register.
*   AB: To me it’s consistent to say it’s a limited set of values from portability perspective.  Saying “undef value” can work.
*   DM: Saying it’s limited set of result values is misleading.  “Anywhere in this bound memory range” is just as bad as “undef value” in practice.
*   AB: An “undef value” may not be something in memory.  It’s worse.
*   DM: If we do handle that, it’s extra overhead. And in-memory has different results than on-stack (in-register).  How do you justify this overhead?
*   AB: Don’t think you dynamic accesses to values. Think it’s one max.
*   AB: Not concerned about performance if the shader is doing a bad thing.
*   DN: Could promise less now, and promise more later.
*   DM: But you always pay for this.
*   **_Consensus seems to be: when getting data out, it’s an “undef value”._**
*   **Challenge is to spec that well.**
*   JB: Undef value, undefined behaviour, and unspecified value.
*   RM: For int and float it’s clear (and include NaN).  For bool, result is either true or false.
*   AB: We don’t specify a bit pattern for bool.  Can always
*   RC: Want to clarify “safe” value.  Content from all browser tabs are in the same render process. One tab should not be able to access state outside the security domain of the given.
*   AB: For memory values, want to bound it to bound memory. Discussion is for non-memory composite values (e.g. let v : vec3&lt;i32> = ..)
*   RC: WebGL allows you to return 0 or some other value in the buffer. Can’t return value from a different tab’s storage.
*   AB: Implementation has to avoid abutting storage from different domains too closely (e.g. sharing pages or cache lines). But that’s an implementation detail.
*   RC: For D3D, arrays on stack, there are no guarantees, and would have to clamp those.
*   TR: The guarantee is they won’t see memory from another process.That’s not good enough for WebGPU.
*   AB: So please review the memory part of the PR in question. There’s a bunch of cases you’ve raised that I think are covered in this PR by the pointer and reference parts. Please double check that.
*   TR: Seems some bounds checking code is needed no matter what.  Especially for arrays in local memory.
*   AB: Yes.  And current debate is dynamic index on a let-declared vec3 (for example).  For literal index we can do compile error.
*   TR: Why not define result as 0.
*   AB: Need to be the union across APIs.  “Somewhere in the buffer” is good for Vulkan. 
*   JG: Metal doesn’t give guarantees at all.
*   TR: Wonder what the difference in performance is between predicating the store vs. allowing the store to go ahead with an out-of-bounds access being wrapped.
*   JG: Don’t care about perf of a bad program.
*   TR: I’ve seen code patterns that purposely do OOB to get the 0.
*   _Action on Microsoft to review the memory parts._
*   _Action on AB to update the “undef value” part_


### [wgsl: describe derivatives, related uniformity #1728](https://github.com/gpuweb/gpuweb/pull/1728)



*   Non controversial
*   Consensus; let’s merge it.


## 

---



## Discuss


### [Buffer indices should be unsigned · Issue #1135 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1135) 



*   JG: Think we’re headed: unsigned
*   DM: Think we’re headed: signed
*   DN: Want array elem count bounded above by signed max
*   RM: Thought we were going to allow both
*   AB: … provided array elem count is limited to signed max.
*   … discussion about how to do the signed max check:   u32(i) &lt;= u32(INT_MAX).  Is one operation because the bitcast is free.
*   AB: Taking unsigned only: usability with integer literal.
*   JG: Larger question about what is the type of the literal.
*   AB: Those other languages have more implicit conversions than WGSL does.
*   JG: The main usability concern is do-what-i-mean with literals. It’s stronger than the guardrails provided by lack of automatic-convertibility.
*   TR: HLSL has a special literal type for float or int literals. Nice, but carry their own problems; have corner cases that don’t have perfect solutions. E.g. ternary operator.
*   JG: Folks like unsigned for indices because it’s an implied immediate constraint. If we accept only signed for the array indices, then we force people to cast. We should not do that.  I’m inclined to accepting signed or unsigned here. Slight preference to accept unsigned only if we pick only one.
*   DN: Let’s work out a written proposal for “what if you allow signed and unsigned, limit array size to INT_MAX, and plan what happens down the road when very large arrays are allowed. What do you do with u32(3billion).”


### [Add the uniformity analysis to the WGSL spec by RobinMorisset · Pull Request #1571 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/pull/1571) 



*   


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-05-25 (like normal)
*   [https://github.com/gpuweb/gpuweb/pull/1735](https://github.com/gpuweb/gpuweb/pull/1735) wgsl: access mode is no longer an attribute
    *   Had one round of review, and addressed.
    *   Can we land this?
*   
*   [https://github.com/gpuweb/gpuweb/pull/1727](https://github.com/gpuweb/gpuweb/pull/1727) Editorial: “mipmap level” -> “mip level”
*   [https://github.com/gpuweb/gpuweb/pull/1742](https://github.com/gpuweb/gpuweb/pull/1742) Allow non-overridable constants in workgroup_size #1742
*   
*   