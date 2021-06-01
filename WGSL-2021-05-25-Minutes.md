# **WGSL 2021-05-25 Minutes**

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
    *   **Myles C. Maxfield**
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
    *   **Rahul Garg**
    *   Ryan Harrison
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
*   Dominic Cerisano
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


### VF2F



*   Survey soon!
*   


## 

---



## Timebox


### [wgsl: access mode is no longer an attribute #1735](https://github.com/gpuweb/gpuweb/pull/1735)



*   (wgsl: access mode is no longer an attribute)
*   (Had one round of review, and addressed.)
*   (Can we land this?)
*   DM: When do we specify access quals for pointers. Spec has it for function. I will follow up offline.
*   


### [wgsl: mipmap level -> mip level #1727](https://github.com/gpuweb/gpuweb/pull/1727)



*   (Editorial: “mipmap level” -> “mip level”)
*   JG: Seems fine.


### [Allow non-overridable constants in workgroup_size #1742](https://github.com/gpuweb/gpuweb/pull/1742)



*   AB: the hard case was overridable.  This is *non*-overridable. Simple case. Since we don’t have constexpr, this is at most substitution.
*   DM:When we do get constexpr there is more work.
*   _Discussion about relation to spir-v spec constant expressions_
*   RC: In compile HLSL you can’t override workgroup size.
*   TR: HLSL can do some limited amount of const expr by “static const int”.  Believe you can use that for num_groups.
*   DM: Sounds like that fits well.
*   JG: So seems implementable.
*   AB: We already adopted the harder version. (overridable).
*   JG: But overridable can’t be const-expressions.  They’re just identifiers.
*   DN: module-scope constants can only be literals right now.
*   _Agree to land_


### [Remove % for signed integral #1763](https://github.com/gpuweb/gpuweb/pull/1763)



*   Undefined in spir-v and in glsl for negatives
*   DN: Please mak the polyfill without control flow. (use select)
*   DN:DM: What about overflows generally? INT_MIN / -1? (overflows signed integer)
*   JG to file an issue for that
*   DM to try making a spir-v polyfill


### [Add floating point execution rules #1743](https://github.com/gpuweb/gpuweb/pull/1743) 



*   (probably fine. One day for people to double-check)


## 

---



## Discuss


### [Buffer indices should be unsigned · Issue #1135 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1135) 



*   MM to talk with Metal team about proposed solution (i64 all the time)
*   


### [Add the uniformity analysis to the WGSL spec by RobinMorisset · Pull Request #1571 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/pull/1571) 



*   (nothing new)


### [Clarify out of bounds behavior #1722](https://github.com/gpuweb/gpuweb/pull/1722) 



*   DM: Uses semantics of “orginating variable”. Which is WSGL-centric. Have to consider the host-side. Consider the resource which is the source for the binding.
*   MM: But the point is you won’t see data from another resource.  Use case is you might multiplex multiple user contexts into a single graphics context?  Is that the point.
*   DM: Think they want to limit it as much as possible.
*   RC: For each access you want to know which buffer is being accessed.
*   MM: What happens on Vulkan with bindless.
*   Metal guarantees no access reaches outside the process.
*   Same for Windows
*   _Discussion about separating WebGPU context per process_
    *   Has high cost.  E.g. sharing with image sources, compositing.
    *   Firefox shared compositing resources across processes
*   RC: Reaching within process but outside resources can land on a page you own (safe), or not own (which leads to crash). That’s “safe” but non-portable and painful to deal with.


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-06-01 (like normal)
*   [https://github.com/gpuweb/gpuweb/issues/1777](https://github.com/gpuweb/gpuweb/issues/1777) wgsl: compute shaders should have a workgroup_size attribute
*   [https://github.com/gpuweb/gpuweb/issues/1431](https://github.com/gpuweb/gpuweb/issues/1431)  allow array size that is a pipeline-overrideable constant 
    *   Use case: sizing workgroup storage .Tied to workgroup size, which is overridable
*   
*   