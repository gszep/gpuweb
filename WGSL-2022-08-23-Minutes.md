# WGSL 2022-08-23 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** ds

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-08-16 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1IYqOBVGGncsexKQ5AAB4IGivRgkx1w5dSwIYjQ3wUew) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Daniel Glastonbury
    * **Myles C. Maxfield**
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
    * James Price
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
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * Rafael Cintron
    * Tex Riddell
* Mozilla
    * **Jim Blandy**
    * **Kelsey Gilbert**
* Connecting Matrix
    * Muhammad Abeer
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
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
* **Mehmet Oguz Derin**
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
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


### FYIs and Notable Offline Merges



* DN: Not merged, but [PR 3292](https://github.com/gpuweb/gpuweb/pull/3292), tooling to refactor grammar for recursive descent. Non-normative section to layout grammar rules. Take look for any objections but think it’s a net win.


---


# ⏳ Timeboxes (until 11:15)


### [add AbstractFloat overloads for modf and frexp? #3358](https://github.com/gpuweb/gpuweb/issues/3358)



* KG: Called out weird because return structs and don’t have structs with abstract floats
* DN: Only usable if extracted one of the components by name and used that in const-eval expression. Generality issue.
* AB: What happens if result of frexp is assigned to let but it’s an abstract. Does it keep abstract and resolve to f32 at let? Where does conversion occur?
* BC: Same happens with abstract array assigned to let. The assignment does a materialization. Would assume struct with abstract would decay to natural, so f32 if put in let, if in const still abstract
* KG: SO struct of abstract?
* AB: Which user can’t do, but not an issue, already can’t declare these structs
* KG: Ok.
* DN: To handle conversion would need to put entry in conversion rank table for decay path. User can’t declare so only case is builtin, so can just spell out type returned from blah decays.
* BC: Don’t think spec forbids abstract in struct, you just can’t type them.
* KG: Which is fine.
* BC: Don’t see reason why not have abstract version
* KG: Sounds like everyone agrees on complexity but we should probably just do it and come back if it breaks.
* MM: Don’t feel bad about this
* KG: **Resolution, have struct with abstract float and rules to materialize**.
* MM: Sounds good.


### [Accuracy of `mix` does not cover all common implementations of linear blend · Issue #3260](https://github.com/gpuweb/gpuweb/issues/3260)



* KG: Feels like answer is be as lax as subsystems are
* AB: Basically covered in other issue last week, it’s editorial and we update language on inherited from
* KG: **Resolved to editorial**
* GR: So, mix is one or the other? \
AB: Yes. Basically distribution and refactorizations are correct.
* GR: Works for us.
* JB: Do the answer in WGSL and Vulkan have different accuracy?
* KG: Yes, denormal for example
* DN: Catastrophic cancellation happens in different spots.


---


# ⚖️ Discussions


### [invalid expressions in const-expr and override-expr should be error (no later than pipeline creation time) #3253](https://github.com/gpuweb/gpuweb/issues/3253)



* KG: Talked about domains of builtins. Feels like we’ve strayed from path. Agree we have to solve these things, worry we’re dealing with what we need to solve and not the level were solving it. Do we agree there should be errors which cause compile errors at const-expr time
* JB: Thumbs up
* DN: Yes
* AB: Don’t know if we all agree
* MM: What is these
* DN: Think we agreed out of bounds index is compile time error. Disagree on domain errors on Floating Point.
* MM: Last week requested splitting this into runtime and compile time and discuss runtime first.
* JB: Agree with MM that sorting out runtime behaviour si fine as let’s settle and make sure other is coherent. What if spec said result is NaN or 0 and that way people don’t depend on it as it maybe nan on supporting platforms. Have upgrade path when GPUs support NaNs and we don’t have unspecified values. All it means is the platforms that can’t support nan have to do some kind of check. I propose this but first object is sqrt is too important and fear for perf reasons we have to accept platform sqrt. Don’t know how to make that call.
* JB: Would fixing alternative to Nan at a value change the way apple feels?
* MM: Would be better then if we did not modify the spec at all. Think that would be acceptable in the spirt of compromise.
* JB: No-one cares about anything but sqrt.
* MM: List of a few of these and it might make sense to split into inverse trig from others. Inverse trig are going to be slow and clamp is fine
* JB: log?
* KG: Want that to be fast
* JB: Really?
* MM: Can probably make resolution about inverse trig functions
* DN: So what JB proposed aligns with what I’m allergic too. Having folks getting used to math working ways it doesn’t. Having that Nan is a good warning. It’s a marker that if you get slow path then you have underspecified GPU and should get a faster one as it’ll be slow
* AB: Doesn't’ seem like market for us as the person suffering has any idea.
* MM: Want to try to understand proposal
    * Premise 1: There is no implementation of vulkan that can handle nans correctly?
        * DN: False
    * Premise 2: Knowing if impl supports nan is extension?
        * DN: True
    * Premise 3: any impl that doesn’t have ext gets slow path, ext is not opt-in would be used. COmpiling making spriv would give Nan when enabled?
* AB: Think DN is wrong about prevalence of vulkan support. Some instructions with core features where you get nan preservation but not applied widely to all instructions
* JB: So just broad absence of support for Nan
* AB: In the spec
* MM: To restate, it is possible for vulkan to say instruction to make nan but not possible to say “and the nan flows into rest of program via IEEE”
* AB: Sort of, more finely grained. Has feature to preserve nans but instructions that feature applies too is not all math instructions.
* JB: So exist instructions where no way to know fi they support nan
* AB: Correct and those are usually GLSL extended math expressions. Can guarantee for core SPIRV instructions like +, *. Other extensions didn't’ get spec, so it’s a hole. Probably not tested any maybe taken advantage of
* JB: So you think they may have skipped nan handling
* AB: If it’s faster, yes.
* KG: Something last week, people said want to do better then WebGL
* MM: WebGL doesn't’ spec out of bounds and I said we should do better then that.
* KG: Need something actionable. What is the thing we’re worried about and scope to prevent. Don’t feel like we have a good handle
* MM: Worried about portability. 
* KG: Means something to you but possibly different to me.
* JB: Program runs same on all browsers? \
KG: Same *. Have big *. How big should * be before v1
* DN: Don’t care about programs misusing math  as being portable or programs misusing indexing
* KG: Think nan-mumbler which takes in nan and spits out elsewhere, Don’t care about preserving that. But care about math being fast.
* MM: So think it’s reasonable to discuss instructions separate from each other
* KG: On webgl side haven’t seen this come up. Believe portability issues are true in theory and we could make test cases but isn’t something we run into in webgl
* JB: Interesting data point. When being absolutist on portability the motivation is what is the experience of folks using. Pragmatic. No principled objection to leaving holes just will it fly and folks be happy. If it’s the case where no-one in webgl has struggled then that is counter to what I was expecting. Weakens my will to struggle about it.
* KG: There are collaborative market forces. Systemic forces are authors want code to run on users machine and we want that too. Make reasonable choices for authors and it works out. Even for portability issue where someone messes up math they didn't’ realize. That’s why I think 1 important question is if we do put our head in the sand, how much can we do that and have it no be dangerous. Can’t tolerate dangerous if we have Undefined Values that cause Undefined Behaviour, can’t have that. But, also the two options are implementation takes on complexity and slowness to make sure undefined values become defined or other option is to do something more structable in spec and forbid them.
* JB: Rewind, what is blocking nan or zero compromise aside from nan not happening on most platforms. Was something I was expecting in future but less confident it will be a thing but could still keep that and still has value in spec as a threat. Sounds like nan or 0 is best option and something folks are ok with
* MM: Yea, could make the spec say this operation will return a nan. If you pass the results into isnan it would return true. What we can’t do is go further then that and say if you flow it through other methods it will be nan
* AB: We removed isnan because we couldn't guarantee them
* MM: Because there are no nans
* JB: Chance the omission in GLSL extension could get filled in?
* AB: Possible to fix in extension but not core spec as a bug fix.
* MM: Instead of emitting those instructs, could emit more instructions
* AB: Don't’ want to emit taylor expansion of our own.
* JB: Those methods are …. Complicated.
* KG: Implementations of complicated functions in terms of complicated functions. Taylor expansion is core common thing
* JB: Do a lot of stuff to make it palatable
* KG: And to make fast. Lot of opcodes in SPIR-V.
* AB: Think KG is persuasive in that this hasn’t been a problem before. Feel this is state of the art for graphics and is kinda expected. Not sure if  we have to work around and define the behaviour and say we’ll give a value instead of nan.
* MM: Maybe mis-understood, when said emit instruction or block of code, didn’t mean emit polyfil, was saying emit clamp and then instruction. Don’t think we should emit our own cosine function.
* KG: If I was the decider of should we fix this before v1, I would vote no. Want to know what we need to fix before v1 release. Agree it would be nice to do better then this to see and experiment and if we could offer on-par performance with better portability guarantees but don’t want to worry about it for v1. Even if could be a cool post-v1 project. What are the v1 requirements.
* JB: Just math or entire issue?
* KG: Entire thing
* JB: Have consensus on indexing.
* KG: Don’t want to unwind consensus. It’s great, but I don't’ really care which way it goes. We should try to wind down our active development. We can make this better, we’d be approximating perfection.
* BC: We could go and wrap the math functions to be more portable but they are the foundation of what most folks will be doing. Impacting perf for portability will likely upset vast majority of folks who don’t care about extremities of functions. Will be some folks who do care. Problematic drivers and hardware should get better. Say we should leave as are and have extension for better math library with full representation for inf and nan. An extension for devices which do support it.
* KG: Few more thoughts then table? ……
* DS: **Can someone split the issue**, we’ve mentioned a few times
* **MM: I’ll do that**.
* 


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* AB: Hope people could look at [#3336](https://github.com/gpuweb/gpuweb/pull/3336) which is what I’m proposing.
* JB: Have not.
* KG: Table for folks looking at PR.
* AB: Not final language but should be explanatory enough.
* KG: **AI for folks to look at 3336 for next meeting**.


### [Accessing an "invalid memory reference": Trapping behavior · Issue #3272](https://github.com/gpuweb/gpuweb/issues/3272)



* DN: wanted to visit one thing from last week. Feel like flipping side saying why have to expand behaviours and why can’t JS compiler trick work with fast/slow path.
* MM: Don’t want to double code size. For JS it’s fine, for us i don’t think so
* KG: Originally worried it wouldn’t be doubling but a doubling per instance. Could see how one true fast path through program. Rest goes through slow path. So as good as only doubling. Hopefully. Until you load from something. Also one thing that to me could be good to get implementation feedback. At this point we’re doing pre-flight design review and should get eyes on the ground if anyone has appetite to implement trapping.
* MM: So we’d be doing that and we aren’t near that point in our compiler. Want to be up front that if it turns out at least one compiler investigates this and after that happens all compilers decide this is not a good idea then OK to take out. Can’t add in later and has to go in now. So, add, do investigation and then take out if not useful.
* BC: Could prototype without full webgpu stack. To know performance don’t need webgpu. If want to test with webgpu could add a tint transform.
* MM: Unreasonable for us to add transform to tint.
* JB: Have in mind some what c-language had language for trapping and very few c implementations ever did this. Few special validating compilers that are not widely used. Spec has language to allow those complers to be conformat. Didn’t really get in anyone's way. Is that what you’re trying to do? Here is a possibility that is allowed and maybe no-one does it?
* MM: Not trying to build a science project. If no-one uses it it shouldn’t be in spec
* JB: Still feel uncertain about implementing this.
* KG: Question is do you have to be. Our implementation would be fine never touching this
* JB: Is anyone going to do it?
* KG: Proposal is Apple could choose to if we allow. They can not add later. If we find out they try to add it and it isn't’ faster we can, with good confidence, remove it.
* MM: One of failure criteria is, not faster, other is vertex positions at the world origin and have huge narrow triangles. If that happens in practice it breaks real things. Thats sufficient to say this is a bad idea but can’t make claim until we get there.
* AB: Goes into what we discussed last week. Way to get feedback to user trap occurred. Thought we’d talked about that being investigated. Whole spectrum of how much info relayed back. Debug mode could be more then non-debug. But in actual runtime something to check why have blackscreen or other strange thing that’s hard to trap down. A signal you need to dig into something when you wouldn’t otherwise.
* MM: A few comments, 1) the hard part about the request is getting data out, for metal is easy. Tones of space going in and out. For vulkan that’s harder. At least in exernal_texture that’s a sticking point. Passing extra data is hard from bindings perspective. 2) if recording into web inspector doen’t need new api. For our implementation, 95% sure we’d report these errors in web inspector if they occurred 3). If we wanted in webgpu api, we have a mechanism of error scopes so we could say if trapping happens an error will be reporting in normal mechanism. No opinion. If you want it’s fine, fi group doesn’t that’s fine.
* AB: Refine what I said, if api had an error as result of draw call or dispatch you pop error and say out of bounds, think that’s fine for productionAPI. Much less scared to say it’s a possible behaviour. As know it happens. Difficulty on vulkan isn’t as scary. Anything in devtools giving more info is just beneficial. If the api side has new type of error that is satisfying.
* MM: Already have situation where shader takes too long and se wstopped it. Early termination already happens and we can report the same way.
* KG: These are also cool avenues to investigate. Not necessarily blocking nor required.
* MM: All of this doesn’t require new API. Uses existing API.
* KG: Don’t think it’s reasonable to require trapping visibility into trap if clamping implementations don’t require clamp visibility.
* AB: Feels slightly different as it’s non-local effect. If impl that does trapping also thinks it’s easy to get info, why not just add to error code if trap occurs? Don’t think it’s sounds hard?
* KG: See value in exploring trapping as optimization opportunity. Don’t think trapping behaviour, even with non-locality, don't’ think it should be a requirement that it specifically serves to the user. That’s more then we need to ask. But if everyone else agrees that’s fine.
* MM: Question comes down to, I plan, when we implement this to do what AB is asking for. So, remaining question is should the spec say if trap occurs must do what AB asked for. Sounds like KG wants that to be no.
* KG: Also very little vote in the matter. Doesn’t seem like right thing to ask. In particular don’t want that to be weird sticking point later where trapping was good idea but reporting error was the problem. Maybe need to make change that we don’t get indication back at runtime. Don’t overpromise but if you want to, that’s fine.
* BC: Looking through proposal, correct me if i’m wrong you either super return, which is conditional heavy, every function call needs to check maks or you discard and, as far as I”m aware, discard has perf penalties on certain architectures as you don’t get fragment write which effects depth writing. Both avenues would be surprised if perf win over branchless select.
* MM: The first part about super return is correct. Second part isn’t. The outputs still get written. So trap in fragment does not get discard
* BC: How to avoid all vertices at 0,0,0,0?
* MM: You wouldn’t it’s an author problem. Out of bounds access, you’re fault.
* BC: Do super return for discard transform now
* MM: Right because OpKill
* BC: Not happy but user has opted in. User used discard so they get perf hit. Having any memory access taking hit would be something we avoid.
* MM: Right, came up previously and response was that’s why it’s an option. Impl only has to do for memory access it makes sense. Deeply nested functions and inner most access wouldn't get this behaviour as you don’t want to pass boolean back
* BC: Still need to check before write
* MM: Downstream from trap, yes.
* BC: So no potential optimize conditional check for out fo bounds write
* MM: In the middle of series, right
* BC: For those reasons, would imagine Google wouldn’t implement if it was in the spec
* KG: Right, think we have 2 groups with separate implementations and 1 thinks it’s cool and we should lt them do the thing unless we have concerns on how it effects others.
* AB: What does spec language look like for trap. Describe options for out of bounds think it means all spec says some writes may no longer occur. Once it’s trapped but you don’t know which will trap. Shader outputs will not be written unless already written?
* MM: No
* AB: Want to understand spec langue and how it combines with other behaviour
* MM: Think spec would say if an access is out of bounds a trap can occur. Trap means that all shader outputs are filled with zeros and memory operations that are observable after the trap do not occur. Then leave to the implementation to make appear to be true.
* AB: With uniformity is it observable on load?
* MM: Uniformity not effected
* AB: You mean uniform control flow. Reading from a uniform buffer still needs to load
* MM: Don’t understand
* AB: If i load i have uniformity, if you don’t load don’t have uniformity, still have to maintain uniformity
* MM Is that different from discard?
* AB: Just writes that don’t occur. Reads may or may not?
* KG: Operates as if  which is where MM said observable.
* MM :Yup
* KG: Sounds like talking bout spec, can we get a proposal?
* **MM: Yea, next step is PR**.


---


# 📆 Next Meeting Agenda Request



* Next week: Tuesday, August 30, **11am-noon** (America/Los_Angeles)
    * KG out-of-office, CW to host.
* 