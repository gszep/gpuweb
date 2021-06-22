# **WGSL 2021-06-22 Minutes**

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
    *   **Antonio Maiorano**
    *   Ben Clayton
    *   Brandon Jones
    *   Corentin Wallez
    *   **David Neto**
    *   Ekaterina Ignasheva
    *   Kai Ninomiya
    *   James Darpinian
    *   **James Price**
    *   **Rahul Garg**
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
    *   Tex Riddell
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


## 

---



## Timebox


### [wgsl: Remove level from 1D texture load intrinsic #1814](https://github.com/gpuweb/gpuweb/pull/1814) 



*   (background: Not all targets support mipmaps for 1D textures)
*   JG: Think of it as 1D texture can’t have LOD, so effectively think of this level param as clamped to 0.  
*   DN: Happy with that, suggest adding a note to that effect.  The language is capable, but it’s limited by what the API allows.
*   JP: Make textureDimensions consistent with the choice here.
*   JG will make PR for “fyi no mids for 1d tex”


### [wgsl: array size may be module-scope constant (possibly overridable) #1792](https://github.com/gpuweb/gpuweb/pull/1792)



*   (Dzmitry pointed out interaction with layout, so that needs clarification)
*   DN: Leaning towards (1)
*   MM: Don’t we need these?
*   DN: Can use real dynamic-sized arrays instead
*   MM: It would be good to have a configurable size for workgroup memory
*   DN: I think this allows this
*   (consensus 1)
*   DM: So is there an equation for layout based on spec-consts?
*   DN: We might want to just attempt compilation in the driver, and fail if the driver fails, rather than trying to predict success.
*   DM: Well we do have a workgroup memory size limit, so we do need to validate against that at least.
*   MM: Letting the driver maybe-fail would be new to us. Our impl expects to always generate correct code, and if the driver rejects, we’ve done something wrong. Tentative about merging this but it’s probably fine.
*   DM: Can DN follow up with an issue about the driver rejecting things? I wouldn’t expect it to fail if it passes our validation. (yes)


### [wgsl: can't type-construct or zero-value-construct aggregates containing atomics #1860](https://github.com/gpuweb/gpuweb/pull/1860)



*   DN: DM had great feedback that it would be good to make a new category for these types, so I’m going to rewrite the PR with that in mind.


### [wgsl: allow unsigned workgroup_size components #1858](https://github.com/gpuweb/gpuweb/pull/1858) 



*   (but require them to be all the same type)
*   (consensus: merge)


### [wgsl: refine pointer function call restrictions #1855](https://github.com/gpuweb/gpuweb/pull/1855) 



*   (built-in functions can have pointer-to-storage parameters)
*   (only user-defined functions constrain where the pointer came from)
*    (DN will massage the PR based on feedback)


### [wgsl: explicitly restrict storage classes for frexp, modf #1851](https://github.com/gpuweb/gpuweb/pull/1851) 



*   (Excludes storage)
*   (punt in favor of structs instead of outvars)
*   MM: I think there are args in favor of outvars (vs structs) is that (1) an output can be put into a register, which may be faster; and (2) you can pass null to ignore an unused result.
*   AB: can register both.  And nullptr isn’t possible some places.
*   DM: A lot of our APIs have ptrs, so we’d be breaking new ground here
*   MM: Probably breaking ground in an improved way
*   MM: I like structs aesthetically, but we need to be aware of the trade-offs.
*   DN: +1, and spir-v has both.
*   (MM will propose structs, probably)


### [Should the API enforce that writable storage buffer bindings don't alias each other? #1842](https://github.com/gpuweb/gpuweb/issues/1842) 



*   DN: I thought there was no aliasing, so I’m surprised it’s back! One Q is “how much work is the API willing to do to forbid/validate this”
*   GR
*   DM: I wanted to talk about this here instead of in API group, because it’s a more important question for us to answer, and we are the ones who know what our constraints are.
*   AB: Anything is possible, but we would have to mark things as [aliased], which we would only want to avoid/do conservatively. (otherwise it’s UB)
*   MM: Code-gen can definitely be better if we avoid aliasing. Scope is not the whole renderpass though, it’s just the invocation.
*   DM: Even without aliasing, if you read and then write to a UAV, what even are our guarantees?
*   MM: If you’re not using atomics, and reading from UAV, that’s a race. The compiler doesn’t have to consider what other threads to, if not using atomics.
*   DM: So the only thing we care about aliasing is within a single shader, like does it see the same resource in two different places.
*   AB: 
*   MM: If you bind the same resource to different types of bindings, they couldn’t be coherent, and you’ll get very different results on different machines.
*   AB: In a shader, different views of the same binding are ok if marked as [aliased]
*   DM: Since we don’t synchronize in this usage scope, can’t we just keep everything non-aliased?
*   (much discussion of UB)

[https://github.com/gpuweb/gpuweb/issues/1844](https://github.com/gpuweb/gpuweb/issues/1844) is the issue for bounding UB / ub.


## 

---



## Discuss


### [Buffer indices should be unsigned · Issue #1135 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1135) 



*   MM: Two directions, I will do homework. (1) performance, I think we just need to test/evaluate perf.
*   DM: I think it’s hard to test this
*   MM: I will test many devices. (2) I want to see what other shading langs do for e.g. negative results
*   


### [2nd, 3rd, and 4th components of sampled depth/stencil textures are undefined in Metal · Issue #1198](https://github.com/gpuweb/gpuweb/issues/1198) 



*    


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-06-29 (like normal)
*   