# **WGSL 2021-04-13 Minutes**

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
    *   Myles C. Maxfield
    *   Robin Morisset
*   Google
    *   Corentin Wallez
    *   David Neto
    *   Kai Ninomiya
    *   James Darpinian
    *   Brandon Jones
    *   Ryan Harrison
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
*   Joshua Groves
*   Kris & Paul Leathers
*   Lukasz Pasek
*   Matijs Toonen
*   Mehmet Oguz Derin
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
        *   


---



## Timebox


### [Vector-matrix arithmetic extended with scalar "splats" #1611](https://github.com/gpuweb/gpuweb/pull/1611) 



*   Merged


### [consider adding matrix determinant #1623](https://github.com/gpuweb/gpuweb/issues/1623) 



*   (yes, add it)


### [consider adding: acosh, asinh, atanh #1622](https://github.com/gpuweb/gpuweb/issues/1622) 



*   AB: Polyfill would need care about precision
*   TR: +1 for builtin, might be opportunities in the future
*   JG: Let’s speculatively spec this


### [Add pointer-chaining expressions: ptr.field and ptr[i] #1580](https://github.com/gpuweb/gpuweb/pull/1580)



*    JB: Some reservations. Can say, “which usage is more common: Value of field, or address-of?”. 
*   AB: This facilitates easier constant declarations, can chain into pointers into buffers. I’m for it.
*   JG: I like it, but it spends our “innovation points” on something entirely optional to the language.


### [[wgsl] Proposal: Remove pointer out parameters from modf, frexp · Issue #1480 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1480)



*   DM: Do we want to spend innovation budget on anonymous structs here?
*   BC: Previously there were concerns about having to have a named struct to return here, but now with type inference, we could return anonymous structs here. Not necessarily proposing userland anonymous structs, and if we decided to name them in the future, we could name them.
*   DM: Some complexity, new types for these builtins.
*   AB: They need special types named or not, if we’re returning structs.
*   BC: Motivation: In the future, we will probably want this with generics.
*   DM: It might be nice to do this with tuples instead, and we can’t switch to tuples later here.
*   BC: I think named fields are nicer than indexing into tuples
*   KN: +1
*   JG: We can still switch to tuples if we spec them before we commit to backwards compat.
*   DM: I feel like named fields is notably different than what we have for normal call-sites, which don’t have named argos
*   AB: Why tuples?
*   DM: Nicer for wrapping? Vs nested structures
*   KN: Sort of only difference between anon structs and tuples is that anon structs are more strictly typed, and cannot be assigned to a different anon struct, unless you have “structural typing”, where what matters is the shape of the struct and not its name.
*   DM: Did we consider implementation cost?
*   BC: Probably less work than having to declare many structures here
*   DM: I see tuples are being first-class constructs
*   BC: I think anon structs can be pretty similar to what you see for tuples.
*   JG: Can we use this to first solve the multiple-return-value problem? Vs what we have now with part-return-part-output-pointer. 
*   BC: Yeah, that’s why I proposed it
*   AB: That’s why I’m bearish on this: We have a solution that works, and this would require something new.
*   BC: I don’t care as much as I used to, but I feel like if we don’t make it good now, it will always suck.
*   AB: We could add overloads later, but I don’t want to get too far off in additions to the language today, just for this.
*   KN: I don’t think named structs are a dealbreaker
*   DM: Yeah, maybe there’s no real implementation issue here, but I wondered if you’d tried it yet. We’ll need to prototype.
*   JG: Is named structs that bad here? No new feature needed, just boilerplate.
*   DM: Would we want to follow up with a general anon struct proposal anyway?
*   JG: Like tuples, proposals welcome
*   AB: We didn’t agree on names yet, which we need for speccing
*   JG: Probably just try to spec something, as long as we don’t think naming discussions are non-consensable.
*   BC: This proposal was because we would not need to agree on names! :) What we have now is workable, but no strong feelings.
*   DM: I like anon structs better than named structs.
*   BC: I was thinking anon structs would be easy to plug into the lang. KN said though that this adds a foible that structure types are based on names, which becomes maybe problematic for anon structs. 
*   JG
*   BC: What if you have two calls to `modf`, but do they have the same type?
*   JB: Probably want that to work, and there is prior art here.
*   BC: If we wanted anon structs, we need this struct duck-typing here
*   DM: If we wanted to name them later, we wouldn’t be able to s/anon/named/, because programs would break.
*   AB: Does structural equivalence consider name or just type?
*   JG: Can we just name them and talk about anon struct proposals right here right now?
*   AB: Some reservations allowing anon structs for users


### [Specify WGSL as UTF-16-encoded text. #1626](https://github.com/gpuweb/gpuweb/pull/1626) 



*   BC: I agree with KN: Do we need this specified here? (probably?)
*   KN: Ties into how we define how to talk about errors/warnings for locations. Code-unit vs code-point. It seems like a use case is for easily converting error/warning to offsets into JS strings
*   DM: Why do we need this specified at all? Does any part here care?
*   JG: difference in how we treat the input. If treated as ascii vs UTF-8 it has effects on validity of incoming code. Parsers need to handle that. Need to know end-of-line for example.
*   KN: no byte representation in the spec. It is a string that the implementation can choose to encode how they wish.
*   JG: Constrained by Web IDL
*   KN: native apis will care.
*   JG: this should be written down


## 

---



## Discuss


### [Add the uniformity analysis to the WGSL spec #1571](https://github.com/gpuweb/gpuweb/pull/1571)



*   Waiting on RM


### [consider pulling 'access' into the type of a pointer/reference #1604](https://github.com/gpuweb/gpuweb/issues/1604)



*   (tabled)


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-04-20 (like normal)
*   Out-vars
*   Uniformity (hopefully!)
*   Access as types
*   