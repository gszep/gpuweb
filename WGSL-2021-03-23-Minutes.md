# **WGSL 2021-03-23 Minutes**

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
    *   **Rahul Garg**
    *   Ekaterina Ignasheva
*   Intel
    *   Yunchao He
    *   Narifumi Iwamoto
*   Microsoft
    *   Damyan Pepper
    *   **Greg Roth**
    *   Michael Dougherty
    *   **Rafael Cintron**
    *   Tex Riddell
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
    *   Jim Blandy
*   Kings Distributed Systems
    *   Daniel Desjardins
    *   **Hamada Gasmallah**
    *   Wes Garland
*   **Dominic Cerisano**
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
*   **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy) **
*   Mass calendar invite will have been sent out
    *   Add yourself to the list if your email isn’t on here:
        *   [https://docs.google.com/document/d/1eTmJF2yVjn8sxJ_J5htu2nYpdxS7j2Zm6BRvAn--ufk/edit](https://docs.google.com/document/d/1eTmJF2yVjn8sxJ_J5htu2nYpdxS7j2Zm6BRvAn--ufk/edit) 


### WGSL takeover of next week’s API meeting?



*   1. Do we have enough to talk about?
    *   Yes
*   2. Do we have enough people who can make that timeslot, who aren’t usually there?
    *   Yes


## 

---



## Timebox


### [Restrict where entry point IO types can be used #1526](https://github.com/gpuweb/gpuweb/issues/1526)



*   Different rules per attribute?
*   RM: What’s the problem with allowing this and ignoring it when they aren’t being used as IO
*   AB: That’s the meta question about attributes: when should they matter
*   MM: They matter when it’s observable.
*   AB: It’s not obvious where it’s observable.  E.g. calling a function.  Do we need a bunch of individual rules? Or do we say ignore in all contexts except in certain situations
*   GR: Bad to have things being ignored silently.  You get superstitious folks adding things and then having no effect.
*   MM: example of offsets in struct returned from a helper, then stored into a buffer. That should work.
*   AB: Agree for certain cases, e.g. sharing structures. Not sure it applies everywhere.
*   MM: “When a location is specified in a particular place, then it has _this _ effect.” So that entails that if it’s used in a struct but is used in a different context, then it has no effect.  This is a convenience-for-authors argument.
*   GR: How does it help an author to be able to put this everywhere.
*   MM: Code reuse.  E.g. the first example in the issue seems perfectly fine. Not objectionable.
*   AB: If we say they’re ignored in certain places, then we may limit the future path of development because all of a sudden something that used to be ignored becomes meaningful, then it can break an already-shipped shader.
*   AB: Example adding auto-layout of locations, could lead to conflicts.
*   MM: If we change the language to auto-assign, where the author can currently assign value manually, then that change to the language must take into account the possibility that the author has assigned those fields.
*   AB: E.g. authors themselves could put conflicting locations that they expect to be ignored, and then later it becomes significant.
*   JG: Think that’s possible but low risk.
*   AB: Also ties to issue David raised about whether adding an attribute to the type if it forms a new type. 
*   MM: offsets internal to a struct, perforce is a different type.
*   DN: Agree.  The interesting case is when the attribute is on the outside of the type.
*   DM: Concern about confusion if allow things in too many places.  E.g structures with locations in them, but then used in a compute shader.  Think the test case in the issue should not be supported at launch.
*   MM:_  describes use-case of a thing (having sample mask) used both in fragment shader and compute shader._
    *   Using such a struct in a helper function doesn’t care about whether or not a particular sample mask was populated in an entry point (some other function)
*   _Timebox closed._
*   _Encourage: add comments to the bug_


### [Make read_write access the default for storage buffers #1487](https://github.com/gpuweb/gpuweb/pull/1487) 



*   Default access for storage buffer
*   Where do we generally draw the line for user convenience?
*   Seems similar to [#1493](https://github.com/gpuweb/gpuweb/pull/1493)?
*   AB: Meta question. We’ve often done things for user convenience, but not been consistent.  This can get out of hand, but need guidance as a whole on where to come down.
*   GR: Agree. Don’t have clear rules on user convenience tradeoffs.
*   GR: 1529 (infer types) also falls into the convenience category.
*   MM: Trying to standardize grand philosophy often doesn’t work. Can lead to endless arguing.
*   DN: read-write is overwhelmingly the use case. Compiler needs to validate the uses anyway.
*   MM: Natural design to allow default to allow the user to write what they want in the shader, and have opt-in for the fast path.
*   MM: For compute shaders, read-write is a good default.
*   JG: (some consideration of offloading work to users).
*   JG: Interesting to add the obligation on the implementation to validate the usage. Concern about mis-analyzing or not analyzing.
*   DN: analysis must always be static, to be consistent.
*   MM: My arguments were that if there’s a default, it’s better to be the read-write rather than the read-only.
*   JG: we can get to MVP without a default.  Then add it later. Can’t go it the other way.
*   AB: Also need to update the spec examples.
*   _Timebox ended._
*   AB: I’ll file an issue ….


### [Support basic inter-scope shadowing. #1472](https://github.com/gpuweb/gpuweb/pull/1472) 



*   Consensus to do this.  Need spec text.


### [Support for vector splats (scalar operations on vectors) #1520](https://github.com/gpuweb/gpuweb/issues/1520)



*   E.g. `vec(x,y) * 2.0`
*   DN wrote PR [1527](https://github.com/gpuweb/gpuweb/pull/1527) for vector-constructor-from-scalar;  DM landed it.
*   Deliberately does not address the naked scalar case as originally requested
*   MM: Why is multiply privileged
*   DN: linear algebra building block.
*   BC: As a graphics programmer, I want to divide a vector by a scalar.
*   JG: `gl_Position = vec4(pos01 * 2.0 - 1.0, 0.0, 1.0);`
*   MM: Thought we had a rule about no implicit cast.
    *   Here’s a counter proposal: Don’t allow “vector op scalar” and “matrix op scalar” for any operation. Instead, expose constructors / standard library functions to turn those into “vector op vector” and “matrix op matrix”
*   [...]
*   MM: If someone made the popularity argument, I would be convinced by that. The argument being “if every popular shading language supports ‘vector * scalar’ but not ‘vector / scalar’ then that’s a pretty strong argument that we should follow”
*   DM: Maybe it’s better to force people to be really explicit?
*   DN: Every new feature costs test time.
*   JG: I’ll survey other shader languages to see what they have.
*   GR: I volunteer to get the answer for HLSL.
*   GR (later): HLSL accepts every arithmetic operation between vectors and scalars and matrices and vectors for both DXC and FXC



---



## Discuss


### Type Inference



*   [https://github.com/gpuweb/gpuweb/pull/1529](https://github.com/gpuweb/gpuweb/pull/1529) 
*   _Lively discussion_
*   _Consensus on shortening both const and var by dropping the type.  For var, must have an initializer or a type (or both)_
*   _Support for super-short case as well, but concern about spelling (:=).  Will discuss spelling offline._


### Pointers



*   links:
    *   [https://github.com/gpuweb/gpuweb/issues/1456](https://github.com/gpuweb/gpuweb/issues/1456) var as syntactic sugar for explicit pointer ref/deref;  alternate:  ref&lt;SC,T> as dual type to ptr&lt;SC,t>
    *   [https://github.com/gpuweb/gpuweb/pull/1368](https://github.com/gpuweb/gpuweb/pull/1368) Describe pointers (like C++ references, with a disambiguation rule about subaccess taking priority over dereference)
    *   [https://github.com/gpuweb/gpuweb/issues/1334](https://github.com/gpuweb/gpuweb/issues/1334) replace ‘ptr’ with ‘ref’
    *   DN: Options side by side: [https://github.com/gpuweb/gpuweb/issues/1456#issuecomment-794186692](https://github.com/gpuweb/gpuweb/issues/1456#issuecomment-794186692) 
*   _Consensus on how pointers work for getting sub-field access directly on a pointer, and [i] sub-element directly on a pointer._
*   MM: which operators work on which types is not that material/important/objectionable. The nuance is that it’s a convenience, nice to have, strictly additive.  Think the same argument of convenience is the same argument for the arrow operator.  *a.b.c and a->b.c will generate exactly the same machine code. If we allow the convenience of dots on pointers, we might as well give authors the convenience of an arrow operator, for the same reason.
*   DN: For me, the  *a.b.c is privileged because that vastly simplifies translations. Am happy to support -> at some point as pure sugar.  But from a workload perspective will land *a.b.c first and delay support for ->.  Am happy to support -> eventually.
*   MM: Think we have a way forward.
*   DM: What about dot doing auto-deref?
*   JG This becomes slower/ambiguous: `const sub2 = ptr.sub;`
    *   Is sub2 of pointer type or not?


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-03-30 (like normal)
*   I/O locations: [https://github.com/gpuweb/gpuweb/pull/1519](https://github.com/gpuweb/gpuweb/pull/1519) 
    *   Limit structures to 1-level deep only.