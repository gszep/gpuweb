# WGSL 2023-04-04 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN, DC

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **4-5pm **Americas/Los_Angeles (**Pacific-timed/APAC**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-03-14 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1u_Dp57ehvA3GNIDNiq2XTZnNmRKy8tBIJZB61ZqLo3E) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * Mike Wyrzykowski 
    * **Myles C. Maxfield**
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * dan sinclair
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Price
    * Rahul Garg
    * Ryan Harrison
* Intel
    * Hao Li
    * Jia A Chen
    * Jiajia Qin
    * Jiawei Shao
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * Zhaoming Jiang
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Microsoft
    * Damyan Pepper
    * Greg Roth
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
* Unity
    * Brendan Duncan
* **Dominic Cerisano**
* Dzmitry Malyshau
* Eduardo H.P. Souza
* Jeremy Sachs
* Joshua Groves
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
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


### “Pacific-timed” instead of APAC



* KG: Someone suggested (sorry, I forgot who!) that instead of “APAC” and “non-APAC”, we call them “Pacific-timed” and “Atlantic-timed” respectively.
* KG: I like this, so I’ve started moving that way.
* KG: **What do we think?**
* 


### FYIs and Notable Offline Merges



* WGSL syntax highlighting support contributed:
    * Pygments.  Landed
    * Chroma. Landed
    * Monaco-editor (the editor for VS-code):  PR [https://github.com/microsoft/monaco-editor/pull/3884](https://github.com/microsoft/monaco-editor/pull/3884) (only “major” languages are accepted, let’s see…)
* [PR #4011](https://github.com/gpuweb/gpuweb/pull/4011) Described what a dynamic error can do.
* [PR #3956](https://github.com/gpuweb/gpuweb/pull/3956) Out of bounds access via reference is dynamic error
    * This also allows trapping behaviour, closing [#3727](https://github.com/gpuweb/gpuweb/issues/3272) 
        * Backed by this from the minutes “
            * AB: In my mind, remove all text and don't discuss behaviours and just say it's a dynamic error. there are multiple levels, what you read in spec is not what's being guaranteed.
            * KG: Right, this is what gives you trapping, you just get it.: 
    * In particular [https://github.com/gpuweb/gpuweb/blob/main/wgsl/index.bs#L9959](https://github.com/gpuweb/gpuweb/blob/main/wgsl/index.bs#L9959)  (from the [Uniformity analysis overview](https://gpuweb.github.io/gpuweb/wgsl/#uniformity-overview))
        * “The analysis assumes [=dynamic errors=] do not occur.

            A shader stage with a [=dynamic error=] is already non-portable, no matter the outcome of uniformity analysis.”

* James pointed out this **effectively reverses **the earlier discussion that resulted in adding workgroupUniformLoad.
    * 1. James [argued](https://github.com/gpuweb/gpuweb/issues/2586#issuecomment-1062068883) that non-atomic loads from shared memory would only have non-uniform results when there was also a data race.  He proposed that WGSL rules should assume no data races, and therefore uniformity should infer non-atomic loads from shared memory are uniform.
    * 2. Committee argued against. (Dzmitry [here](https://github.com/gpuweb/gpuweb/issues/2586#issuecomment-1062536148), Myles [here](https://github.com/gpuweb/gpuweb/issues/2586#issuecomment-1068250337)), so uniformity analysis assumes non-atomic loads from shared memory are non-uniform.  This motivated adding workgroupUniformLoad.
* Should we weaken uniformity analysis, i.e. take James’ earlier proposal?  This would allow more compute shaders (which don’t have an escape hatch).
* 


---


# ⏳ Timeboxes (until XX:20)


### [wgsl: unpack2x16snorm exhibits 3ULP error on AMD GPU on macOS Ventura #3991](https://github.com/gpuweb/gpuweb/issues/3991)



* (Don’t know why this doesn’t also affect unpack216unorm?)
* KG: Concern with polyfill is it might not work perfectly?
* KG: One possibility: mark it in the spec as you should get the right answer but
* MM: Why is 3ULP so bad?
* KG: It’s a data conversion that should be better.
* DN: It’s a multiply by a constant 1/32767.
* AB: Everyone else says it’s more accurate.
* KG: If this is a hot path for someone’s app, don’t want the app to always polyfill it out of fear.
* AB: It’s not clear if texture fetches have any accuracy defined, when they do 
* JB: on this GPU, do texture fetches work with good accuracy?
* AB: We don’t know yet. Ben was in the middle.
* AB: Metal spec says 1.5 ULP on these kinds of image format conversion functions.
* AB: AMD GPUs on certain macbooks.
* MM: Would like a more precise GPU name to be able to track it down.
* MM: All the other error bounds are “what you get in practice”; why make a special case out of this.
* AB: Usually that applies across many.  But this is one is a particularly special device on one OS.
* MM: Would be concerned if it was a large error.  But 3ULP is not terribly bad.
* AB: It’s AMD Radeon Pro 555X 
* MM: Can tighten up in the future.


### [Add a floored or euclidean modulo implementation to wgsl #3987](https://github.com/gpuweb/gpuweb/issues/3987)



* (Previously “modf() should either be renamed or have a different implementation”)
* JB: We talked about this this morning. Our thoughts are that there are three common definitions.  There is no harm in expanding the library to offer all of them.  No need to change what we have.  Can offer more.  Don’t need to change what we have.  Can be forward compatible.
* AB: We don’t mind adding a new function.
* KG: when we were trying to dissect this internally, we wished that it would be crystal clear which one was happening in different places.  E.g. A much longer builtin name  e.g. modulo_trunc, then define the  a % b defined as the application of one of those longer names.
* ~~AB: For reference, we don’t define % for floats (?)~~
* DN: Wikipedia page has what looks like common names. Let’s use them or derive from them.  And it has really  nice diagrams.
* **KG to propose builtins**


---


# ⚖️ Discussions


### [Filterable diagnostic names should be checked, but also permit third-party diagnostics · Issue #3989 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/3989)



* JB: Looks like we have a consensus proposal.  Mozilla is fine with the three-arg form.  One final pitch for the dotted form; it takes the possessive notation from the language and uses it for this other thing.  That’s what Rust has done.  Rust has a very close facility.  It’s a tried and trusted form of solving this.  A minor notational question is much less important to us than giving warnings when the diagnostic name is mis-spelled.  It’s up to the committee.
* DN: As to having a preference, it’s probably BC who has the most concern about anticipating the design of modules, so maybe best to get him on the call before deciding.
* JB: If I’m the only one who likes the dots, I’ll cede, but also in my mind the similarity of dots to modules-ish things is a *benefit*, because that’s what we’re trying to draw to mind.
* DG: I also lean towards JB’s dots for similar reasons.
* DN: I imagine BC would ask: If we do modules, would we do identifier resolution lookup here?
* JB: No, just for suggestive effect.
* DN: Ok! **I’d rather have BC on the call then to move forward**.
* JB: Alright!


### [Shader extensions are redundant · Issue #3961](https://github.com/gpuweb/gpuweb/issues/3961)



* KG: Feedback from Mozilla is we think it’s fine to do it in both places. There are advantages to having it in both places.  One advantage, as noted already, is it’s an in-band assertion within the shader that is readily readable. Think that’s a benefit.  Technically it is redundant, but it’s ok to be overconstrained.  You can imagine a case where the shader would have it, but didn’t enable it at device-creation, and it’s ok for the browser to complain.
* KG: One of the things you do at device-creation, and another is at shader-creation.  Many apps are single-focused and know the bounds of what they will require.  But middleware that might need the feature later, will have to ask for the feature up front. But if we got rid of this redundancy it would be implicitly enabled in all the shaders, but might have a cost.
* MM: Thought I addressed the issue. Example is f16, and there is no cost to turn it on. Yes, there are hypothetical features in the future that have a cost when turned on.  But the signal for that doesn’t have to be in the shading language.  For those cases, it’s ok to remove that extra information into the pipeline creation call rather than in the shader source.  And in that case it’s not redundant info.  Our goal is to eliminate true redundancy.
* JB: Would you be amenable to changing the title to “shader-f16” is redundant.
* MM: “shader-f16” is too narrow.  Looking for general rule for cases where the feature is truly redundant.  Having that true redundancy is setting themselves up for failure.
* KG: Think OpenGL does things poorly in this space. Think we did better in WebGL.
* MM:I’m with you for things that have cost.
* KG: Kind of asking for a lint.  The lint is “am I using shader-f16 when I haven’t enabled it at device creation”.
* MM: If you haven’t enabled it at device creation, you can’t do anything about it in the shader. 
* KG: If you haven’t declared it at device creation, then we have to reject the shader.
* KG: If you have declared it, … (?)
* MM: If it doesn’t cost, then why not always enable it?
* KG: User enjoys that the browser has these built-in lints. What we’re weighing is should we be slightly less redundant in a way that doesn’t matter much?  Or users can lint with what we already have in the browser.  Two relatively minimally beneficial things. Don’t think the goal of “being less redundant” overrides the other thing.
* JB: I’m understanding Myles’ argument better. We can treat ‘enable shader-f16’ to lint uses of f16.  We decide to lint for specific reasons. We specifically lint things that have some kind of problem attached to them.  Perf cost, programming risks.  Myles’ argument is ‘f16’ is not interesting. He’s saying in this particular case, f16 isn’t different from all the other language features we have.  I see that….
* KG: This is a portability lint.   Folks write code with “if it works on my machine i assume it works on others”.
* JB: But device creation fails….
* KG: You often test on your powerful machine, and seldom on your low capability thing.  When you write this one new shader, you accidentally use this portable-hazard feature.  Today you include or not include in the shader. If you develop this app, your expectation is it will be portable.
* JB: You can get that same testing device without asking for f16.  
* KG:If you realize that you have that portability hazard.
* KG: WebGL got really far by forcing you to always say these things.  And being pedantic.  The benefit is that it almost always worked on low spec devices. Slight redundancy gives us benefits.
* AB: Overall, agree with KG.  There are benefits to being in the shader, and not necessarily cost at all.  The small cost is smaller than the benefits.  You get a cleaner split between API and compiler. Get better offline tooling.  Benefit to separation between asset creation time and deployment.
* MM: The user story Kelsey gave about which programs work on which device. Think redundancy actually works against that. If you have two sources of truth then the author will be confused.   Second is philosophical. That separation between shading language and API, is ideally reduced not enlarged.  People using WGSL are going to be using WebGPU. They would already declare which features are to be used.
* KG: Assert it doesn’t cause confusion.
* JB: Also interested in high quality offline tooling.
* JB: You say enable shader-f16 has benefits.  If you have free reign. Alan, if there are other places you’d add.  Kelsey is making a case in favour of them. What other things would we be flagging this way.
* AB: Already thought we reached consensus.  Things that are not prevalent on all hardware. Think that’s reasonable.  If you’re expecting some feature that’s not everywhere, then do so. May expect some of them to become superfluous. We know other features are coming soon that are not universal.
* DN: Want to repeat the point by JonahPlusPlus: shaders are developed separately from the app itself. Separation in time, space, skillset and who is doing the authoring.  The enable forms one half of the handshake with the app deployment.  Make the contract clear and smooth.
* KG: There are other places we’ve ceded the declaration. But don’t want to cede this one down to zero.  Benefits outweigh the cost of the redundancy, therefore don’t want to eliminate it.
* JB: Would like to talk more about the handshake.
* AB: Hope I understood Myles’ point.  Anybody who uses WGSL will use WebGPU.  Have partners that don’t start with WGSL. IT’s better to capture signals earlier in their flow.
* MM: If you’re on a device that doesn’t support f16, or you’re on such a device but haven’t enabled it.  Then if you use an f16 shader then that compilation will fail.  Now take the shader and add f16; it will still fail. Adding it didn’t benefit.  Take the converse.  Enabled it on the device, but then the shader will or will not run depending on if it is otherwise correct. (assuming we relax this rule).
* KG: This is like defense in depth against accidental portability leaks. Additionally I see it as a benefit to be listed somewhere affirmatively in the shader.  It’s a benefit not a disadvantage.
* MM: What’s the difference between this and a version? Don’t see this as different.
* KG: I see it as different.
* AB: I would reiterate Kelsey’s argument is where we reached previous discussion “because we have A, doesn’t mean B follows”. If we wanted the most portable thing is “v1”. After much discussion we decided not to do that. Lots of arguments on whether to be a version. Decided one way.
* KG: Can draw similarities.  But analogies are imperfect. The difference lies in there.
* DN: For versioning, if I have a machine/browser that doesn’t run the app, then my solution is to wait for a software upgrade to support that language-version thing.  But if I can’t run it due to hardware shortcoming, there is no solution to me on that machine. I can wait forever and it still won’t be portable to that machine.
* KG: Would like to close the communication gap.


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed/non-APAC**) Tuesday April 11, 2023, **11am-noon **Americas/Los_Angeles
* 

 
