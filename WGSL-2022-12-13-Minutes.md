# WGSL 2022-12-13 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN, JB

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** (APAC) Tuesday **4-5pm **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-12-06 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1bMdWQlrWlc8GZlYzax9zAfiNz2zThEzr100Il1ZYU6g) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * Myles C. Maxfield
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * Dan Sinclair
    * **David Neto**
    * Ekaterina Ignasheva
    * **Kai Ninomiya**
    * James Price
    * Rahul Garg
    * Ryan Harrison
* Intel
    * **Hao Li**
    * Jia A Chen
    * Jiajia Qin
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * Erich Gubler
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * Teodor Tanasoaia
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* Dominic Cerisano
* Dzmitry Malyshau
* Eduardo H.P. Souza
* Jeremy Sachs
* Joshua Groves
* Lukasz Pasek
* Matijs Toonen
* Mehmet Oguz Derin
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson
* Jason Erb


---


# 📢 Announcements


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


### FYIs and Notable Offline Merges



* 
* 


---


# ⏳ Timeboxes (until XX:15)


### [WGSL: allow bgra8unorm as texel format with extension bgra8unorm_storage #3640](https://github.com/gpuweb/gpuweb/pull/3640)



* AB: Discussed internally to Google.  Think we should add it unconditionally in WGSL.  For Vulkan you need to generate code in the shader to do the swizzle before store and after loading. 
* JB: Does that mean putting off codegen?
* AB: No. For a given platform (vulkan and d3d) you always generate extra code in the shader. For Metal not.
* KN: Can’t do the swizzle on the Vulkan API side for the storage format. There’s no extension support in Vulkan for bgra8unorm.  But want to support it because it’s a fast path on Metal. But does a little bit of emulation on Vulkan and D3D.  S
* KG: So you rely on authors to pick the “right” one for fastest path.  
* KN: Yes, but we think it’s better to have the feature everywhere.
* KN: Alternately, cut it from v1, or make it an option for v1.  
* KG: I would probably have the same concerns I’d always had.  Think it’s tolerable.
* KG: Probably the right choice to let authors choose. Think people will be drawn to it.  Think we had more positive message to programmatically give out of implementations to prefer one format over another.
* AB: At some point there was talk of querying preferred format.
* KN: We have it for canvases but not other usages (e.g. storage images).  We can tell you for canvas.  It’s usually aligned with preferred one for storage, but can’t always be certain.
* KN: bgra is strongly preferred on Apple platforms, which is why we’re considering this.
* KN: The polyfill on Vulkan is fairly cheap anyways. Don’t think it’s a big enough issue.
* KG: If the polyfill is cheap enough on Vulkan, wouldn’t the inverse polyfill on Metal be just the same low cost?   If we try to say the performance is good enough then just have one.  If the choice is much better, then we should be able to tell them which to prefer.
* AB: or update tooling / doc to recommend one versus the other.
* KN: We did think a lot about using bgra8 on mac without telling the author. Concluded we could not.  
* KG: Would like to see that argument, because that points in the opposite direction of what we’re concluding here.
* KN: For storage, doesn’t matter, because you can do a free swizzle on Mac.  For other textures, it’s not so simple. Some uses of textures can use swizzling, but copying can’t use swizzing.  If you have a copying-able texture, you’d have to polyfill the copy with compute shaders.
* KG: You could limit the feature for storage textures only and have it cheap and free.  If we can pick just one format and have it good and fast, let’s do that.  If there’s an argument for needing symmetry for storage and all other texture kinds, then that needs to be made.
* KN: Specifically this comes up for canvases, because that’s the only place we’ll recommend using bgra.  You use bgra on Apple because Apple says it’s faster. It’s free on Apple.  If the idea is to take that bgra texture and make it appear as rgba for all other uses, that idea we have discussed.
* KG: Concerned this feels like a really niche thing.  Don’t want to regret this later.
* KN: Agree it’s quite niche.
* KG: I don’t understand the various constraints. Hard to make an API decision. Seems every time we pick it up we make a different call.  Maybe we that shouldn’t bother us?  If others are not worried, then I can convince myself not to worry.
* KN: We would not be very opposed to taking it out of v1.  But Apple wanted it.
* KG: What does Apple actually need for their wants.
* DG: I don’t have enough information to represent.
* KN: I believe what Myles wanted is an efficient path to output to a canvas from a storage texture. Rgba8 is not sufficient because it’s not as fast as bgra.
* KN: Using the preferred canvas format for storage is free on Mac, but not free on other platforms.  Original idea was to make the extension mac-only. But then we discovered it could be implemented everywhere.
* KG: One of the things in the solution space is taking bgra in the shader, then add something on the API side to report preferred storage format. That might be the easiest thing to compromise on.  I’m concerned about the overhead we’re pushing people toward.
* KN:So a query for that preferred?
* KG: Yes.
* DG: Is it worth waiting for Myles’ return in January?
* JB: Let’s summarize the explanation and proposed direction in the issue. (So we can reload context quickly.)
* KN: I should be able to.
* KN: If we table this, we should probably address what we have in the spec. We _say_ you can do it in WebGPU spec, but no way to signal it in WGSL.   Don’t want to leave the spec as-is; because it’s in a broken state.
* **KG: How about go ahead with my proposal, and then revisit the decision when Myles gets back.**
* **KN: Yes. ** Then we can implement it and get data. Suggest landing Jiawei’s PR (if it’s still ok).
* DN: Mechanically the WGSL PR was correct, but we should remove the ‘enable’ for it. Make it unconditional in WGSL.  TODO: I’ll mark the PR that way.
* KN: And think we should make it unconditional in the spec side too.


### [wgsl: add predeclared type aliases for matrices #3674](https://github.com/gpuweb/gpuweb/pull/3674)



* **Resolved to take this PR.**


### [Distinguish between pointer and pointer contents for uniformity requirements on parameters #3677](https://github.com/gpuweb/gpuweb/issues/3677)



* JB: Will that require separating value tag from pointer tag.
* AB, DN: Yes, probably, but **details need work**


### [Restrict grammar of texture_and_sampler_types #3672](https://github.com/gpuweb/gpuweb/issues/3672)



* KG: Think we’ve talked about this kind of thing before and leaned on doing the validation in validation rather than gramm.
* DN: Yes.
* AB: There’s value in always referencing the same grammar element from many places.
* **Resolved: Leave restriction in the validation instead of the grammar.**


### [Uniform control flow vs textures for flat interpolated varyings #3668](https://github.com/gpuweb/gpuweb/issues/3668) 



* KG: Think this is again where developers don’t understand the surprising lack of guarantees.
* AB: Think your summary is correct.
* AB: If it helps, I’ve received lots of feedback that things not being intuitive to the user should be “fixed in the spec”. Thanks but can’t do that. Reality is what must be described.
* KG: Want to be able to convey that many vendors can reliably say the guarantees actually are not provided. 
* AB: There’s an extension available for Vulkan for subgroup uniformity, and need to query what stages it’s allowed on (and must include compute, but fragment is optional).
* JB: Telemetry about how widely adopted those extensions are?
* AB: Have the telemetry but can’t share it.
* AB: Look at experimental CTS tests for “maximal reconvergence” in the Vulkan CTS (which is public).  You can read it and play with it.  I can’t share experimental data about it. I can give you a link to that file. \
[https://github.com/KhronosGroup/VK-GL-CTS/blob/main/external/vulkancts/modules/vulkan/reconvergence/vktReconvergenceTests.cpp](https://github.com/KhronosGroup/VK-GL-CTS/blob/main/external/vulkancts/modules/vulkan/reconvergence/vktReconvergenceTests.cpp)
* KG: **So resolution: as much as authors want the better analysis, we can’t give a better analysis that guarantees what they want. **
* AB: And the opt-out we now have  will let them proceed if they are willing to take the risk.


### Language evolution [https://github.com/gpuweb/gpuweb/issues/3149](https://github.com/gpuweb/gpuweb/issues/3149)  



* AB: Happy restate what we think is the direction.
    * We (W3C WGSL)  increments a number for core version, and say what feature is in each version.
* AB: Know Myles was against that. What do other stakeholders think?
* JB: Idea of letting developers specify an intended high water mark and have the browser flag if they’ve gone beyond the intended limit.  Sounds cool. As long as we’re not incrementing major version frequently. (Those should be very rare). But as long as the minor version bumps have the preserving properties you’re looking for. It wasn’t clear to us how prototyping should work.  Give to users something to prototype but are not in the spec yet. How do those fit in the picture?  Also seems to run afoul of what Apple said they wanted: devs try stuff and feature detect by seeing pass/fail.  Perhaps wait for Myles to return.
* AB: Agree, not trying to get a decision before Myles return.
* AB: Experimental features are not mentioned in the spec.  Expect they would work like other we specs:  not documented in the spec, and features have browser-specific previx/suffix. And if those don’t land eventually in the spec, then that’s your users’s problems.  We want to reduce the granularity of the minor number; expect browsers to implement the incremental features without much phase difference. (Shouldn’t have one feature take a month and another take 5 years.)  Won’t have too many “this is on but that is off” interactions in the features. That’s why we think the bundling is a reasonable approach.
* JB: We like the way it avoids the combinatorial explosion. It’s just linear.  We’re not too concerned about forcing a linear order.
* AB: If linear ordering becomes a problem then that’s a sign we’re picking the wrong things to go into core.


---


# ⚖️ Discussions


### [Designing a uniformity opt-out #3554](https://github.com/gpuweb/gpuweb/issues/3554)



* ~~PR [https://github.com/gpuweb/gpuweb/pull/3644](https://github.com/gpuweb/gpuweb/pull/3644) : proposed a fine-grain and coarse-grain opt-out~~
    * Withdrawn by Google
* New Google proposal:  PR [https://github.com/gpuweb/gpuweb/pull/3683](https://github.com/gpuweb/gpuweb/pull/3683) 
    * DN:   Summary is:
        *  Describes diagnostics
        * defines “triggering rule”, creates one triggering rule for “derivative_uniformity”
        * Provides source-range-based and global mechanism for filtering diagnostics. If any error diagnostics remain after filtering, then causes shader creation error.
* JB: like the machinery, seems good to have in general. Having it in the spec will help us more opportunities to have other kinds of diagnostics.  Know we can handle them and them not be showstoppers if they’re wrong.
* JB: Concern (not a blocker), the only thing this does is affect the diagnostics that are reported. Doesn’t affect the behaviour of the analysis.  If the developer knows that in practice a value is uniform, there is no way to say that and influence subsequent behaviour of the analysis (elsewhere).  So if you have this function that you know returns a uniform value, you have to apply the diagnostic filter to every call site that is influenced by the return value of that call. 
* AB: Agreed. That’s no difference from the previous proposal.
* JB: The kinds of tools I want to influence the analysis.
* DN: Yes, can add that later as a nice add-on (which itself could not be complete, by Tex’ argument a few weeks ago).  That is not necessary for v1.
* JB: spec isn’t clear what you can apply a ranged diagnostic filter to.  It shows examples.
* DN: Right now it’s spec’d as call_expr (which is a new thing).
* AB: Can put it anywhere you can put an attribute.  Currently 
* JB: Would be nice to put it on compound statements.
* JB: The defaults are where Mozilla wants.
* DG: Started asking internally about leakage across threads.  Started looking for myself, but don’t have an answer.
* JB: Any time there’s a security problem we’ll revisit anything.
* **KG: Resolved to take this. And this solves #3554**
* DN: Q about generating “all” the warnings. As drafted, this diagnostics PR says have to generate all the warnings.
* JB: Think James’ concern is about the combinatorially many paths that are then required to generate many warnings.


### [Spec says “should warn” for unicode homophones. Should we allow control of that one via diagnostics? #3688](https://github.com/gpuweb/gpuweb/issues/3688) 



* AB: Another change somewhat related to #3683.  Spec says “should warn” for unicode homophones.  Should we allow control of that one?
* JB: **Yes**.
* AB: **Default is “warn”.**  Would think “info” is acceptable.
* BC: We haven’t implemented this warning, and I’m concerned it might be a thread we pull and find it’s really tricky to implement. Can we punt that to post-v1?
* KG: That’s why we said “should warn”. If it’s “should” we don’t have to test for it.


---


# 📆 Next Meeting Agenda Requests



* Next week: (**APAC**!) Tuesday, January 03, **4-5pm** (America/Los_Angeles)
* **Write any PRs you’ve promised!**
    * There are no other v1.0 issues outstanding (other than the ones on this agenda!) that are not blocked awaiting proposals.
* WGSL schedule:
    * _December 20: No Meeting (canceled by consensus Dec13)_
    * (Christmas Eve&Day Dec24&25, Saturday&Sunday)
    * _December 27: No Meeting_
    * January 03: APAC
        * DN: Suggest we cancel
    * Note: Based on our current issues remaining for v1 though, we are very likely to cancel meetings due to lack of discussion topics, so we can likely be liberal with skipping meetings.
*  