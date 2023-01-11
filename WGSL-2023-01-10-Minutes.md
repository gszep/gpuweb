# WGSL 2023-01-10 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** JB

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** (APAC) Tuesday **4-5pm **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-12-13 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1S8t2bTAHyNimVeF9LpL3qgNpEBJb6qr6JqQw2dnSlAE) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * **Mike Wyrzykowski **
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
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * Dan Sinclair
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
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * **Shaobo Yan**
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Microsoft
    * Damyan Pepper
    * Greg Roth
    * Michael Dougherty
    * Rafael Cintron
    * **Tex Riddell**
* Mozilla
    * Erich Gubler
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * Teodor Tanasoaia
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* Unity
    * **Brendan Duncan**
* Dominic Cerisano
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


### FYIs and Notable Offline Merges



* FYI:  text/wgsl media type registration
* DN:  No feedback from [media-types@ietf.org](mailto:media-types@ietf.org).  So I submitted the registration via form at [https://www.iana.org/form/media-types](https://www.iana.org/form/media-types) 
* DN: Have one feedback item: to reference WebGPU 2.2 privacy considerations.  Will do.
* 


---


# ⏳ Timeboxes (until XX:25)


### [WGSL: allow bgra8unorm as texel format with extension bgra8unorm_storage #3640](https://github.com/gpuweb/gpuweb/pull/3640)



* (Merged, but Myles had a follow-up question)
* MM: If you have a BGRA texture bound as a storage texture, in Vulkan this is impossible - it has to be polyfilled by attaching an RGBA texture, and then emitting additional instructions to do the swizzle
* AB: In D3D you have to have a packing instruction instead of a swizzle, and would load+unpack from an r32uint
* MM: I believe there is no shading language today where the swizzling is contained within the shading language itself, where the language itself makes a distinction between bgra and rgba textures. Is this true?
* AB: Not sure. In SPIR-V, I don’t think you write BGRA anywhere
* MM: The problem is that if someone has a shader in another language and they want to compile it to WGSL, then they have to divide up this information about whether it’s BGRA or RGBA. Can we avoid this? We could force the person to add this information in order to produce WGSL, but what would it take to make it unnecessary for them to add this information? And for the texture types that do require additional instructions, could we unconditionally have the instructions around the store, but predicated on some other contextual information, so that the side channel information could be provided at compile time, and used to either swizzle or not swizzle (hopefully without a branch). Was this considered in Nov/Dec?
* DN: When compiling from a different language, if the other shading language doesn’t distinguish between BGRA8 or RGBA8, then why not always unconditionally map it to the RGBA8unorm? Isn’t that viable?
* MM: Suppose you have a two-pass algorithm: first pass renders into a BGRA texture, using a rendertarget. That writes data 1,2,3,4. Then you have a second pass algorithm that attaches that output as a storage texture, and you read from it. Ideally, you’d want that read to also be 1,2,3,4. That seems to fall down.
* AB: Right now we don’t do storage reads. We do texel loads. We swizzle on write, so when you sample later, it’ll get the right components. I think all this was predicated on the idea that it was a performance benefit on Metal to expose the BGRA. IS that actually the case?
* MM: It’s important that the results of this discussion aren’t, let’s remove BGRA, because our compositor can’t handle RGBA, so we want it to at least be possible for a WebGPU impl to produce an BGRA texture, so we don’t always have to do a blit. That’s why there’s a performance concern. The perf of loads and stores in a shader isn’t the issue.
* KG: Can we say storage is always rgba8unrom?
* MM: Alternate: shader lang doesn’t say it’s rgba or bgra, and be able to bind either kind to your shader.  Then have your shader be flexible with that runtime side channel flag.
* AB: We require a format in the shader for storage texture. So you’re saying always require that to say rgba8 but then exploit side channel to conditionalize the swizzle in the shader code.
* MM: Benefit is for the shader porter.  Move the burden to the API implementor.
* AB: Don’t have enough context.  Not sure it’s (...missed this).
* BC: Are there other formats that require swizzling in the shader?  So this new magic thing would apply to this format.
* MM: Yes.
* BC: Feels a bit funky.  What we have is very explicit. The API to the shader is 1-1 mapping.
* AB: We discussed about loosening the validation on the API side, and allow binding either.  But API folks decided intentionally to not allow that.
* MM:Ok, **Glad it was explicitly discussed. Wanted to make sure it was on the table**.


### [Distinguish between pointer and pointer contents for uniformity requirements on parameters #3677](https://github.com/gpuweb/gpuweb/issues/3677)



* DN: JP has coded it up, but **we need to go write it down for the spec** still. We’re happy with the semantics though.


### Language evolution [https://github.com/gpuweb/gpuweb/issues/3149](https://github.com/gpuweb/gpuweb/issues/3149)  



* AB: I recall, a while back Myles had queued up discussion points around this issue.  Would like to hear them if available. Google has expressed an opinion (e.g. high water mark of used features).
* MM: Don’t have queued up issues. Did post to the issue last night.
* AB: When we’ve discussed the JavaScript model internally. On the API side, querying when things exist or not makes sense. On the shader side it doesn’t seem great to do a bunch of compile-fail.  We agree with overall major point: don’t want version hell.
* MM: True the “version hell” is our primary concern.   The larger model is concern #2.
* AB: Definitely agree with the major concern.
* KG: Hard to tell lessons learned from WebGL, WebGL 2. While we do have version declarations in the GLSL that are not strictly compatible; but perhaps it’s not too onerous?
* MM: Can speak more why about the primary concern. First, it’s way easier to maintain compiler if you don’t have to maintain all the version layers.  (So benefit to us, to reduce complexity).  For developer learning, searching for tutorial for WebGL, many of them are very old.
* KG: Does that not run counter to the desire that whatever is on tip is the “real one”. Isn’t it a benefit to see “ah, this example is old”.
* MM: E.g. look for GLSL tutorial that may say “varying”. Would like the JS model where wouldn’t break backward compatibility.
* DN: Isn’t that concern about breaking backward compatibility?
* AB: Really the concern is when introducing new things that are close to old functionality. Don’t want 50 ways to do very similar things.
* MM: Can see “do we have version numbers” is distinct from “changing keyword”. But I will just make the observation that GLSL started with version numbers, and then felt free to rename keywords.
* KG:  Can agree we should agree it’s bad to rename keywords. I’m not particularly concerned about slipping up: when we might do it, restate the argument, and it’s compelling.
* **Can think more, but nothing concrete to conclude in this meeting today.**


### [Formal parameter scope is the function body #3719](https://github.com/gpuweb/gpuweb/pull/3719) 



* DN: From someone at Google who tried a bunch of things that work in Rust that don’t work in Tint. For formal argos to user funcs, they are instantiated when they are declared, so e.g. i32: i32) -> i32 validates “weird” because the return type/ident comes after the formal param named `i32`.
* JB: Isn’t e.g. i32 an ident keyword? Wouldn’t that not be able to be an ident?
* DN: We’re agreed to allow that, just not in the spec yet.
* BC: FWIW I did have to replace i32 with a type alias, but the main point still stands
* JB: We like this.
* MM: Neutral on this.
* KG: Only thing that comes to mind is e.g. c++ template pattern like &lt;typename T, T U>.
* JB: It seems unlikely that you want to write this, but if you did, it does feel weird that it’s forbidden.
* **Resolved accept**


---


# ⚖️ Discussions


### [Designing a uniformity opt-out #3554](https://github.com/gpuweb/gpuweb/issues/3554)



* Also:
    * [https://github.com/gpuweb/gpuweb/issues/3689](https://github.com/gpuweb/gpuweb/issues/3689)
    *  [https://github.com/gpuweb/gpuweb/pull/3713](https://github.com/gpuweb/gpuweb/pull/3713)
* JB: Are there really two PRs? Would be nice to reduce to one, if so.
* DN: [yes?]
* DN: Briefly, uniformity issues are an error in compute shaders, but only a diagnostic (soft) error for fragment shaders. [...]
    * Uniformity failure triggers a diagnostic
    * Diagnostic is can be filtered
    * But if not filtered by default then it produces a shader creation error.
* MM: So the opt-out is for disabling the diagnostic, not modifying the dataflow through the graph?
* DN: Yes
* JB: Do I understand that the reason for collecting these sets, when we have a user-def function, we have the detail to attribute the error to specific builtin calls in the user-def funcs?
* DN: Yes, we don’t want to have to go back and re-calculate this
* JB: I would like another week to better understand the language used here.
* DN: In real life, you’d construct this differently anyway.
* JB: Doesn’t that not work, because the analysis needs to match?
* DN: If you turn off the error, then it can’t cause an error, so you might as well not emit that edge.
* JB: But in terms of how we assign tags to funcs, diag tags have no applicability to [...]
* DN: [...]
* DN: The obvious thing to do in an implementation, 
* KG: Would prefer to have pseudocode that implementors implement rather than spec language that is different from how it should be implemented.
* AB: The modifications david was referring to is good for turning something “off”. But not great for different levels of “warn”, “info”, “error”. So it depends on the maximum severity that may be in effect.
* MM: Nitpick: It would be nice if we referred to these by the [class of] builtin that they are, rather than the type of shader. [imagine if we added workgroup to frag shaders]
* MM: We are discussing with our HW team about how bad it is to get derivative stuff wrong.
* JB: If it’s “bad derivatives are a disaster”, then we have trouble
* MM: Just trying to reflect our harsh realities. 
* AB: Regardless of where that lands, this is two things, one is opting out, other is introducing a way to talk about severity of diagnostics.
* MM: Sounds legit. We don’t have opinions about details about opting out of UnAn. Mechanism for quieting the error seems fine, we’re mostly concerned about security here.
* DN: Sounds like we’re on the same page. 
* MM: What happens if you try to opt out of UnAn in a compute shader.
* DN: Only opt-out is actually for `derivative-uniformity` as an attribute, so it wouldn’t apply.
* MM: sounds like we should warn if it’s used where it doesn’t make sense.
* KG: UAs are allowed to emit warnings!
* **Resolved: Write more and better spec text.**


---


# 📆 Next Meeting Agenda Requests



* Next week: (non-APAC!) Tuesday, January 17, **11a-noon** (America/Los_Angeles)
* F2F is in San Francisco Bay Area.  Specific host to be determined.
    * See email: Feb 16, 17. Intended to be in person, but definitely have remote options.
    * Roughly each day is: 2 blocks 4 hours morning and afternoon. Lunch together.