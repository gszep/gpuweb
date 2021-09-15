# WGSL 2021-09-14 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** David Neto, Jeff Gilbert

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
    * Myles C. Maxfield
    * **Robin Morisset**
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **David Neto**
    * Ekaterina Ignasheva
    * **Kai Ninomiya**
    * James Darpinian
    * **James Price**
    * Rahul Garg
    * Ryan Harrison
    * Sarah Mashayekhi
* Intel
    * Narifumi Iwamoto
    * Yunchao He
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * Rafael Cintron
    * Tex Riddell
* Mozilla
    * **Dzmitry Malyshau**
    * **Jeff Gilbert**
    * Jim Blandy
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Dominic Cerisano
* Eduardo H.P. Souza
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* **Michael Shannon**
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


### TPAC meetings?



* JG: hear that meetings don’t have to run any differently.  TPAC is similar to face-to-face in other contexts.  Likely will mark some future meetings as TPAC meetings.


### On Todos



* (JG: I will make a spreadsheet for thumbs-up/down/maybe for each todo)
* 


---


# ⏳ Timeboxes


### [precise_math attribute on functions #2080](https://github.com/gpuweb/gpuweb/pull/2080)



* DN: Discussing earlier and with MS. Our concern is two things
    * Making sure when targeting FXC that the math survives as well as we hope. We’re concerned about stability/reliability here
    * The name `precise` may end up promising more than all underlying platforms can do, so we may want to revisit the name if we can’t guarantee its behavior everywhere.
    * Thanks for the feedback about infinities and NaNs not being too important
* MS: Requires operations to not be reordered
* DN: Ops that are correctly rounded are +/-/*, but division is a harder request. Do you need division? (maybe?)
* DM: MM worried about spooky action at a distance. (SAAAD)
* KN: If this is defined as best-effort, at least for Metal, I think would prefer to use clang pragmas rather than SAAAD. I’m worried that we wouldn’t want to use *all* the flags, for perf reasons. Just reordering is less bad.
* DN: Rough consensus that we want this to be testable, rather than a pure hint.
* DN: **I want an investigation to show that assured non-reordering and non-reassociation are both implementable**. On DX11 (FXC), DX12, and Vulkan (desktop and mobile).
* DM: Sounds like something we need to figure out before v1.
* AB: Why?
* DM: We see real issues, MS and users of MoltenVk exist and they need this.
* JG: Is this a need that e.g. webgl already had.
* MS: We’re going from desktop directly to WebGPU. There’s extant code for this. Without this, we would need to do vert shading on CPU. We do that today, but we’re expecting to want to change this. We’d be pretty sad to not have this.
* 


### [Error with literal indexing in a shader #1803 ](https://github.com/gpuweb/gpuweb/issues/1803)  



* AB: The rules are consistent now.  There is tension if you want to change them: if you want constexpr to come in now, and if you want to implement using SPIR-V spec constants, then you can’t evaluate them in the browser's code. It’s unclear what the compilation path is on the different platforms. You get inconsistent constraints on when things can be evaluated. There are consistency issues when you get beyond literals.
* RM: We already have different rules about where constexpr/literals can be used (e.g. offsets).
* AB: It’s been raised that some people would want to use SPIR-V spec constants. Why double-up to implement the expressions in two places.  If you want to allow spec constants as the implementation then you don’t want this as a programming error. Let the check be deferred.
* DM: SPIR-V spec constants can’t implement all of desirable constexpr, e.g. cosine. Then why have two paths. Are you sure you want to use spec constants in SPIR-V for anything?
* AB: Depends on how far you want to go with constexpr. Not clear that I want to evaluate floating point math in the browser. That’s not the real value. Far more value in just integer expressions, which maps nicely to SPIR-V spec constants.
* DM: So you say it depends on scope.
* AB: And also requirements on the implementation about when they are evaluated.
* DM: Can you use OpConstantExtract with a spec id as ? Thought you can’t.
* AB: It’s one of the opcodes allowed for OpSpecConstantOp. 
* DM: I’m talking about the indices into a value-array.  You can’t use access chain. Have to use OpCompositeExtract. You have to evaluate that index on the spot as a literal.
* AB: That may be desirable, but is not the predefined only way. Gets tied up with the rules for indexing into value-array
* AB: If we get more clear about where the evaluation *must* occur, then it would be more palatable. (.... missed stuff….)
* JG: Would be happy to make it a warning; it’s a pretty good signal it’s a bug.
* DM: We decide based on how things are implemented. But we don’t have the whole picture there.
* JG: Difference here is constexpr is not mandated that it must be implemented a certain way. Usually you want it all done at compile time. Even in the C++ standard it’s as-if behaviour. The actual thing the machine does isn’t what you expect it to.  But if we go too far here, we might back ourselves into a corner into how it’s implemented.  
* JG: Premise: because we’re not 100% sure we want to constrain ourselves to always evaluate these early, then we should leave the door open to evaluating them later. Onus is to provide argument/evidence that it’s ok to force the earlier.
* AB: We have difference of opinion on what’s efficient in SPIR-V w.r.t. CompositeExtract vs. AccessChain on a local variable.  That’s a different issue.  Let’s not rat-hole on that here.
* DM: One issue depends on the other. The let-arrays, constexpr, and this.  One of them has to be a leaf to help resolve the other ones.
* AB: This is the smallest of them. The other issue should be decided first. For Const-expr we should decide on scope.  This decision falls out of those.
* DM: constexpr scope is slippery. How big do you go?  Goes circular.
* JG: Guess and iterate.


### [texture_2d_depth for loading depth values might be unnecessary? #2094](https://github.com/gpuweb/gpuweb/issues/2094)



* DM: Previously we decided all depth textures had to be bound as depth bindings. This is inconvenient. Not how it’s done anywhere except Metal. Metal doesn’t have the binding types that we have.  Observed by @magcius that the porting paths on Metal, in the cases when comparison is not use, will map to regular texture, and not using depth-texture. Seems to work for everyone for now.
* AB: But that’s not documented as the valid behaviour.
* DM: Want to layout the options
    * 1. Ask clarification from Metal: is the desired pattern supportable?
    * 2. Allow the shader to see regular texture 2D, provided the binding is the regular depth binding.  (Does not require input from Metal.  Solution is in the shader compiler only.)
* JG: On Metal the compilation needs the pipeline layout, to say which samplers are dept samplers. **We probably want to wait for MM for both**.
* RM: **I’ll ask the Metal team**, try to get feedback.


### (small) [PR#2095](https://github.com/gpuweb/gpuweb/pull/2095) Editorial fixups for describing statements. 



* ~~(Hopefully this can land before meeting~~
* LANDED before meeting


### (medium?) [PR#2096](https://github.com/gpuweb/gpuweb/pull/2096): more restrictions on control flow interruption (builds on #2095)



* Fixes: [#2093](https://github.com/gpuweb/gpuweb/issues/2093)
* If (cond) {
*      {return;}
*     A = 1; // surely not executed
* }
* RM: Agree this should be in the spec. Similar to a section in the uniformity rules. 
    * [https://github.com/gpuweb/gpuweb/pull/1571/files](https://github.com/gpuweb/gpuweb/pull/1571/files) 
    * Already a need in the uniformity rules to track that.  “Behaviour analysis”
    * (Discuss editorial merge conflict.)
* **Approved the concept**.  Find a way to reconcile the two statements of them.


### (small) [PR#2097](https://github.com/gpuweb/gpuweb/pull/2097): Editorial: Define “control flow”, and sequenced execution (within a compound statement).  



* Fixes some TODOs.  Builds on #2096
* Sounds good.  Merge after editorial discussion converges.


---


# ⚖️ Discussions


### [[wgsl] Support compile-time-constants (constexpr) · Issue #1272](https://github.com/gpuweb/gpuweb/issues/1272) 



* JG: Sounds like we roughly agree we should have it, but not decided what’s implementable well on various backends, and what those mappings look like. Would be great to surface the “cool” parts of underlying platforms.
* JG: Should we take a stab at trying it, then iterate? 
* DN: Suppose this is the same as what happened at C++ constexpr.  Don’t paint yourself into a corner.
* JG: Might be reasonable to start with just integers, then expand later.  Would be cool to do floats too.  Try any place where you currently need an integer literal, e.g. offset.  Then we would have to decide what syntax you want.
* DM: Earlier feedback from Google was constexpr didn’t solve the problems we wanted solved. E.g. sizing your array proportional to workgroup dimensions.
* JG: Even if you can’t nail down everything, trying something limited can give us something structured to work with. Can revisit before v1 and cut it if it’s not very useful.
* DN: Browser is also not necessarily the only tooling that will need to interpret/understand WGSL. Other tooling would need to.
* DM: The scope can be too big. Do you want the whole standard library?
* JG: That’s one reason to limit initial scope to integers. Cuts down a lot.
* DM: Initial issue was about texel offsets.  
* DM: For that, assumed we could use a module-scope let, if it’s not overridable.
* JG: That’s not allowed by the grammar right now. See const_expression grammar rule.
* DN: There’s a validation rule on the ‘offset’ parameter that it must be const_expression.
* BC: At least one backend needs a literal. 
* JP: module-scope let must not have arithmetic in its initializer.
* AB: This is where I get into how big the scope is.  If this is one problem we want to solve, then it adds restrictions on how it’s implemented.
* BC: Module-scope let is our current const-expr but highly constrained.
* DM: Why not just fix this by updating the grammar to allow the module-scope let in more places.
* JG: Its good to have first-class constexpr.
* BC: It’s offputting to have separation of module-scope and function-scope ‘let’ being different.
* JG: Would like to require use of keyword ‘constexpr’ to make it behave as one.
* JG: Think we need (1) list of problems to solve, and (2) proposals to address them.


---


# 📆 Next Meeting Agenda



* Next meeting: 2021-09-21 (non-APAC 11am time)
* [PR#2101](https://github.com/gpuweb/gpuweb/pull/2101) Syntactic CI for Grammar