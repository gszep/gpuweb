# WGSL 2022-09-13 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN, KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-09-06 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1_LaIeP5V5cXn792TmwsBGqymioTtUw8ltdm4zF9YHA4) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Myles C. Maxfield**
* Google
    * **Alan Baker**
    * **Antonio Maiorano**
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * dan sinclair
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
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * Jim Blandy
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


### TPAC



* TPAC is this week. We are not meeting face-to-face, but rather online on our usual schedule. (No special plans for TPAC)


### WG Charter



* **Please Review: [https://github.com/gpuweb/admin/pull/18](https://github.com/gpuweb/admin/pull/18) **
* KG: We received a 3mo extension, to give us a chance to formally renew our WG charter.
* KG: If we’re happy with the shape of it, we’ll continue the formal renewal process.
* DN: **Will review in the next 2 days**.
* MM: Apple is aware, no feedback right now.


### CG Charter



* KG: **Our CG has been operating under a draft charter. This is okay, if we are all okay with it.** A formally adopter charter is less essential for a CG than a WG.
* KG: However as we renew our WG charter, this is a good time to check with y’all if we’re happy to continue the status quo. If we want to have something stronger than a draft charter for the CG, we can get that process going too.
* MOD: How are proposals handled.  In the repo, or another channel. E.g. shader language is based on….
* KG: Francois would know.
* MOD: Ok,** I’ll post a PR for the CG charter**.


---


# ⏳ Timeboxes (until 5:15)


### [Clarification needed with regards to flushing denorm f16 value #3421](https://github.com/gpuweb/gpuweb/issues/3421)



* DN: There’s a gap in the f32->f16 case, it’s not clear if it should permitted to flush. We think it should be permitted to flush though just like e.g. f32->f32.  Applies to both pack function and quantizeToF16.
* MM: On devices we target for webgpu, does the hardware tell us if we will flush, or is this operation by operation
* DN: Op by op. Most cases you’ll probably see it, a la fastmath. It’s never tested by “I have a global flag and will always flush (or not)”. 
* **Resolved: Flush in builtins**


### [Accuracy of acosh is does not include Vulkan CTS alternative #3287](https://github.com/gpuweb/gpuweb/issues/3287)



* KG: Just widening the precision allowances?
* DN: Yes, we did this with `mix`.
* **Resolved: Someone make a PR**


### [Add texture sample function which clamps coordinates #3403](https://github.com/gpuweb/gpuweb/pull/3403)



* **Resolved: Accept**


---


# ⚖️ Discussions


### [wgsl: ban ptr-to-workgroup params to user-defined functions #3422](https://github.com/gpuweb/gpuweb/pull/3422) 



* MM: I remember a leaning, but not a resolution to this. Is this really resolved? I wrote a new comment to summarize things.
* …
* MM: Many people including TR said that ptr-to-wg is broken, but I don’t really agree it’s a broken feature. Rather, it’s broken if there’s a race, but that’s what the spec says?
* DN: Nobody intentionally says “please make a data race”.  They use it wrong.
* KG: I suppose they *are* doing the wrong thing, but users so often get it wrong that some consider the feature to be broken in practice/common use.
* TR: They use it wrong in a way that often just works out, but it’s not guaranteed to happen, and that’s a problem
* JB: We’re fronting this feature to a user as something that looks like it’s going to work.
* TR: You’d have to inline to make it work as the programmer wanted.
* MM: The point Jim made is the one I made a week ago: if we take a program that uses the word “pointer” ,that has a particular meaning. If we then compile that to HLSL as inout, then we introduce the data race.
* TR: People weren’t wanting to introduce the inlining to fix it.
* JB: Yes.
* DN: Yes.
* MM: Think that restricting the expressivity of pointers so much is the wrong direction.
* TR: It’s easier to relax restrictions later.
* GR: Your final two proposals are either disallow ptrs entirely, or add an inout.
* MM: Those are on a spectrum.  And strictly speaking, orthogonal.
* AB: We reviewed your proposal.  When you describe “ptr-to-workgroup”, and its manifestation in HLSL, you’re assuming more than is actually there.  It doesn’t work for the programmer’s intent.  The actual functionality that is usable is data that is private to an invocation, so “private” and “function”.  So that matches quite well with reliable behaviour in other shader languages.  So we don’t see it as a great restriction in functionality, while you see it as a great reduction.
* AB: The confusion due to aliasing is the same programmer problem whether you express it as inout or as ptr-param.  You might define the semantics strictly in the spec, but it acts as a gotcha for the user.  Jim’s analysis is a protection for either method of encoding the feature.  
* MM: We get to define WGSL.  So if we want to make a feature that is copy-in-copy-out we can name it however we want.  Maybe don’t call it ‘inout’.  So we get to say if the semantics is like ptrs or like copy-in-copy-out.  You made the point that it matches existing programs. E.g. in GLSL you can inout any variable, regardless of address space. Same in MSL and HLSL.    If we could rely on  ptrs, it would be intuitive because of experience with CPU code that uses ptrs.  In a previous call I may recall that Greg mentioned that copy-in-copy-out order defacto exists. That order is depended on by real programs.
* GR: Don’t know that people depend on the order.  Wouldn’t bet on it either way.
* KG: Hyram’s law.
* AB: You mention inout on a buffer. We get to the programmer assumption being wrong about how it works. If it truly were copy-in-copy-out then it goes against what the programmer wanted.  In our view it’s not a useful feature at that point.  So you are overstating the amount of restriction.
* AB: Also, currently the spec is very clear about what is mutable, because stores can occur.  Introducing inout causes a big change in how we frame what is mutable, would cause great disruption to the mental model of how things work. And in future if we pass ptrs into these inout then there’s a complex interaction.
* MM: Don’t feel very strongly about adding inout as a language feature.  Would rather have it, but it’s fairly low in priority of desires. Much higher is relaxing the restrictions on pointers, that are being proposed.
* JB: When I evaluate this feature, I have in my mind is what is the alternative for the user having to write, if we don't provide this feature. Ptrs are a convenience, but there is always a way to get around it.  The code looks straightforward and the behaviour is clearer.  Given the alternative code that people would have to write isn’t that bad, and we expect ptrs in future, I’m in favour of dropping the ptr-to-workspace arguments.  
* MM: I thought you were going to conclude to remove ptrs altogether from params.
* JB: It would go that far.  But if there are cases we can support, then we should support that.  The stuff we do provide should be well-behaved.  Don’t provide anything that is not well behaved.  We expect this to be expanded in the future in a sensible way that extends cleanly over what we start with.
* MM: Internally, we concluded a surprising thing (to me at first). We feel the partial support (relative to in-out params in current languages) are worse than no parameters than not accepting any params as params to functions.  First, intuitiveness. They’ll try one address space, then another and will get confused.  Second, people will come from other inout languages, and WGSL has less functionality than those. So it’s better to wait for a full-featured feature thing in the soon future. E.g. augment to return to struct.
* DN: When you say your second point, that it’s strictly less than inout currently in GLSL, tc.  That is a bad way to think  of it because we are shaving off the part of the feature that exactly is confusing to users, andi s a misfeature.
* JB: The mut part in Rust is confusing to users right now.  WGSL will get a “pass” from users because they understand that GPUs are special and restricted/limited.  Don’t think anybody is going to be surprised by limitations in ptrs.  In first revelation you say “there’s ptrs”, then they learn the restrictions. If we offer good diagnostics they will get used to it.
* AB: Maybe you consider it confusing to have these restrictions because coming from C++ with one address space.  We’re very explicit about different address spaces. Understanding the hierarchy of memory in GPU is critical to starting GPU programming at all and getting utility out of it.  Even with the facility for private/function address space, you can still have the user write the emulation bit by copying from buffer resource into function var, then pass ptr param, then copy back out. There remains utility to having the ptr param only for limited address spaces.
* MM: So hypothetically, if we added new lang feature, you’re urging that this inout feature should be guarded by the static analysis to prevent the tripping hazard people keep hitting in hlsl today.
* MM: For the programs people write that are wrong (e.g. inout with a whole array and then write to a single cell) such a function can be rewritten to be correct instead. So I understand that it’s possible and even easy to write a bad program, but I’m worried about people [being able to?] write correct programs from hlsl.
* AB: If we keep the ptr-to-private/function feature around, we get utility. We don’t need an extra language feature to handle those few cases.
* MM: You described a program with local var, copy from local var, pass ptr to that var, then copy-back. That’s similar to what you’d need to do if we don’t have pointers at all. So I don’t see ptrs as better than no-ptrs in that case.
* MM: Specifically the augmentation of code you’d need to write to polyfill inout with ptrs.
* JB: BJ has written a bunch of [lang] and it’d be useful to see what WGSL code would actually look like here.
* DN: We need this kind of thing anyway.
* DN: Second, [...] need to do an awful lot of hoop jumping to do [...] Where you have these function calls matters a whole lot for how much work people need to do. 
* MM: As an aside, FWIW I do think the comma operator would help here.
* MM: JB made a point about how Rust moved the state of art here forward.
* JB: Rust eliminated dangling pointers, which GWSL has already done too. The other thing is that it cleaned up a bunch of re-entrancy issues. E.g. in java, you’re used to [...]. WGSL doesn’t have that though, so we don’t need analysis for that kind of code. Rust has made it a lot clearer who owns data, e.g. I’m gonna lend this data to you temporarily, but you’re forbidden from doing something bad like storing it. Some stuff here applies to wgsl, some stuff doesn't. Mutable vs immut in rust probably doesn’t apply so much to wgsl. Clarification of ownership is something that would carry over to WGSL. E.g. utility function is just doing something well contained, and not doing anything more dangerous. But in C++, you see a pointer and your vigilance has to go up. Different when you can relax more.
* MM: Does Rust do alias analysis, static?
* JB: Yes, and that’s what freaks people out. It’s one of the two main learning curves: Generics and Borrow Checker (which does the analysis)
* GR: Meta: As long as we’re talking Charters, this discussion has been going on for some time. I think all proposals would satisfy the needs here. What we don’t want to do is regress to just a sort of war of attrition between proposals.
* MM: Standards work by consensus, but I would describe your “war of attrition” as “gathering consensus”.
* GR: Is “consensus” need to be universal?
* KG: Universal consent, not universal “this is best”.
* DN: …
* JB: I understand being impatient also. As a data point, I am not impatient for this yet.
* AB: One concern wrt MM: I don’t think we’re saying that pointers don’t operate “normally”. Rather, how much of this fundamental thing that we agreed on should we expose?
* MM: Aim of the text at the end of the comment on the issue is to partition into acceptable solutions vs. unacceptable solutions.
* JB: If Apple is only weakly in favour of inout, and Google and Mozilla are firmly against inout, then can we eliminate inout?
* GR: Adding inout could soften the blow of other ptr-related decisions
* KG: **Let’s try to center on fewer options and work from there.**


### [Add a static alias analysis #3336](https://github.com/gpuweb/gpuweb/pull/3336) 



* (previously [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457))
* (out of time, tabled)


---


# 📆 Next Meeting Agenda Requests



* Next week: Tuesday, September 20, **11am-noon** (America/Los_Angeles)
* 