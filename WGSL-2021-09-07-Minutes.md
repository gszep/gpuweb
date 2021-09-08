# WGSL 2021-09-07 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday **<span style="text-decoration:underline;">***5pm-6pm***</span>**

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
    * Alan Baker
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


### On Todos



* (JG: I will make a spreadsheet for thumbs-up/down/maybe for each todo)
* 


---


# ⏳ Timeboxes


### [Fill out "Textual structure", eliminating several TODOs #2087](https://github.com/gpuweb/gpuweb/pull/2087)



* DN: I made a defined term called “blank space”, not versed in unicode, so I’m not sure it that’s the entire set of things we want. We also talked about how `>>` and `]]` can maybe require backtracking, but I filed another issue for those. [#2092](https://github.com/gpuweb/gpuweb/issues/2092) I also didn’t describe nested comments, in order to keep this simple.
* MM: Sounds good to us.
* **JG: Let’s take it.**
* 


### [precise_math attribute on functions #2080](https://github.com/gpuweb/gpuweb/pull/2080)



* DN: Further discussion today: Concerns that it’s not testable. Also concerned that it’s not stable, no way to make sure that (because untestable) it will keep working. One possibility is to make it an extension with actual strict requirements, but without signing us up for these sometimes-impossible strict requirements in core.
* MM: In version of spir-v that wgsl is targeting, there’s no way to guarantee/require ieee floats?
* DN: OpenCL can, but not spir-v in general.
* DM: Spir-v doesn’t support some things, but not everything we need?
* DN: NoContraction is visible, feature in vulkan-spirv. Not sure how strict the vulkan conformance tests are good enough to guarantee what we need. I would need more time to test whether it’s feasible on vulkan.
* MM: When this extension is enabled, we can test that the math would be right. Problem is when we don’t have the extension, where we can’t really guarantee anything.
* DN: When this was “SHOULD”, it’s easy to try for. Making an extension would require more work to see if we can support “MUST”.
* GR: Why was it said that this would have no effect on DX12?
* DN: I think the original poster did minor testing and didn’s have issues, so didn’t look into this?
* GR: We do have `precise` which should work, but yeah, not sure how to test non-precise.
* DM: `precise` is applied to variables? (yes)
* MM: Another question, is can global variables be precise? This leads me to a recommendation that it be per-module, and also that this all Metal can handle.
* DN: Could (galaxy-brain idea) compile multiple times with and without precise as needed, since we control when entrypoints are used?
* MM: Compiling multiple times would be bad, because compiling is already slow.
* DM: I think we sort of already handle this (function granularity) in Metal backends.
* MM: Why per-function, when no native API does that. Metal is per-module, others are per-variable.
* DN: There’s some concerns about how deep (into function calls?) to propagate `precise` when on variables, at least in a way that’s not super verbose.
* (timebox hit, tabled to next meeting)


### [Rewrite select() for scalar condition and add missing algorithm tags #2084 ](https://github.com/gpuweb/gpuweb/pull/2084)



* (DN: “I need some more time for review.”  Particularly the editorial aspect:  how should builtin functions be described: using type assertions vs. function prototypes (which are not a defined concept.)
* DN: I see a non-editorial aspect here, and so the group should discuss that. However, I wanted to just split out and land the (easy) editorial part first.
* MM: We’re ready to discuss the substantive proposal here. We think it should be fine. If it’s not natively supported, can broadcast.
* DM: I’m worried about adding implicit broadcasts.
* MM: I think it’s a natural addition here but I won’t fight for it.
* DM: I would like to see an investigation of existing languages, if they offer it, and therefore if people use it.
* DN: I can probably do that investigation as part of the review.
* **Result: DN to split out, do review, maybe investigation mini-report**


### [wgsl: support arrayLength on pointer to sized array #2081 ](https://github.com/gpuweb/gpuweb/pull/2081)



* DN: Ben followed up here, original poster wasn’t aware of the ability to resize arrays based on global lets (not implemented in Tint?), and their usecase didn’t need it(?), so our purpose here is more tenuous.
* DN: Also, some of us don’t like the pointer form, but also, some of us don’t like the non-pointer form. Particularly having to maybe (officially) do the evaluation of the expression in order to determine the size.
* DN: As such, we’re not sure we need this, so we’d like to push this post-MVP.
* MM: A little sad about this. We’d like to see this eventually, and we’re ready to talk about this in more depth now, but we’re ok with pushing it back.
* DM: Why did people not like the pointer form?
* DN: I’m trying to represent another person’s stance, but people don’t like the extra character/typing to do this.
* MM: Ideally we’d like to see this for all array types. For now, because of the shape of things, it’s easier to not allow it on `let` arrays for now, even if we want to go further eventually.
* JG: I’m having trouble entirely following here, so examples of good-for-me-thing vs bad-for-me-thing, that’d be easier to follow for me.
* DN: I’m trying to capture the warning here about disadvantages to the pointer approach, but not to block it. We will definitely have to keep explaining this, but maybe that’s fine.
* MM: I’ll echo that. Also the language is worse off if theres *no* way to do this. *A* way to do this would be an improvement here.
* DN: Ok, ok to land the pointer form? (yes)
* **Result: Land pointer form, MM to open issues for the other forms.**
* DM: I hope we don’t need more forms, ideally.
* MM: It’s a good point that we should consider all this in respect to the rest of the language.


### [Error with literal indexing in a shader #1803 ](https://github.com/gpuweb/gpuweb/issues/1803)  



* DN: AB strongly wants us to have consistent behavior for OOB access here, so more discussion is needed here.
* DM: The method for OOB protection is in fact different for different types here, so I’m not sure we *need* them to all be the same.
* RM: While I think consistency is important for portability, I think it’s fine to have this be compile-time.
* MM: Whenever we expand constexpr, if we do this analysis, this should apply to all indexing, not just literals.
* DM: Right, it’s not that it’s an option *not to* for e.g. let-arrays and OpCompositeExtract
* MM: I think the job for the compiler is the same.
* MM: DM, can you restate the OOB behavior point?
* DM: On uniform and storage memory backed arrays are relying on robust-buffer-access, but everything else uses manual code injections for guarding against OOB access. Point AB was making was that we should have consistent behavior between these different types. I’m saying that we’re already doing something different, so why pretend we’re not. 
* MM: So the consistency argument isn’t particularly strong for you here.
* DM: Two types of consistency here, so it’s not clear-cut.
* DN: Today, spec gives you an Invalid Reference if you reach outside your array. If this is still inside the robust-buffer-access, the hardware feature will not necessarily kick in to save you. I haven’t thought it all the way through, but I worry about us being incomplete here.
* MM: Ok, so you have a buffer of ten-element arrays. I think it’d be bad if it had to do access checks against both the ten-element array, and also against the full buffer/outside array. If we only have one for performance reasons, we only want the latter.
* DM: I think that’s actually what we have now.
* DM: I think we might have spec’d something even less restrictive, maybe for local variables? We should double-check that we haven’t written divergent things.
* MM: Can we tentatively resolve and ask for reversion if need be?
* DN: Would prefer to get input first, preferably in written async form.


---


# ⚖️ Discussions


### [Match let declarations for inferred types #2053 ](https://github.com/gpuweb/gpuweb/pull/2053)



* Earlier, the group agreed not to do that.
* DN: accidentally added an example showing it works; but grammar still prohibits it.
* DM: [https://github.com/gpuweb/gpuweb/pull/1944](https://github.com/gpuweb/gpuweb/pull/1944) fix the example, and clarify the prose
* MOD: [#2053](https://github.com/gpuweb/gpuweb/pull/2053) updates grammar to allow it
* Tint supports it, and has some Google-internal users have used it.
* DN: Story is, we said we didn’t want inference for global scope, but then I wrote a bad example, that wasn’t actually valid, so here we are.
* MM: Why didn’t we want this?
* DN: I think we had some voices that were concerned, e.g. about the amount of work. But we did the work anyways
* DM: Yeah I think it’s not too much work. Any reason not to just take it here?
* JG: I think it would be good to revive the reasons that we’re not deciding in spite of.
* DM: I think it was mostly the perceived amount of work, which we’re now not worried about.
* **Result: Let’s take it, and revert later if we’re wrong.**


---


# 📆 Next Meeting Agenda



* Next meeting: 2021-09-14 (non-APAC 11am time)
* [[wgsl] Support compile-time-constants (constexpr) · Issue #1272](https://github.com/gpuweb/gpuweb/issues/1272) 