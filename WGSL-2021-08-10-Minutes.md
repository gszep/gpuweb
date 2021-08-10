# WGSL 2021-08-10 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** Alan Baker

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
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
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
* **Dominic Cerisano**
* Eduardo H.P. Souza
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Pelle Johnsen
* Timo de Kort
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
        * 


## 

---



# ⏳ Timeboxes


### [wgsl: array size may be module-scope constant (possibly overridable) #1792](https://github.com/gpuweb/gpuweb/pull/1792)



* (Rebased, and updated to incorporate #1135 (allow array element count to be unsigned), and a bunch of cleanups)
* Further discussion: allow this only the case where:
    * The array type is the store type for a workgroup variable. (only the outer dimension. (Then can be implemented in MSL as a pointer-to-local entry point param.)
* [Deferred until this week. Myles didn’t do his homework]
* [https://github.com/gpuweb/gpuweb/issues/2024](https://github.com/gpuweb/gpuweb/issues/2024)
* MM: for 1792, allow non-overridable module scope constant for array sizes
* DN: well separate overridable part out
* _Resolved module scope constants (non-overridable) are useable for array sizes_
* MM: Don’t want arbitrary overridable arrays due to Metal philosophy (and C++). Want to know the static size whenever possible. Willing to limit to workgroup arrays. Size is known later than shader creation time. How do we expose workgroup arrays sized after shader creation. Option 1 use specialization constants and limit to just outermost workgroup array. Option 2 make a new feature, add attribute to the workgroup variable, specified via the API. Could get the length of an array the same as a runtime sized array.
* DN: control knob specified after shader creation that affects “stuff”. Seems like specialization constants to me. Hard to think past. Would prefer fewer new features.
* MM: new use of specialization constants feels distinct from existing use cases.
* DN: feels parallel to sizing the workgroup. Single knob to control multiple language constructs.
* MM: workgroup size is not specializable.
* DN: we changed the spec to allow overridable constants.
* JP: [https://github.com/gpuweb/gpuweb/issues/1442](https://github.com/gpuweb/gpuweb/issues/1442) made that change. 
* MM: seems compelling. One source of truth for what the workgroup variable is.
* JP: not always 1:1, but usually a function of it.
* MM: that seems more in line with option 2
* DN: worry about the coupling between api and shader
* MM: Metal exposes one variable
* JG: should we tackle constexpr sooner?
* JP: not sure there is a single source of truth with option 2. Still modify API and shader. Works even better with contexpr
* MM: seems too narrow a view to me. Regard it as part of the binding feature. 
* DN: think it is often solvable just in the shader.
* 


### [Issue 1534's array types differ on stride #1943](https://github.com/gpuweb/gpuweb/pull/1943)



* [Deferred until this week. Google needs more internal discussion]
* Tabled indefinitely, Google to revive discussion when ready


### [Clarify IO location overlap #1998](https://github.com/gpuweb/gpuweb/pull/1998)



* To be reviewed offline


### [Restrict out-of-bounds behaviour for RMW #2016](https://github.com/gpuweb/gpuweb/pull/2016) 



* RM: If I have this right, when we do read or write, we can choose what to do. This then says for RMW that it must make the same choices for the combined read and write.
* MM: E.g. if you do clamp, it must be the same index for both operations?
* JG: Yes
* DN: Atomics were clear for me, but less strong about e.g. +=, but I’m fine with this generally.
* MM: Can revisit this if we want to change our mint when we get to +=.
* AB: This is *statements*, which would mean e.g. `a[i] = a[i] + 1`, both `a[i]` must be the same address/place/choice-of-oob
* JG: What about function inlining?
* AB: We don’t talk about that elsewhere, so no new language needed there.
* MM: This is a bigger change than I thought it was.
* MM: Is this implementable for robust buffer access?
* JG: Not per se, unless we hoist the `a[i]`
* MM: I think we should restrict ourselves to atomics for now?
* DN: Happy to do that.
* DM: Do we even need language for this? We would naively expect it to use the same address.
* RM: We should have spec text for it
* DM: So this would be a PR for the OOB behaviour on the atomic operation alone.
* JG: Looking forward to += feels like that should behave the same way. But agree doing it for the whole statement is very broad, even if a[i]=a[i]+1 would naively expect to do the sensible thing.
* AB: Concern with a[i]=a[i] + f(&i);  you end up with 
* DN: If I have two expressions that evaluate to the same address, do they have the same result? Is it evaluated once? Statement scope?
* JG: Could save yourself if you computed a pointer, and put that in a let, then do two operations on that pointer, then because the pointer value doesn’t change, then that does what the programmer wants.   This PR proposes that the implementation do that let-saving for you.
* JB: The atomic takes one parameter, so it would be wrong for the operations on the pointer to be on different locations.
* AB: The problem is the pointer is bogus.  You don’t know what it “really” points to, so the spec has to say specific that both parts of the accesses.
* JB: Think that as far as the atomics are concerned, the spec already says this.
* JG: Let’s see what the “normative note” would say.
* RM: Are the atomics allowed to drop the access altogether if the pointer is out of bounds?
* JG: I think this might get at a core question: Do we have to commit to an OOB-handling approach/address when we create a pointer (or ref), or can it vary at the point where we *use* the pointer?
* AB: To be consistent with underlying applications, the decision occurs if it’s good/bad occurs at the time of access (not at the time of computing the pointer.)
* GR: Ultimately Vulkan and D3D run on mostly the same hardware. What does the hardware do in these cases.
* AB: For general OOB behaviour, because lots of hardware supports D3D, those desktop GPUs have HW support for masking writes, for efficiency. Mobile HW doesn’t necessarily support that masking-out of writes. 
* MM: (one past the end of a pointer use case)
* AB: C is clear that one-past-the-end is a valid pointer.  
* MM: &arr[elemn_count] should produce a pointer that is different from any interior pointer to the array.
* JB: atomic has to know whether to respect a pointer as valid for operation on vs. not.
* AB: That knowledge/implementation is smeared across compiler and hardware.
* JG: Suggest a new PR from folks who think we need more spec language.
* AB: Will do, on same branch in git.
* JG: Feels better for bookkeeping to keep this as a closed-off avenue of exploration.  Please make a new PR.


### [Buffer indices should be unsigned · Issue #1135](https://github.com/gpuweb/gpuweb/issues/1135) 



* (are we done here?)
* Yes


## 

---



# **⚖️ **Discussions


### [[WGSL] WGSL needs a plan for expansion · Issue #600](https://github.com/gpuweb/gpuweb/issues/600)


### [consider namespaces for WGSL · Issue #777](https://github.com/gpuweb/gpuweb/issues/777)



* MM: Wanted this on the agenda because of the frexp issue. I think we decided that the structs we returned there would be special and magical and unnamable. It would be cool if you could name them, so it would be nice if we could figure out a way to let this happen. 
* DN: I like namespaces. Would like to see a proposal that is conventional.
* JG: I think I’m the one who sorta doesn’t want them. I want them to solve an actual problem. I don’t want to work too hard to a feature if it’s not really going to help. In my experience namespaces haven’t been particularly useful for shader-level things. Am curious to see how they help with versioning and extension.
* MM: Two benefits. First is expansion. Second is shaders can get big, written in parts by different teams, and namespaces help there. Even JS shoehorns it in with anonymous functions. Evidence from modern languages is it helps.
* JG: Argument is that shaders tend to be smaller in scope.  Won’t put INSERT_LARGE_FRAMEWORK in a shader.
* MM: Certainly can agree shaders can get very big. Propose shaders can get quite big.
* JG: Matter of degree.  If it’s something that’s relatively essential to studios, then consider. Can see arguments for it. Want the directionality of the arguments weighed against the magnitude of the usefulness.
* MM: Bringing this up now not because of the studios asking. Instead, bringing it up now because of specific benefit to the standard library.
* JG: Would like to see a concrete spelling out of that solution.
* MM: Think we have some proposals to pull up for that and state them in terms of ns.
* MM: what’s the argument for the frexp/modf not being regular structs.
* AB: Ben’s argument; he’s not here.  Wanted a future-compatible way of implementing the feature as an anonymous struct, and without carrying baggage of a name that we declare in the spec that we have to live with forever.
* DM: There are no good names for these structures, and not vastly useful to users.
* MM: I think it’s useful for e.g. polyfilling frexp, and passing the result of frexp to a function


## 

---



# 📆 Next Meeting Agenda



* Next meeting: 2021-08-17 (like normal)
* 