# WGSL 2022-10-18 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** **(APAC!)** Tuesday **4-5pm **Americas/Los_Angeles

(Check your timezone: [https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+APAC+Meeting&iso=20221018T16&p1=137&ah=1](https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+APAC+Meeting&iso=20221018T16&p1=137&ah=1) )

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-10-11 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1MIQgOl7ijbmYcMr6SL83qpkHE6Kfv9Slg8eRKleLREA) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * Myles C. Maxfield
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * dan sinclair
    * David Neto
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Price
    * Rahul Garg
    * Ryan Harrison
* Intel
    * **Hao Li**
    * Jia A Chen
    * Jiajia Qin
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * **Shaobo Yan**
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
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



* 


### CG Charter



* **WGSL Only: [https://github.com/gpuweb/admin/pull/19](https://github.com/gpuweb/admin/pull/19)**
* 


### More APAC meetings during Nov and Dec



* We will be doing more APAC meetings during Nov and Dec.
* **We are also moving APAC slot one hour earlier** (4pm Americas/Los_Angeles), though this will drift again in early November with USA’s timezone change.


---


# ⏳ Timeboxes (until 4:15)


### [Clarify behavior of float division/remainder by 0 · Issue #2798](https://github.com/gpuweb/gpuweb/issues/2798)



* DG: Looked at ldexp in stdlib, defined to return HUGE_VAL for error conditions. Talking internally, we think we need something like this for a frozen value, so that we can test about it.
* AB: There’s PR#3508, which I think does describe a solution like that for “indeterminate value”.
* DG: Do we expect to define this val?
* KG: Implementation defined.
* JB: We want to ensure that e.g. Inf is still valid if supported.
* **Waiting on DN to merge #3508.**


### [Runtime behavior of NaN-producing math functions #3365](https://github.com/gpuweb/gpuweb/issues/3365)



* Myles still needs to make a PR. Can we defer this?
* **Yes, defer**


### [Spec doesn't allow local variables with the same names as parameters #3502](https://github.com/gpuweb/gpuweb/issues/3502) 



* **-> Post v1**


### [wgsl: limit nesting depth of if-else #3527](https://github.com/gpuweb/gpuweb/issues/3527) 



* JB: There’s no clear perfect relationship between limits in our target langs and wgsl source, so we should probably pick min of the target langs and divide by 2.
* AB: We can fall back on Spurious Error too.
* KG: We are just picking something to test, where we say “something at least this complex works”
* **Let’s take 127, and “any control flow”, not ifs specifically.**
* 


### [wgsl: grammar for LHS is ambiguous about grouping its subexpressions, affects type checking #3530](https://github.com/gpuweb/gpuweb/issues/3530)



* 


### [wgsl: Move * and & from lhs_expression to core_lhs_expression #3531](https://github.com/gpuweb/gpuweb/pull/3531) 



* (PR for issue #3530).  
* JB: I trust DN to implement this.
* DG: To clarity, this is indeed changing the tightness of e.g. function call vs pointer deref parsing to closer match other langs?
* AB: That’s my understanding.
* DG: It looks like no actually, that this would diverge us.
* KG: DN writes that we could look into [#1580](https://github.com/gpuweb/gpuweb/pull/1580) if we wanted an alternative
* JB: Because we don’t allow pointers to interiors of structs, the usual Rust/C parse is not useful: p[0] can never be a pointer. And given that we do want to use pointers then even though we would theoretically diverge, we probably don’t have any issues here in practices.
* DG: +1, I want DN here also
* **Tabled**


---


# ⚖️ Discussions


### [wgsl: pointer-to-workgroup in user-defined functions · Issue #3276](https://github.com/gpuweb/gpuweb/issues/3276) 



* AKA “function-out-param language functionality”
* See also:
    * (pr) [wgsl: ban ptr-to-workgroup params to user-defined functions #3422](https://github.com/gpuweb/gpuweb/pull/3422)
    * (issue) [Tuple-like-returns proposal for function-out-params functionality #3501](https://github.com/gpuweb/gpuweb/issues/3501) 
* JB: Moz is ok with the group consensus last week:
    * 1.0 leave spec as is today
    * With understanding that we’d expand this in 1.1.
        * Specifically, we’d use native constructs where we can, and on systems that don’t support this, use the Clayton Transform.
* JB: We think this is a solid compromise, especially if we can defer implementation complexity.
* AB: Awesome. But also, “leave as is” includes merging “ban workgroup pointers”
* MM: can we add a 1.1 milestone?
* **KG: I will make one**
* RC: Had TR review, his Q, what if we have different modules compiled at different times? Does this make it harder to reason about?
* AB: You’d need to see the whole module if you need to do a transform. Nowhere that doesn’t support smarter pointers is going to require this transformation.
* TR: Yeah, my concern was just for when modules might be compiled separately. Specifically what [thing] was written to, and that we don’t have [thing] across module boundaries.
* JB: Difference between being able to validate separately, vs emitting code separately.
* TR: My understanding is that you can’t have two function args, that are both pointing to the same root ident. But that …
* JB: What analysis says right now, is that it’s not just about the function, but also cares about its callees.
* TR: Yeah so you’d need new metadata about callees I think.
* JB: We don’t have separate compilation right now, so we don’t know what that would look like, and so it’s hard to speak to any problem there.
* TR: Ok
* MM: +1 to JB about how we would approach this. 
* TR: One more Q, a little confused about aliasing rules: If you have  pointer into aggregates, it seems like you should be able to, but couldn’t find example except indexing an external buffer. I was wondering if you can point into two points within an aggregate?
* AB: You can make those pointers into aggregates, but you can’t pass those into functions according to spec.
* AB: I’d like to get MM to clarity which carveouts MM wants us to remove in 1.1 specifically.
* MM: Sure.
    * Alias analysis, I don’t know enough about the Clayton transform to know if if preserves the order of writes and reads. If you have two pointers and they do alias, does that change their order.
* BC: Implementation on HLSL, aliasing for `inout`is still a hazard, so I propose that we keep that restriction in 1.1.
* MM: If we keep that, imagine we have array, and you have an index into the array, but you can’t know if two of those would point to the same part of the array, so we’d still need to prevent that?
* BC: Maybe? I could see how it would be annoying if you wanted a swap function to e.g. swap two elements of an array by pointers.
* AB: You wouldn’t necessarily know if e.g. two dynamic/runtime pointers had the same target.
* BC: That’s part of the “shape” of this.
* KG: I would like to see more examples if 
* MM: Are we going to relax the restriction against subobject pointers?
* AB: Yes, but you still couldn’t pass in two subobject pointers to the same object.
* MM: so this is a pessimal analysis here, where we’d forbid just in case.
* **Merge ban-ptr-to-wg, and add v1.1 milestone ([#3422](https://github.com/gpuweb/gpuweb/pull/3422))**


### [Uniformity analysis is far too strict #3479](https://github.com/gpuweb/gpuweb/issues/3479)



* KG: 
* MM: I wanted to mention that if we need UnAn for security, then we can’t give an opt-out here. There was some talk that this might be the case (during office hours?), but we need to find out. I think need to figure out if we do need this. 
* BC: We did talk with one partner and asked to be able to publish/share some of their shaders, and in the mean time we’ve agreed to drop Chrome back to UA warnings instead of hard errors.
* BC: One issue seen is that scalars which require separate uniformity tracking are being packed into vectors as part of earlier shader pipelines. The spec doesn’t currently track uniformity across vector lanes, but it could. Even with this - there are cases in our partner’s corpus that wouldn’t be solvable by adjusting the UA algorithm - we need **_an _**escape hatch.
* AB: One major fix, was when I added the value assignment, where if you reassigned an aggregate, it could become uniform again. However, we do think there are cases where there’s just no recourse that we can analyze.
* AB: We think that we need some kind of escape hatch though. 
* AB: To MM’s point, I don’t know how to give proof of security here, but it would have to do with e.g. what we do for indeterminate values.
* JB: One reason this convo this will be a little odd, is we’re not just talking about what changes we make to the spec, but we’re also talking about how to relate to committee members’ partners. We’re on the verge of getting super valuable feedback from authors. Particularly, we want to get this valuable information out of people hitting, and we want to do it without being insincere.
* AB: 
* KG: While it makes sense to give partners a temporary carve-out so work can continue, we don’t want them to show up in three months after we’ve done everything we can and say, “we still need a global opt-out”. Ideally, we want something like Rust’s `unsafe` blocks, where you can mark one section as “trust me”, but still get the benefits of the analysis from other areas. But we still want feedback from them on what are the actual issues that people are running into, so we can fix them for other people. It sounds a bit like they don’t want to fix the problems, they just want the analysis to be turned off.
* AB: This partner did post specific issues, and they did explain the parts that don’t work. They’re not just saying, “turn it off”. We’re not proposing a global switch, we’re open to a range of possibilities from the broad to the specific
* KG: We want the client to feel heard, but we also want to make sure that we can move forward by hearing more about the shaders that fail. We want to unblock them in the short term, but we need to get the feedback about the details
* MM: One lens to view this discussion: Should we track vector members independently? yes/no? Should we split variable lifetimes at assignments? We knew it was a likely request, here it is. Discard is already fixed. Should we add a uniform load function? Now we have a request for that. One way to avoid heated discussions is to address the concrete requests that we already have.
* KG: Yes - we want to collaborate. I didn’t know that those things were specifically asked for, because they were not stated directly in the issue.
* AB: We can make the analysis more accurate, up until we run into similar problems to those that arise with alias analysis, where the results depend on runtime behavior. They have issues beyond what is reasonably solved by the analysis. For example: they sample a texture, and based on that they choose which texture to sample. The issue has gotten long, and these specifics are hard to find. We’re working on getting the details from them. We all agreed that we wanted uniformity analysis, and we’re committed to make it work, we’re not abandoning it
* KG: We just need to concentrate on specifics here.
* MM: I’m not sure that having an opt-out is better than just not running the analysis, but we can leave that for another discussion.


---


# 📆 Next Meeting Agenda Requests



* Next week: (non-**APAC**!) Tuesday, October 25, **11am-noon** (America/Los_Angeles)
* 

FYI:: Merged. [https://github.com/gpuweb/gpuweb/pull/3539](https://github.com/gpuweb/gpuweb/pull/3539) wgsl: Fix how we say concrete left shift must not overflow (if known before runtime) #3539 



* Fixes a drafting error from before.

[Allow signedness to differ between integer texture builtin arguments (C template) #3536](https://github.com/gpuweb/gpuweb/issues/3536)
