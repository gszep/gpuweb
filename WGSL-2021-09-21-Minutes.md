# WGSL 2021-09-21 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** David Neto

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
    * Robin Morisset
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
    * James Price
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
        * [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)


### TPAC meetings



* JG: Expect to hear more soon


### On Todos



* (JG: I will make a spreadsheet for thumbs-up/down/maybe for each todo)
* 


---


# ⏳ Timeboxes


### [PR#2101](https://github.com/gpuweb/gpuweb/pull/2101) Syntactic CI for Grammar



* WAS MERGED
* JG: Added CI. What about playground.
* MOD: Can propose PR to expose a playground as a URL, so readers can try grammar constructs.
* JG: I saw example PRs that broke the grammar.
* MOD: Yes, tried two things:  An invalid example, failed the CI.  And invalid grammar, also failed the CI.  Please notify if there are cases when it doesn’t work as expected.


### [Make atomicCompareExchangeWeak return a structure #2113 ](https://github.com/gpuweb/gpuweb/pull/2113) 



* _Consensus to merge_
* MM: Does this rule need to say what we said for the frexp structure.
* BC: It has a note.
* **TODO: Make issue to expand note to mention *why* you can’t name it**
* 


### [wgsl: support bitfield insertion and extraction #2129 ](https://github.com/gpuweb/gpuweb/issues/2129)



* DN: Came from internal review.  Seems common enough support in underlying languages.
* JG: HLSL would use shifts?
* DN: It’s no worse than what an author would write themselves.
* _Needs spec._
* MM: Would be nice if there were a recipe for developers to follow so they could be reasonably certain that the D3D code will be “good”.


### [wgsl: finding most-significant bit and least-significant bit #2130 ](https://github.com/gpuweb/gpuweb/issues/2130) 



* DN: Similar functionality in all backends. But the clz/findMSB functionality is framed differently. Think we need the functionality, but don’t want to have unproductive fight about it.
* MM: Can see going both ways, but what about the confusion induced in the author.  Which way _should_ I use.
* AB: Don’t need a branch to implement these. Clspv implements both ways.
* DN: Propose adding both styles: the MSL way and the GLSL-ish way.
* MM: Simplicity would be better, but ok.
* _Needs spec._


### [Allow non-overriable constants in texel offsets #2108 ](https://github.com/gpuweb/gpuweb/pull/2108) 



* JG: Didn’t actually have consensus to merge this. Please be more cautious about this kind of thing in the future. Folks presented hesitancy.
* DM: Followed a previous decision.
* JG: Similar but not the same.
* AB: Agree with Jeff here.  If constexpr evolves a certain way, can influence this decision.
* JG: My disposition was that we were going to be cautious.
* MM: Could discuss now.
* JG: Let’s not.
* DM: Can revisit in future.
* JG: Next step is: if I feel strong about this, I will propose reversion.  And let’s be more careful in future.


### [#2055](https://github.com/gpuweb/gpuweb/pull/2055) - Memory model mapping



* AB: Normatively references Vulkan memory model, describing how WGSL concepts maps down into the Vulkan memory model.  Should be relatively straightforward. (accesses, locations, variables/”memory model references”).
* MM: If Robin were here, probably would be happy with it.
* JG: Definitely not expecting to timebox the details.
* JG: Let’s request review from Robin.
* **MM: I’ll get Robin to review.**
* GR: Question about explicitly referencing the Vulkan memory model.  Concerning IP.
* AB:... I am not a lawyer, not providing legal advice.
* JG: Think we want to highlight this to the TAG.
* JG: Do we want a label for TAG review?  Something to indicate for tag review.


### [precise_math attribute on functions #2080](https://github.com/gpuweb/gpuweb/pull/2080)



* MM: Let’s postpone.


### [Error with literal indexing in a shader #1803 ](https://github.com/gpuweb/gpuweb/issues/1803)  



* JG: Suggest not making this a hard error, but can make it hard error later
* MM: Not my recollection. My recollection is what Robin said. [https://github.com/gpuweb/gpuweb/issues/1803#issuecomment-914826793](https://github.com/gpuweb/gpuweb/issues/1803#issuecomment-914826793) (consistent/what-makes-sense)
* JG: think we think differently about what makes sense/consistent.
* DM: Alan had counterargument. Want them written.
* AB: Think the notes from last week captured it.  [https://github.com/gpuweb/gpuweb/issues/1803#issuecomment-920211977](https://github.com/gpuweb/gpuweb/issues/1803#issuecomment-920211977) 
    * Would want to first converge on the breadth/depth of the feature of constexpr.  When must constants be folded.  This issue is the tail of the dog, but the “scope” of constexpr feature is the dog, i.e. more important to consciously decide on that first.


---


# ⚖️ Discussions


### [wgsl: remove ignore, add phony-assignment #2127 ](https://github.com/gpuweb/gpuweb/pull/2127) 



* AB: Concerned about saying “evaluates e”. Happier to say side effects occur but otherwise not necessarily fully evaluated.
* JG: I think all this is “as-if”, so we’re not necessarily signing up to actually evaluate the expression, but rather that it’s as-if evaluated.
* MM: Can add note saying expressions that have no side-effects are expected to take zero time at runtime.
* AB: Ignore had the same issue; trying to avoid sweeping away a binding reference. Not clear it’s guaranteed in the underlying API.
* BC: use case is keeping bindings alive, so reflection can “see” all the intended bindings.
    * Want to reconsider “must use the value-return”. [https://github.com/gpuweb/gpuweb/issues/2115](https://github.com/gpuweb/gpuweb/issues/2115)   That motivates this thing.
    * Don’t think there was as much put into considering that original rule than already spent on arguing over ignore and this phony-assignment.
* JG: I still want this phony assignment even without that must-use-return-value rule.
* JG: It’s modifying how you spell something we already have.
* MM: Can we have all three:  ignore, phony-assignment, and turn make must-use-return-value a warning instead of error.
* JG: Most people would not want ignore if you had phony-assignment.  And follows a ton of other languages, and it opens flexibility for future.
* JG: Let’s talk about must-use-return-value separately.
* JG: would have preferred to see _ as a special purpose identifier.  For someone reading the spec, say “there is also this magic identifier”.
* MM: If we pick one of ignore and _, let’s pick _.
* BC: Agree, let’s pick _.
* **DN: I’ll go off and rework this PR to be framed with _ as special purpose identifier.**
* **_BC to raise an issue to reconsider the must-use-return-value rule._**
* AB: Concern that static use at WGSL may not translate to a static use in underlying API. At what level do you shader reflection: at WGSL or at lower level.
* JG: Reflection must be sure to work at the WGSL level. What that means for underlying implementation is up to the underlying implementation.
* MM: We already have this problem.  E.g. if (false) {foobar..}  can hide foobar
* MM: JG is right about portability.  Around 3 years ago we asked DirectX team at Microsoft and they said any reflection API that doesn’t consider optimizations would be useless.  Applications would often not know what inputs/outputs are in their shader config, then compile the shader, then ask later via reflection.
* JG: Definitely tension here between portability vs. optimization.
* BC: This is tested between Dawn and Tint.  Not sure it’s described correctly in the spec.  
* AB: My understanding of engines is what Myles described.  Engines want the slimmest interface.
* MM: If we do add a reflection API, the first version has to be portable. Maybe there can be a second version that is intentionally non-portable.


### [wgsl: Add limits section, and "spurious" failure #1997 ](https://github.com/gpuweb/gpuweb/pull/1997) 



* DN: There are legit use cases (autotuning) which are not memory exhaustion, and should not result in context loss.
* AB: The underlying APIs got it wrong. They de facto have these kinds of failures.
* MM: CSS deals with this by having a minimum, e.g. structs can nest 32 levels.  WGSL is more expressive than CSS.  Might be something like total size, or number of leaves, etc. Might not know.
* DN: I’m most concerned with hunting for biggest workgroup size that can be compiled. Browser can check all the limits it can compute, and if those checks pass but the underlying compilation still fails, then return this as spurious.
* DM: How would we tell if compilation failed on the host API?
* DN: E.g. vulkan might tell us out-of-host-memory, but we’re pretty sure it’s not, so we can say “ah, this is probably a compilation failure”
* DM: What if the driver really does run out of host memory?
* JG: Spurious spurious error, which is probably fine
* MM: So you want a new error return kind in the API side. “Spurious” or the like.
* DN: Yes.
* DM: Can the browser do any better than the application itself.?
* MM: Yes. The browser has more detail about the memory limits of the underlying machine. Vs. the insulated view presented to the page application.
* MM: Hard to test.
* DN: Think you can test the workgroup size.  4B workgroup size, with a barrier, will fail most/all shader compilations. (I expect.)


---


# 📆 Next Meeting Agenda



* Next meeting: 2021-09-28 (non-APAC 11am time)
* [PR#2128](https://github.com/gpuweb/gpuweb/pull/2128) Enable Assume Explicit For.  **MERGED (bikeshed/editorial only)**