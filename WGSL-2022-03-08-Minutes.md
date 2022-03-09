# WGSL 2022-03-08 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️ Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon Americas/Los Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-03-01 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1NVjJlWiitJrc3IJMyATyJrc3vvMc-ZYu9S0lPn6bPuQ) 

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
    * **Ben Clayton**
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
    * Jaebaek Seo
* Intel
    * Hao Li
    * Jiajia Qin
    * Jiawei Shao
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * Zhaoming Jiang
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * Dzmitry Malyshau
    * **Jim Blandy**
    * **Kelsey Gilbert**
* Connecting Matrix
    * Muhammad Abeer
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* UC Santa Cruz
    * Tyler Sorensen
    * Reese Levine
* Dominic Cerisano
* Eduardo H.P. Souza
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Michael Shannon
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
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


---


# ⏳ Timeboxes


### [Allow statically unreachable code #2622](https://github.com/gpuweb/gpuweb/pull/2622)



* (Needed Kelsey’s review)
* _Merged._


### [Should clamp be only defined for low &lt;= high? #2557](https://github.com/gpuweb/gpuweb/issues/2557)



* DN: Our position is that we don’t want to be force to polyfill, and as spec’d we would need to.
* MM: Agree that clamp needs to be fast. But we’re not talking about the claiming that browsers do, which is different from the problem we’re talking about for user code to normalize clamping behavior.
* MM: We’re therefore talking about maybe-adding an extra instruction 1->2, where we don’t have compelling evidence the perf in *user* clamps is a problem, because we don’t know the frequencies here.
* DN: In the user-clamping case, flipped low/high is a misuse of the API.
* MM: I don’t know if I agree that it’s misuse.
* GR: I would not call it fully-standardized on hlsl, but rather that *today* hlsl happens to ensure standardized behavior here.
* MM: Hard to know what the trade-offs are here, so it’s hard to decide.
* AB: I feel like we’ve made decisions before based on commonalities in underlying apis/hardware, and that that prior art is pretty strong pressure.
* GR: Worth noting that maybe HLSL’s experience shows that it’s probably not a perf problem for users, since they haven’t complained, and (to my knowledge, no ihv tries to replace min(max()) with clamp().
* DN: From another direction, one alternative is to just remove the builtin entirely. If you want min(max()) use min(max())
* DN: No native APIs specify it as anything in particular.
* MM: HLSL does polyfill this anyway on all devices.
* **(timebox’d)**


### [Concise attributes · Issue #2589](https://github.com/gpuweb/gpuweb/issues/2589)



* MM: Discussion with Robin. This is a proxy for new users vs. expert users. New users would probably want more characters, whereas expert uses would probably want fewer characters.
* RM: A debate that we’ve had for a bunch of features, that we’ve never really fully come down one way or the other.
* Straw poll:
    * KG: Weak no.  Difficulty in searching things. Having multiple way to say things makes it hard to find docs. Not particularly sharp here. Don’t think writability helps that much. E.g. stage vertex
    * MM: We abstain
    * JB: If people very enthusiastic about some parts, possible to have short forms for things like shader stages, but not for builtins. Shader stages seem different.  @vertex is much easier to swallow than @builtins. Builtin variable seems wide and growing. When see things in that category, not necessarily recognize it.  The wrapper of  ‘builtin’.
* **JB to draft PR that picks and chooses.**
* JP: I do think @flat is a good candidate, for verbosity’s sake.
* MM: I actually would prefer to see `interpolate` there
* DN: +1 to JB’s analysis (shader stage but not builtins)
* GR: I’ll say that I work with very few people who write their own shaders, and so for these people that generate the code anyways, this certainly isn’t useful there.
* 


### [Should struct member declarations use commas instead of semicolons? · Issue #2587](https://github.com/gpuweb/gpuweb/issues/2587)



* **Needs spec!**
* MM: Would it be illegal to use semicolons then? (yes) 


### [Workgroup Broadcast #2586](https://github.com/gpuweb/gpuweb/issues/2586)



* JP: Posted comment just before. Discussing, and want to revisit the idea that loads from writable resources will be nonuniform. The only examples we could think where different values coming from that would be data races, and therefore already undefined behaviour. So would change uniformity analysis to rule those loads as not automatically uniform.  And so would need this feature less.
* KG: Aren’t we trying to protect against undef behaviour in the first place?
* RM Looking at the example, with the rules as currently, even if the load you’re doing is uniform, if it’s done in non-uniform *flow*, it’s still non-uniform.
* JP: Exactly, that’s the point of the example.  Only place you would see different values _without a data race _ is when you’d already analyse the example as non-uniform.  So there’s no more reason to add the extra rule.
* KG: Can you have written in a nonuniform way, but now later you’re in uniform control flow. And then get wrong analysis.
* AB:  The index is uniform as well, which makes a difference
* RM: Think agree. Can’t have a case where reading from uniform location, and without data race, see different values.
* MM: So say there is a race, and we allow this program, and program does a derivative. What’s stopping that derivative that isn’t owned by your application.
* AB: Your sandbox has to be resilient against undef behaviour in the shader. It’s not WGSL that can do it.
* MM: As an implementor, don’t see how to strongly guarantee that.
* AB: Then we have a bigger problem. Undef behaviour has lots of ways of occurring.   It’s not just derivative.
* AB: Go think about it. Texture operations should be sandboxed like any other memory operation.
* **Timebox’d, let’s think more.**


### [Name in enable directive can be a keyword or reserved word #2650](https://github.com/gpuweb/gpuweb/pull/2650)



* Fixes [https://github.com/gpuweb/gpuweb/issues/2649](https://github.com/gpuweb/gpuweb/issues/2649) 
* Example: Otherwise you can’t say   `enable f16;`
* **Approved**


### [wgsl: Sketch design for arrays of buffer-bindings · Issue #2105](https://github.com/gpuweb/gpuweb/issues/2105)



* DN: **I think this isn’t pressing for v1**. I was pushing for this before when things were different, but now I don’t think we need it for v1, and that we’d be safe to do this post-v1 if desired.
* MM: +1


---


# ⚖️ Discussions


### [FP16 Extension: Part 1 #2647](https://github.com/gpuweb/gpuweb/pull/2647)



* DN: On our side, we wonder if this should be a branch. Like, how should we add this? Branch to be folded in later?
* AB/KG: Write the PR that adds the feature to the main spec.
* AB: There’s value in writing separate high level doc to describe the feature, for initial review. But we’re past that now.
* DN: The PR can be incomplete, and then get added onto. Wait to land it until the whole feature is described in that PR, or “complete”.
* KG: If a separate doc is useful, then use the separate document.  For this one think this one is past that stage.
* JB: I don’t see a way to write a f16 literal. Deliberate?
* AB: Conversion operators. We waved them off of trying to add suffixes.
* MM: So why have suffixes at all?
* KG: I think we had more of a reason for them historically, though we have less pressure to do so now. Do you want to file an issue?
* MM: I probably should file an issue.
* **KG to write issue for suffix discussion**


### [Should WGSL support Unicode identifiers? #1160](https://github.com/gpuweb/gpuweb/issues/1160)



* [https://github.com/gpuweb/gpuweb/pull/2595](https://github.com/gpuweb/gpuweb/pull/2595)
* Previously:
    * DN: We got the feedback from our Google i18n experts. Came in our afternoon.  We need to review.
* Google to do homework


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, March 15, 11am-noon (America/Los_Angeles)
* Literal type inference
    * DN posted update, PTAL: [https://github.com/gpuweb/gpuweb/pull/2227](https://github.com/gpuweb/gpuweb/pull/2227) 
    * JB posted  [https://github.com/gpuweb/gpuweb/pull/2651](https://github.com/gpuweb/gpuweb/pull/2651) alternative draft, PTAL
* 