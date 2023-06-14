# WGSL 2023-06-13 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Outstanding V1 Ixssues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-06-06 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1e3Loe6Axs-iPLxKVC3EZQNTXYF4KP8rbFCx8lvMCDiY) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
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
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * **James Price**
    * Rahul Garg
    * **Ryan Harrison**
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
* **Mehmet Oguz Derin**
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


### Process, Milestones, and forward progress vs spec change moratorium



* KG:  (In part): the softness in decisions comes from the fact that none of us are spending the engineering effort doing due diligence on the items.
* DN: Hopefully we can consensus/agree that we can discuss general direction for things, and make *provisional* resolutions, which I think are valuable.  Separately I’d like us to be mindful to have a consistent-ish bar about what is worth talking about now.  (Think prioritization of dp4 vs. the indirect structs discussion was inverted.)
* AB: If it’s “provisional” then suggest keeping discussion to the issue. Rather than having a sync meeting, especially if we end up backsliding later.  It all takes time. (KG +1)
* MM: Q: Why would we come to consensus but not make a PR for it.
* KG: E.g. at Mozilla while we freeze a release (and its major features), we still do a lot of preliminary development work that doesn’t actually land.  Don’t want to pause all development until it can actually land.
* MM: Agree can make progress and not land PRs.  In a world where decisions will be revisited later, would make sense to cancel meetings in the first place.
* KG: Push back on that. Don’t expect to relitigate things we decide in meetings when we get the PR.  That’s an uncommon case to reverse that PR.  We can do that today even with landed things; but it’s definitely the exception.
* KG: Concretely propose currently on the table is deref and extensions discussions. After that, consider canceling. But check the “open issues” links at the top for v1 and untriaged.  My guiding principle: if there’s not enough in the “v1” milestone on GitHub, then no meeting.  Hope/think that’s in line with what folks hope for here.  Maybe stretch that for “v1.1” depending how you label versions.
* KG: Expect few meetings in the next 2 months.
* MOD: Will the frequency revert after this period to provide room for people to discuss extensions and various proposals?
* KG: Yes.
* MM: Is this about both WebGPU and WGSL meetings?
* KG: This discussion is scoped to WGSL.  **I’ll sync with Corentin**.


---


# ⏳ Timeboxes (until XX:15)


### [Feature request: textureQueryLod equivalent #4180](https://github.com/gpuweb/gpuweb/issues/4180)



* KG: **Sounds good but not right now.**
* AB: Question about unclamped functionality in SPIR-V. Might not be the same thing (as assumed/stated in OP).
* DN: If it’s common functionality then let’s put it in.    Needs investigation.
* MM: Polyfill? Take the derivatives yourself.
* KG: Would be sensitive to sampler state.
* BC: Drivers can put in custom biases that are impossible to detect.


---


# ⚖️ Discussions


### [Add dereference operator · Issue #4114](https://github.com/gpuweb/gpuweb/issues/4114)



* AB: Examples 4 and 5 look the same to me.  Also, there’s nothing with array acceses; which is what I think of for “dynamic” for me.
* KG: Solutions for 4 proposal 3 had typo. 
* KG: I expected proposal 2 to be recursive. But it is not, per Go rules.  
* MM: With null proposal, are there only 4.
* DN: Think any extra would be in no existing language. Let’s not go there.
* MM: So with 4 proposals.  Think it’s mostly style.  Our main feeling is option 3 is worse.
* JB: Want option 2.
* BC: Want option 2.
* DN: Symmetry option also tells you what to do for p[i].   Can extend option2 to be similar: p[i] would yield reference.
* DN: I’m ok with option 2.  Improves over 3 because 3 has the problem of saying * on one end of expression and . on the other end. Makes it harder to read.
* JB: Pascal solved this by having post-fix dereference.
* KG: Most common thing you want to do with this is to make alias/shorthands. 
* MM: Common case is to make an alias to a field of a resource. Will need to write & anyway.
* KG: Right have to start 
* **KG: Consensus for option 2.**  Despite the coolness of the symmetry of 3.
* AB: Have slight preference to 1 because of familiarity.  But not a big deal.
* **KG: Consensus for option 2 with additional sugar for array indices, and Go-style of only dereference on level right now. (We don’t have ptr-to-ptr right now so the last point makes no difference)**


### [Shader extensions are redundant · Issue #3961](https://github.com/gpuweb/gpuweb/issues/3961)



* DN: The new thing in Kelseys’ write up was the shadertoy environment where the env flips on all the features. Then noodling around with shaders and developer accidentally falls into too-large-a-feature-footprint, and get accidental nonportability.  I agree that’s important to have the speed bump at *that* point in the development flow.
* KG: Think it’s onerous to add options to that shadertoy thing to create device with lower option-set.
* KG: Think people want an escape hatch, similar to ‘using std;’ in C++.  It’s “just give me everything, I know what my device supports; give me that”.  
* MM: Re: shadertoy example. The way we see it, developers of shadertoy want their users to have to enumerate features, or not. If they want the users to have to enumerate optional features, they could require the author write #require in their source code, or checkboxes, or whatever. If they wanted the author to not have to enumerate all features, they can massage the source; the shader source goes through the shadertoy framework already. So we didn’t think the shadertoy example is compelling.
* KG: Depends on shadertoy being ok with recreating devices in a lightweight way.
* MM: Game engine doesn’t allow user-written shaders.
* KG: That does exist.   … REPL-style development.  In the solution space is “import all”.  Another solution is the … (?).  
* MW: Device recreation can be made less costly, so let’s not take that into consideration. Could optimize for that, e.g. caching.
* KG: Reality of games if they have gigabytes of resources that have to be reloaded, which costs to get “back” to the GPU. Can’t rely on implementations making them light in that use case.
* MM: Core of our concern is understanding there’s set of people where having to list extensions is typical. Willing to believe there’s a set of people where it’s beneficial. Want to hear from Microsoft folks if they have requests for explicit listing of extensions.
* RC: What’s the question.
* MM: When one compiles HLSL shaders, three’s  a bunch of flags that you passs the compiler and you have to match the source code.  Have you had requests from authors to specify those flags in some form within the text of the shader rather than out of band.
* RC: That’s more of a question for the HLSL team.  WebGPU is different from regular Direct3D. At shader compilation time, you don’t have to recreate all your resources from scratch.  It’s not like you have to create the device with all potential extensions.  Doesn’t exist in D3D; but does exist in WebGPU.
* RC: You create device, then you ask the device what the max feature set you can use in the shaders.
* MM: Right WebGPU is created with only requested devices.
* RC: It’s more than feature level. There’s tiers in each feature level. 
* MM: So the logic there is, author never wants explicitly a lower tier. If the device can support a higher tier you should just offer it; there’s no cost.
* MM: Kind of similar with f16. If f16 is possible in the HW then the device should just offer it.
* KG: Feel WebGL was successful in portability in forcing dev to being super explicit about requesting extensions.
* RC: WebGL, you can ask for features after context creation.
* BC: Unity’s HLSL has #pragma require and various other things that map to the concept. It’s clearly there to serve the needs.  [https://docs.unity3d.com/Manual/SL-ShaderCompileTargets.html](https://docs.unity3d.com/Manual/SL-ShaderCompileTargets.html) 
* MM: At the top it says “By default, Unity compiles shaders with `#pragma require derivatives`, which corresponds to `#pragma target 2.5`.
* 
* MM:   Could show a way forward:   Partition into opt-in features vs. opt-out features.  Think f16 is candidate for (one of them)
* AB: Unity has done market research into what is available where.  They’ve done the work of something like “90% of devices have BLAH”.  I’m happy to entertain going for a “assume ok” if it’s that high. But we’re nowhere near that.
* BC: Their default “2.5” is DX11, GLES 2.0.  It’s our baseline. It’s WGSL 1 is. There are multiple targets after 2.5 which would … Their default is very very old.
* MM: We have more to say; didn’t get the time.   **Let’s be open to meeting again next week**.

DN after the meeting:



* _Unity 2021.3 _says 
    * By default, Unity compiles shaders with `#pragma require derivatives`, which corresponds to `#pragma target 2.5`.
* But going to older versions has a different default  Unity 5.3 says in [https://docs.unity3d.com/530/Documentation/Manual/SL-ShaderCompileTargets.html](https://docs.unity3d.com/530/Documentation/Manual/SL-ShaderCompileTargets.html) :
    * By default, Unity compiles shaders into lowest supported target; roughly **shader model 2.0 **equivalent. Some other compilation directives make the shader automatically be compiled into a higher target:
1. Using a geometry shader (`#pragma geometry`) set compilation target to `4.0`.
2. Using tessellation shaders (`#pragma hull` or `#pragma domain`) sets compilation target to `gl4.1`.
* So the default baseline shifts over time, which is not great for the web.
* KG: It shifts forward though? Because adding implied includes later should be *mostly* safe


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday June 20, 2023, **11am-noon **Americas/Los_Angeles
    * Might skip depending on progress on in-github progress on #3961.
* 

 
