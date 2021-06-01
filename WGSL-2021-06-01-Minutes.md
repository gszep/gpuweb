# **WGSL 2021-06-01 Minutes**

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
    *   **Kai Ninomiya**
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
    *   Hamada Gasmallah
    *   Wes Garland
*   **Dominic Cerisano**
*   Eduardo H.P. Souza
*   Joshua Groves
*   Kris & Paul Leathers
*   Lukasz Pasek
*   Matijs Toonen
*   **Mehmet Oguz Derin**
*   Pelle Johnsen
*   Timo de Kort
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


## 

---



## Timebox


### [signed integer division: unspecified rounding mode for the quotient; and overflow · Issue #1774](https://github.com/gpuweb/gpuweb/issues/1774)



*   DN: MSL inherits C++’s behavior for opposite-signed division: implementation defined. HLSL is undefined for opposite-signed modulus. 
*   AB: Spir-v says for div is undefined behavior
*   JG: Surely we don’t want to forbid opp-signed div, yeah?
*   AB: Yeah, but just call out what the behaviors might be. Could try to bound the result to one of two answers, but don’t know if it’s worth it.
*   DM: Do we have a proposal for this?
*   DN: 1) Ban int division; or 2) allow different results; or 3) Polyfill? Tempted by leaving it undefined like HLSL.
*   JG: I would like 2. Other prior art is doing fine.
*   DM: I think our portability reqs are higher.
*   RM: It would be nice to know if we can require strict behavior efficiently.
*   DM: David: Can you write the proposed polyfill?
*   DN: Offline, maybe.
*   JB:: What are the costs of the polyfills.
*   DN: Expect quotient and remainder are produced at the same time; tie the decision to include but polyfill both / and % together.
*   JG: Want portability, but concern about cost.


### [Remove % for signed integral #1763](https://github.com/gpuweb/gpuweb/pull/1763)



*   (skip. Tied to integer division)


### [wgsl: compute shaders should have a workgroup_size attribute · Issue #1777](https://github.com/gpuweb/gpuweb/issues/1777)



*   DN: Compute shaders default to workgroup_size=1, which is the worst. We want to force the programmer to think about *at least* the x-dimension, lest people think our perf is bad because they used the bad default. So let’s stop having a default for x. (at least)
*   DM: Ship it
*   KN: Would be ideal if we could have an ‘auto’ mode, where UA can pick for us.
*   AB: OpenCL will make up a default for you, guessing.  But you may pay for multiple compiles. Don’t think you want to do that.
*   RM: I generally like this proposal, and we can expand this later.
*   KN: +1
*   (consensus)
*   


### [allow array size that is a pipeline-overrideable constant · Issue #1431](https://github.com/gpuweb/gpuweb/issues/1431)



*   DN: Want to size arrays based of workgroup-size. Right now arrays can’t be spec-constants, though workgroup-size is. This would be what spir-v allows.\
*   JB: Can we allow for some static checks? E.g. if it can tell that the access will always be OOB?
*   DN: Usually we stay away from that here, so that we don’t snowball required complexity. Because you can’t do it in general, we don’t want to have divergent implementations.
*   (needs spec)


### [WGSL requires dynamic matrix indexing, which requires surprising SPIR-V #1782](https://github.com/gpuweb/gpuweb/issues/1782) 



*   RM: GLSL can be compiled to SPIR-V (and also from HLSL), so do these source langs allow this? What SPIR-V is generated?
*   DN: GLSL does allow this, and the style of code generate is very variable-heavy, and so it does store to a var and load from that. This is kind of gross, but I’m less concerned about the (small) matrices problem, but arrays are more problematic. We definitely need to fix the OpCompositeExtract bug. For philosophy, we sort of avoid hashing that out because we’ve found it generally unhelpful for forward progress.
*   … One option is to delete the references, but it’s useful for authors. Maybe we want an implementor’s guide, but we don’t want to specify that. Philosophically we’ve definitely strayed from our earlier stated goals, but here we are.
*   GR: Which goal specifically?
*   DN: Like, 4 or so of them
*   GR: Should we fix those then?
*   DN: Some of them are still good, but &lt;lists some>. We’re ~50% false now.
*   (we can touch on this more later)
*   RM: DN mentioned the nuclear option of removing the refs to spir-v, but I think it’s good to keep them, even if they are only approximate. For this issue specifically, I’m concerned that if we forbid it, it would be painful for our authors. Especially if we can optimize many cases.
*   DM: A middle ground: Can only index if in memory, e.g. if they are references.
*   JB: I think if I were an author and got this error message, I would just store it in a var, which is exactly our workaround.
*   DM: If they are doing it, they take responsibility for it, which is good for some of the worst cases, e.g. 1000-len arrays.
*   JG: Maybe it’s good to surface the underlying constraints, and have author choose their workaround.
*   DN: For the matrix case, I think DM proposed adding matrix.x/y/z/w, which would help here. 
*   AB: What about just requiring the subscript be a literal?
*   JB: Could even have two rules: Arbitrary subscripts for references, but be strict for non-references.
*   RM: What produces by value? Func argos and intermediates? (also let vars)
*   DN: Should make the same choice for matrices and arrays, yes? (yes)
*   JP: Do authors have an option for making a read-only that has dynamic indexing?
*   DN: Not right now, NonWritable variables in Function/Private (?) storage class was only added to spir-v in 1.4. Maybe later?
*   (consensus for the two rule solution)
*   (JB to draft)


## 

---



## Discuss


### [Buffer indices should be unsigned · Issue #1135 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1135) 



*   (Previously: MM to talk with Metal team about proposed solution (i64 all the time))
*   (Staying tuned for more from MM)
*   DN: Another comment here I wrote was on how pointer validity in C/C++ include that pointers one past the end of a buffer are valid too. This is true in SPIR-V with variable-pointers, which is used for opencl->vk. We want this Eventually, so it’s something to keep in mind.


### [Clarify out of bounds behavior #1722](https://github.com/gpuweb/gpuweb/pull/1722) 



*   DM: Problem was with concept of “originating variable”, but robustness only guarantees within-buffer, not within-binding.
*   AB: Can change that.
*   AB: Also need a review from Microsoft.
*   RC: I skimmed over it. Thought it was ok, but agreed with DM’s point about “originating variable”.
*   AB: Some of the answers will change, depending on where we end up with array and matrix accessing.
*   JG: Myles mentioned that Metal requires within-binding, but MSL doesn’t have robust-access behavior.
*   RC: Is there provision to allow returning zero if it’s OOB?
*   AB: Yes.  Within buffer or return zero.
*   RC: With that, seems ok with me. Greg?
*   GR: Need time to review.
*   TR: Need time to review.

DM:  FYI [https://github.com/googlefonts/compute-shader-101](https://github.com/googlefonts/compute-shader-101) ISV from Google.


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-06-08 (like normal)
*   [https://github.com/gpuweb/gpuweb/pull/1789](https://github.com/gpuweb/gpuweb/pull/1789) wgsl: fix description of correctly rounded 
*   
*   [signed integer division: unspecified rounding mode for the quotient; and overflow · Issue #1774](https://github.com/gpuweb/gpuweb/issues/1774)
*   [Clarify out of bounds behavior #1722](https://github.com/gpuweb/gpuweb/pull/1722) 
*   [Buffer indices should be unsigned · Issue #1135 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1135) 
*   