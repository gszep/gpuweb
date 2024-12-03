# WGSL 2024-12-03 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial++-label%3Acopyediting+no%3Amilestone+), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+0%22+), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+1%22+)

**Todos doc:** [WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previously:** [2024-11-19 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1zJfuYUOQqQY1J2ySNe4IndnbwE7T8VLJp1JVwtVtl3A) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send **dneto** a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Mike Wyrzykowski**
    * Myles C. Maxfield
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * Alan Baker
    * Antonio Maiorano
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * **James Price**
    * Kai Ninomiya
    * Natalie Chouinard
    * Rahul Garg
    * **Ryan Harrison**
    * Stephen White
    * Peter McNeely
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
    * Rafael Cintron
    * Tex Riddell
* Mozilla
    * Ashley Hale
    * Erich Gubler
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * Teodor Tanasoaia
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* Unity
    * Brendan Duncan
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


# 📢 Announcements/Meta


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


---


# ⏳ Timeboxes (until XX:15)


### [No way to sample a `texture_1d` from a vertex shader · Issue #5001](https://github.com/gpuweb/gpuweb/issues/5001) 



* DN: Started to do some archaeology here on how we ended up here
* JB: Seems like the main difference for 1d textures is that they don’t support mipmaps. `textureSampleLevel` would be weird because you don’t have mipmapping, so there’s only level=0. Seems like we should just add what we need.
* DN: Things I’m concerned about:
    * Found an issue four(?) years ago where you’d try it in GLSL and glslang would insert level=0 for vertex.
    * We did note in previous discussions that we expected users to mostly call textureSampleLevel(level=0) in vert shaders.
* DN: I’d like to expose what’s supported, so I think we need to have an investigation here. Concern about sampler addressing modes. Make sure they work.
* JB: We don’t want to deviate too much from what the platforms are doing, but we would like to give authors a hint to authors that we aren’t using derivatives here.
* JB: 
* DN: There is textureSampleBaseClampToEdge and that sounds like what we want to do here. Except it clamps the coordinates so the sampler addressing modes don’t kick in.
* KG: For external textures?
* DN: Both external and tex2d.
* KG: Do we have a textureSampleBase?
* DN: No
* KG: Are we missing textureSampleLevel(tex1d)?
* JB: Yes
* JP: We do support tex1d in textureDimensions(), and asking for OOB level is indeterminant value (also textureNumLevels)
* KG: Might be easiest then to just add textureSampleLevel(tex1d)?
* DN: If we add CTS and it says it works, that seems like the way to do it.
* **Resolved: Write CTS and Proposal**
* JP: Should this be a lang feature?
* Multiple: I don’t think so.


---


# ⚖️ Discussions


### [WIP: Add subgroups feature #4963](https://github.com/gpuweb/gpuweb/pull/4963)



* DN: AB has drafted the PR. We think it’s good editorially. We’ve implemented diag rules. There was a subgroups-f16 you had to turn on, but now there’s just one enable. So far no yelling from our clients. We have an origin trial through [jan?] so we want to know if we should extend that. We want to know how people here feel: Is this its final form?
* JB: Mozilla reviewed the proposal and didn’t see anything really wrong with it. There is the question of fingerprinting and buckets here.
* DN: We said that if you don’t support the feature, you can just report 4 and 128 for the limits. So you can always just return those.
* JB: 
* DN: I’m pretty sure I can write a shader to infer the value of the subgroup limits.
* DN: What the limit says is that you have to write e.g. if subgroup size == 8 { do this } else if size == 16 do this else if etc.
* JB: We do expect that in practice, subgroup size will be correlated with IHV anyways. But that was the only issue we could really see here.
* DN: Apple feedback?
* MW: Overall looks good, still want time to review in detail and can follow-up before next meeting, but everything looks good so far.
* DN: Alright, I think that’s everything we needed from this meeting.
* **Resolved: Good to land after positive review feedback from all reviewers.**
* JB: What were the resolution from the reconvergence “less than we expected” issues in the past.
* DN: All implementations suck, and the spec says that things happen like you expect, but in practice this isn’t lived up to.
* DN: We have one diag … as long as you pass the diagnostic, we will try to tell you.
* DN: There are driver bugs, largely due to e.g. DXC 
* DN: Microsoft says that DXC has a particular behavior promised, but is falling short of it.
* DN: We see ~5% failure rate for randomly generated testcases
* DN: We tried a super strict thing “does not ever diverge” but it ended up very similar to uniformity and ended up being a mess, and felt not worth it.
* JB: Can we do anything to incentivize things getting better over time?
* DN: Kinda did this with maximal reconvergence, but it’s opt-in.
* KG: So we can turn that on for new drivers, those users get better, and old drivers just continue to be bad as before? That’s progress though!
* DN: Yeah.
* DN: We have a dream that once we finish/polish up spec and cts more, we can hand this to IHVs and tell them it’s their problem to fix now.
* JB: So actionable by “Let’s make the CTS very picky and optimistic”, and have stress tests[?].
* DN: We did find two memory model bugs (implementation fell short of spec) and those are getting fixes rolled out.


### [Proposal: fully explicit, non-exclusive "auto" layouts in WGSL (without createPipelineLayout) · Issue #4957](https://github.com/gpuweb/gpuweb/issues/4957)



* JB: Seems like folks want a single source of expressing the binding layout group. 
* KG: There’s still a use case to specify it in the API side.
* KG: so far this is just a colloquial proposal?
* ds: Yeah we would need a syntax proposal here. I think we were hoping to get feedback from the WESL folks, but we could also try to come up with something first.
* JB: I suspect WESL would appreciate guidance e.g. “Try AB’s proposal”.
* ds: I like @interface but that’s just what we came up with initially.
* AB: I think there’s a lot of options. E.g. is it good to pass things around as a struct. 
* MW: Have we had concrete requests from devs here? Like, are devs often using explicit layouts?
* ds: Well, part of the problem is that auto-layouts aren’t re-useable, so that might push people away “artificially”. 
* KG: So is this two issues: 1) fully-explicit and 2) non-exclusive
* ds: You kinda need one for the other, because making changes in one shader would cause it to stop matching with others. 
* DN: So if you want shareable layouts, will you want the same blob in both shader sources?
* ds: Maybe? Or define multiple	shaders in the same wgsl file.
* JP: Is it true that the explicit layout thing is just syntactic sugar vs adding dummy vars?
* ds: Yes, but if you change one pipeline layout, it might change another one “spookily”. 
* JP: But authors could create compatible layouts to make them continue to match?
* JB: [It’s kind of more than strictly syntactic sugar]
* KG: So we want explicit here to “fix” things in place to prevent “spooky at a distance” behavior, but we think this is a blocker for non-excl auto layouts.
* ds: That’s my understanding from talking with KN
* AB: Would be interesting to take to WESL: About composability of layouts, having to rebind things. At least you could say “here’s a layout” to pass round. 
* KG: do we have an @explicit attrib for this kind of thing yet? Or that’s what we’re asking for? Because if we did, we could e.g. Warn if you ask for an `auto` layout from something where not everything’s marked.
* DN: Could have a dev feature to lint for whether there’s something in a helper/util func that accesses something not in some allowlist of entrypoint “expected bindings”.
* **Resolved: Sounds cool, but please make a spec proposal. FYI: We kind of like the struct approach.**
* JB: I would really like to have people try things before speccing things. I think it would be great to have real devs give feedback on real use of this. I don’t know how fast the WESL folk would be able to prototype this, but it would be great to get some kind of user feedback here. I don’t want to Require us to get this feedback before proceeding, but I think we should try to ask for this. But e.g. Origin Trials feels too high-overhead for this. 
* KG: I think this would need to be prototyped in an implementation because this is lifting a restriction we have, not polyfillable. But an impl can just add a behind-a-flag prototype for this.
* AB: Not to put MW on the spot, but this is kinda like the transition from ~GLSL to ~C++-via-MTL. Did people like that?
* MW: I agree with the overall direction, just not sure how long it will take, and not sure how.
* DN: I want to go look at the Slang research again, because they address interface reuse, so that comparison would be nice.
* 


### [Proposal: "auto" layout algorithm should be able to generate any layout · Issue #4956](https://github.com/gpuweb/gpuweb/issues/4956)



*  JB: Does it make sense that attributes apply to the global var and not the wgsl type of the sampler? Is this a thing that everyone lets us do whatever we want and we can just leave it polymorphic, or do we want to make it part of the type system?
* DN: Good question. :) 
* **(Out of time)**


---


# 📆 Next Meeting Agenda Requests



* Next meeting?: (**Atlantic-timed**) Tuesday December 10, 2024, **11a-noon** (America/Los_Angeles)
    * Can we talk about:
    * 
* But email Kelsey or the list if you want to meet sooner!
* 