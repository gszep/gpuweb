# WGSL 2022-11-22 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** (APAC) Tuesday **4-5pm **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-11-15 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1Y939pY9V5FJ94IhWhawzCatvjUyd_M1emfHOnvqQxp4) 

 

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
    * Kai Ninomiya
    * **James Price**
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



* FYI: New PR: [Introduce Operator Precedence and Associativity #3609](https://github.com/gpuweb/gpuweb/pull/3609) 
* 


---


# ⏳ Timeboxes (until 4:15)


### [wgsl: add way to get special values for a given numeric type (e.g. max, min, lowest) · Issue #3431](https://github.com/gpuweb/gpuweb/issues/3431)



* DN: This should be pushed out of v1 because it’s too unsettled, and there are further things we were talking about that uncover even more design issues. Details in the issue.
* KG: a bit sad, but tolerable. We will still be cool.
* **RESOLVED: pushed out to post-v1**


### [wgsl: full reference to module-scope variable is uniform, the value stored there may not be #3602](https://github.com/gpuweb/gpuweb/issues/3602)



* (DN:  I filed this when thinking through options for workgroupUniformLoad.  I don’t think it’s needed otherwise, and would complicate the spec a lot.)
* AB: Let’s defer this to the discussion of the uniform load issue.


### [consider expanding concept of full reference: include indexing into single-element array, and member access of single-member struct? #3605](https://github.com/gpuweb/gpuweb/issues/3605) 



* DN: There’s a sentence that tries to explain why we have the concept of a full reference. There are two ways to d this: have the full reference, versus notice overwrites of structs with a single member / arrays with a single element. (because they’re both the same memory - JB). There is a PR that Alan has reviewed that keeps that current rule, but has the extra explanation of the case of a struct with a single member, or an array with a single element.
* KG: Sounds tolerable. We still support full assignment if want to. This is just a case where the programmer can do something that should work, but doesn’t happen to at the moment.
* DN: Intent is not expand the expand concept of full reference - just wordsmith to make the explanation less confusing.
* **RESOLVED: take this change**


---


# ⚖️ Discussions


### [Workgroup Broadcast · Issue #2586](https://github.com/gpuweb/gpuweb/issues/2586) 



* PR: [Add workgroup broadcast and uniform load built-ins #3586](https://github.com/gpuweb/gpuweb/pull/3586) 
* DN: Had consensus last week, but we didn’t really clarify everything, found something that we didn’t handle. E.g. workgroundUniformLoad, where each WG provides a different pointer. So really the condition we want is to separate the address and the contents at that address. DN+AB iterated and we’re looking to keep something as straightforward as we can. One option is to abandon workgroupUniformLoad and go back to workgroupUniformBroadcast.
    * FYI: The confounding example is from [https://github.com/gpuweb/gpuweb/pull/3586#discussion_r1025511559](https://github.com/gpuweb/gpuweb/pull/3586#discussion_r1025511559) 
        * `override size=16;`
        * `var&lt;workgroup> a:array&lt;i32,size>;`
        * 
        * `@compute @workgroup_size(size)`
        * `fn main(@builtin(local_invocation_index) i:u32) {`
        * `   a[i] = i;`
        * `   let yolo = workgroupUniformLoad( &a[i] ); // the problem is i `
        * `   // yolo is what?`
        * `}`
        * 
* AB: So we think the way broadcast is done *will* work, but not an ideal solution for everyone. For uniformLoad though, it’s really the base address that matters. We don’;t have a way to let you have different base pointers per invocation. All of your address math would have to be formed out of an override expression, or anything that Counts-As. But wanting to do this fully, we’d need to do full-program analysis, sounds ugly. Wouldn’t be able to verify as you see the call. It could put an odd restriction on a function, maybe couldn’t use it on a pointer param, would have to use it on a global var. If you want something like uniformLoad, you need either full prog analysis, or something restrictive. Either way, you need some way to restrict to overload expression. If none of those are good, the Broadcast is the other option.
* JB: Is this instead of #3602? Or in addition to?
* DN: Instead of #3602. You want to make the address uniform. We’re going to restrict by construction, because the indexing math is Override, and so its’ uniform by construction. I pasted the motivating example above. The `i` is wildly non-uniform. The rule AB is saying, si the `i` is not allowed because it’s not an override expr.
* JB: So you’re seized upon overload expression having the right restrictions without introducing new language.
* AB: And without new analysis to data-flow-analysis.
* JP: What I’m missing is why we need to restrict to override, and we can’t use data-flow analysis, why not uniformity to figure it out?
* DN: That should get you what you want, but how do we say that… *thinking* Now you have a validation rule on wgUniformLoad, have to find indices that feed it, need the cone of deps, and require a uniform edge coming out of it. You analyzed the program, have to add edges after the fact.
* AB: First time we have a requirement that the value is uniform, in the graph. We have incidental usage, but anywhere you check rightnow must be called from UCF. That may not matter to you, but it’s a differfnt thing that’s cvhecked by uniform analysis.
* JB: The way I expect this to work, is eval of `a[i]` yields a reference, and uniformity of the ref would talka bout the address itself, would say not a uniform reference. Uniformity would not look into the value stored in the loc until you do the load. Can tell what the uniformity needs to be based on the storage space of the reference. Don’t need to track anything more than we already do?
* DN: That’s way I was headed, but current UA doesn’t distinguish them right now.
* JB: Doesn’t that mean UA doesn’t understand refs?
* DN: It has collapsed the two. An `a[i]` is both the address part and the value part. So every expr effectively has two nodes in it. 
* JB: That’s the #3602 approach?
* DN: Yes.
* JB: I read it, don’t understand why you need pairs. It feels like this should fall out naturally. When you apply a load rule, a uniform pointer might become a non-uni value at that time.
* DN: Could be.
* AB: You may not need to carry it in every node, but you do hav to track the value.
* JB: Don’t we just know that anything in such Storages yields a non-uni value?
* AB: Possible, as DN says, it’s a different cut-line with different sharp-edges. You could say “why didn’t you use a uniform” but I think it becomes more annoying to discuss and spec. If you want to make all read-write loads non-uni, you couldn’t chain uniform loads.
* JB: Well, isn’t that correct though? Isn’t that the restriction that we want?
* DN: It’s a restriction that we want to eliminate. But we also want to eliminate more cases.
* AB: COmes to “how many cases do you want to cover with this”, and how much work would it be to get more *useful* usecases working.
* JB: I thought this would be simpler.
* DN: Pair values *would* work, but making it so we only track single values *should* work. We already have to fixup rval/lval stuff, so maybe we should fix it at the same time?
* JB: I feel like we did the work do spec how references work, and that we should maybe try to go all-in. Sorry to ask for more time, but can I have another week? (yes)
* JP: We already have a special case for array-length, so if we were to solve this, we could get rid of that special case too.
* DN: Is JP volunteering? :)
* JP: I would be interesting in prototyping…something
* 


### [Designing a uniformity opt-out #3554](https://github.com/gpuweb/gpuweb/issues/3554)



* DN: Does Mozilla have a consensus here?
* JB: We have one about the spec, and one about the platform.
* JB: If the locally scoped opt-outs would serve the needs of people doing mechanical generation, it seems premature to provide a global opt-out, because the people requesting the opt-outs have not had a chance to really try out the new local opt-outs, vs the desire for a global opt-out. As far as we see, all of these proposals handles the needs of e.g. Unity doing mechanical transformations. We very much want to have the default such that people continue to get help even as the opt-out to individual parts of their program. [where they know better or don’t care]
* JB: We also do feel that the undefined behavior that plagues fragment shaders is not as severe as the UB that plagues e.g. C++. We want to put soemthing in people’s hands before we go too far in reacting to their demands.
* JB: The platform thing, there’s been saying in office hours, that derivative problems are not UB-like-nasal-demons, so question for the group: Is it indeed not UB, where drivers have no obligations in this case, or is it less than that.
* DN: I was looking at the D3D spec, says “unbounded value” or something like that. Have not looked at the VK spec. I think don’t worry about it. You’re going through a hardware channel, where they don’t want the world to blow up either, even if they might not be able guarantee you what you want.
* AB: It’s “undefined value”, which is like our “indeterminate value”. Will not destroy your shader.
* JB: Great! I do think the rest of our concerns still stand, wrt global vs local opt out.
* AB: Wanted to touch on something you raised, something we discussed internally. That this isn’t severe, and how hard should we try to prevent bugs here? We’re drawing an arbitrary line. It’s eaiser to get behind it for a barrier, where you will hang. But it’s a gradient, so I think the question is where we should put that line.
* DG: Wanted to clarify, that e.g. derivatives won’t be UB, not sec issue, but might just be junk. E.g. could you get different values in different corners of the quad?
* KG: Yes
    * KG: We also touched on this. Alan has a good point about being a line in the sand that we’re choosing to draw.  As Jim says, we’d be more comfortable we should give developers something to try.  Think many things could be satisfactory to their needs.  Maybe we’re asking for implementation time to see if their use case can be satisfied.  Generally don’t care too much about the specifics, except care that the opt-out is localized.  We have a bunch of things in the solution space that meet that. Would like to unblock those in spec and implementation.  Then wait for developer feedback.
* DN: Google will have to go back and think.
* KG: Where did the local opt-out end up?
* DN: TR thinks the attribute should be “this thing is uniform”, so it should capture dev intent about intent for the program. But thers’ teh case where you might put two of these, but they might both be lies. The dev should not be lying with an attribute in the program. But he can come up with a case where you can construct program that forces you to lie about uniformity. So he’s convinced me that a simplistic attribute alone would not be sufficient here.
    * [https://github.com/gpuweb/gpuweb/issues/3554#issuecomment-1319139705](https://github.com/gpuweb/gpuweb/issues/3554#issuecomment-1319139705) is my reworking.
* JB: Could we get a link? 
* DG: So is DN saying if we implemented a uniform opt-out attrib, maybe like a rust unsafe, and you did something unsafe in there, and you lied. Or by using….
* DN: Arg is, if only opt-out you add is “this particular val is uniform”, then there’s a program you can construct, where you have to put the attribute on two things….
* JB: So for an example program that *is in fact* correct, you have to lie.
* AB: That feels like we’re misusing the attrib, or instead just misnaming the attrib.
* DN: Yeah, this starts from TR’s desire that that the attribute is an assertion that a val is uniform, and could *potentially* be e.g. checked in debug mode. AB I guess would be saying that that may be too far along to start from.
* JB: The programmer’s understanding might be pretty complicated, such that a simple attribute might not be able to represent the complexity. For example, the programmer might know things like, “If x is true, then y is uniform, otherwise z is uniform”. You can write correct programs using that knowledge, but a simple “@uniform” attribute can’t express it. You’d have to mark both y and z as uniform, and that’s not true.


---


# 📆 Next Meeting Agenda Requests



* Next week: (**non-APAC**!) Tuesday, November 29, **11am-noon** (America/Los_Angeles)
* **Write any PRs you’ve promised!**
    * There are no other v1.0 issues outstanding (other than the ones on this agenda!) that are not blocked awaiting proposals.
* WGSL schedule:
    * December 06: APAC
    * December 13: APAC
    * December 20: Non-APAC
    * (Christmas Eve&Day Dec24&25, Saturday&Sunday)
    * _December 27: No Meeting_
    * January 03: APAC
        * DN: Suggest we cancel
    * Note: Based on our current issues remaining for v1 though, we are very likely to cancel meetings due to lack of discussion topics, so we can likely be liberal with skipping meetings.
* 
* [Designing a uniformity opt-out #3554](https://github.com/gpuweb/gpuweb/issues/3554)
* PR [https://github.com/gpuweb/gpuweb/pull/3644](https://github.com/gpuweb/gpuweb/pull/3644) : proposed a fine-grain and coarse-grain opt-out