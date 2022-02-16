# WGSL VF2F 2022-02-08,09 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️ Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon Americas/Los Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---



* 


---


# 📢 Announcements


### Office Hour



* **None this week!**


---


# Day 1 (Tuesday Feb08 10am-1pm)


### PSA: Please add issues on the agenda for tomorrow.


## 📋 Attendance

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
    * **Kai Ninomiya**
    * James Darpinian
    * **James Price**
    * Rahul Garg
    * **Ryan Harrison**
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
    * **Dzmitry Malyshau**
    * **Jim Blandy**
    * **Kelsey Gilbert**
* Connecting Matrix
    * Muhammad Abeer
* Kings Distributed Systems
    * **Daniel Desjardins**
    * Hamada Gasmallah
    * Wes Garland
* UC Santa Cruz
    * Tyler Sorensen
    * Reese Levine
* **Dominic Cerisano**
* Eduardo H.P. Souza
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Michael Shannon
* Pelle Johnsen
* **Timo de Kort**
* Tyler Larson


### State of the spec (dneto)



* [Slides](https://docs.google.com/presentation/d/1WIt_9sw2BGIJcnAd92sQ5me9LeEsi9YAPyIxSrOK2HQ/edit#slide=id.p)
* SPIR-V Mappings
    * DM: SPIR-V notes aren’t normative, so [...]   should have a separate document with mappings. And include other languages like GLSL.  It’s useful for people.  And doesn't’ have to be 1-1.
    * AB: I think we should drop spir-v mappings, don’t think they’re providing value anymore. We do need to be careful in our specs that we don’t accidentally describe our translators (e.g. Tint, Naga).
    * KG: About SPIR-V mappings. One thing that pushes me to want to keep external references, even though we fully describe, is sometimes it’s useful to have these equivalencies if you already have them.  You save someone a bunch of work.  E.g. the modulus operation here is like *that*.  Folks coming from other places can take good shortcuts.  Can be informational, not normative.
    * JB: Should these connections be flagged as non-normative.
    * KG: Yes.
    * DM: Specify them for HLSL as well.
    * KG: Yes, where it makes sense. Many that fit.
    * BC: Maintaining mappings is a lot of work / churn.  And can be compiler dependent (as Alan said). Suggestion: a page of example constructs, and a live server to see input/ output results.
    * AB: When you add multiple mappings, it bloats the spec quite a lot.  E.g. a table segregated in the spec is better than inline.  Also good is “go look at that other doc”.
    * KG: Gave reason why it has value. Won’t stand in the way of removing them.
    * &lt;return to topic>
    * MM: What are our AIs here?
    * KG: I think we’re good to remove, and we can re-add anything we want later.
    * JB: **I would be interested in writing that side-document**
    * AB: **I am happy to remove them.**


### WGSL Goal section



* DN: first crack delete it, second crack say something about portability and performance (polyfills)
* MM: we’d like something. Internal feedback that goals are wildly unbalanced.
* DN: **can draft something**
* MM: suggest we don’t spend too much time on it since it was acrimonious in the past


### Extensions



* MM: match WebGPU, has conditional parts of the main spec. Was an intentional decision. Compressed textures as an example (e.g.: [https://www.w3.org/TR/webgpu/#texture-compression-bc](https://www.w3.org/TR/webgpu/#texture-compression-bc) )
* DM: follow main spec. Sort of wish it could just be a github branch
* KG: hard to maintain
* **Consensus: Do what WebGPU does. (conditional inline with the spec)**
* MOD: what about unicode identifiers?
* MM: Probably part of a larger feature pack that gets added
* JB: is spec option in addition to versioning or only mechanism?
* MM: agenda topic


### (MM:) “What is our plan for how to not break stuff when we add things to the stdlib, e.g. namespaces, or Features…” I think it’s important to have a solution by the time the first browser ships.



* (DN: #600 was the issue I filed on this, which we resolved to have features, and maybe namespaces but we never went back to that.)
* [https://github.com/gpuweb/gpuweb/issues/2426#issuecomment-1032109816](https://github.com/gpuweb/gpuweb/issues/2426#issuecomment-1032109816) 
* MM: Posted last night, so it’s a little soon.
* MM: With namespaces, we can put new stdlib things in namespaces, and new ones won’t conflict then. We might want to add a new syntax e.g. array types, and namespaces don’t solve that, but it helps with new builtins.
* MM: They can also be useful for scoping global variables between entrypoints, too.
* MM: I think this work in conjunction with the enable directive. So if we add e.g. array brackets, we still use the enable directive.
* MM: To JB’s question, new features would come in the form of new namespaces/additions and new directives.
* DM: I remember there was a discussion about namespaces a while ago, and we decided that we can just put builtin in root, so can someone explain the gap there?
* MM: I think we couldn’t agree on how to divvy up namespaces, so we compromised on putting them in root. I do think that we have new problems to solve here, so namespaces have more value this time/for this.
* JB: Background: every other language design I’ve been involved with, it’s been hugely fraught.  We should not beat ourselves up if we struggle with.  Substantively, feel it’s easier for me to keep track of things when theres a linear progression of additions to the language. There’s a lot of ergonomic benefit to having versions over having a series of options that can be enabled individually.
* BC: Separate namespace per extension: Not sure you need it.  If part of the set of rules of namespaces that there are reserved namespaces, then you can ensure there are no collisions, as long as extension authors don’t collide.
* BC: Am vague about this proposal an “requires”. Long time ago, we said we’d have a block of { .. } with requires, and you could put a feature in that program.  My argument for that was linking needing to link across those boundaries. Not clear where the proposal stands on that. So really want the requires/enables at the top of the source.
* BC: With Jim on the unpleasantness of having a bunch  of independent named features. That’s complex for implementers and users. But versioning doesn’t play nice with features that only exist in certain implementations. E.g. f16 we really like, but we can’t make it core overnight.  Have to wait for the broadbase of hardware to catch up to features that people wanted yesterday.
* AB: Still see namespaces as purely additive.  We can release the lang without ns.  Future, add features with an enable.  Once you add an enable, it’s ok if your code got broke because you opted in.  Feature soup seems inevitable. It’s hard to get core versions out if the only things you could add have to be supported everywhere. It would move extremely slowly.
* MM: About linear progression.  Think that’s reasonable.  For features like f16, probably doesn’t belong in linear progression of the language.  E.g. raytracing doesn’t belong.  Something like unicode identifiers *does* belong in next version of the language.  Think that idea naturally fits here.  E.g. there’s an extension named “wgsl2”, then “wgsl3” that each is superset of the previous. Not proposing a separate namespace per extension.  If adding new builtins to standard library, could have new enable, without new namespace.  Keep adding them to the existing namespace.
* MM: Not trying to change the “enable” being at the top.  This is just about name resolution.
* MM: An “enable” is conceptually a namespace. You import new functionality into the global namespace.  One of the reason namespaces is good is it gives shader authors the ability to do something the standard library  / implementation already has.  It also supports feature of grouping things; that’s a shader author problem that is not a problem for the standard problem.
* KN: Namespaces good.  Jim’s right “modules” system is the hard part.
* KN: Thought we were happy to put all standard library stuff into the global namespace.  Including extra helpers, for hardware independent features.  Don’t think it’s a breaking change.  Think we should continue putting hw-independent things in the global namespace.
* KN: Try to stay away from language versions. That is the “way of the web”.  E.g. unicode identifiers. Don’t think you need an enable.  Non-breaking language change should just roll out as browsers implement them. Don’t think a browser should have to validate a rule like that.
* MM: WGSL is fundamentally less dynamic than JavaScript.  In JS you write an if statement to see if you can use a feature. Can’t do that in WGSL.  Example: unicode identifiers. Authors will want to detect whether they can use that feature.  To detect whether unicode identifiers can be use, the author will try to compile a shader with a unicode identifier.
* KN: Put the feature reporting/detection outside the shader. You would be able to detect availability of Features via JS, but it’s unconditionally enabled in shaders, and just works.
* MM: So the information flows from the browser to the author.  (Not the author providing the info to the implementation)
* MM: I worry about fingerprinting, but that sounds reasonable.
* KN: There is no fingerprinting beyond the browser version. It’s strictly tied to the browser version and there is no further identifying info.
* KG: Regarding versioning, I think there is a middle ground. Might choose to bundle a bunch of extensions into a core version of the language.  Could be a very far future thing; can tackle it when we get to it.  Think the enable directive is sufficient for now.  Can decide versioning model later. Allow the luxury of not solving that problem now.  Example: ES 2.0 vs. ES 3.0 is really valuable, but the gap is huge. (Large granularity)
* BC: To Kai’s idea of a rolling language feature set.  Two things seemed in conflict: don’t need a namespace for library. How do you deal with symbol collisions, e.g. user code declares something that is later added to the core.  If you don’t declare the version/features of the code in the shader code itself, then that makes offline tooling difficult.
* KN: For second thing, that’s a fair point.  But not sure why tooling would need to do anything but the latest. 
* KN: For collisions, we can find another solution to that. We have to add namespaces too, that will add an identifier. Don’t think it’s a different problem. Can say for things added in the future, they can be shadowed by things the user defines. For example. 
* DM: Concern about that approach of  “your shader will fail anyway”. As a developer, I may be aware of 3 language features, and have shaders with 3 variants.  Bunch of work to try them all against all. Seems unfortunate.  Tragedy is it’s asynchronous progression.  Might help on the API side to have a true/false answer for whether a feature exists.
* JB: Specific example of unicode identifiers irks me. Nobody is going to keep two copies of their shaders.  Will just keep one copy around, the base case.
* MM: Think authors will have two versions: an old vs. a new (that uses many new features together).
* KN: People will use caniuse.com to see what has broad coverage among browsers.
* MM: Unicode identifiers is too small a granularity feature. Would want to bundle that with a bunch of other things that are independent of hardware features.
* KN:...
* MM: I can buy that.
* MM: Problem I’m trying to solve is a zoo of small features.  Rolling works when many browsers implement things in roughly same order
* KG: There is a long tail. We have a product “ESR” with support for year-old release. We have to care some. Would like to avoid situation where people don’t have a good idea about when something is going to work. Unsatisfactory is quickly checking can-i-use, try it once, the ship it.  Nice thing in JS small code snippet to check the feature.
* KN: The more we can make our problem look like the web platform’s problem, the better.
* MM: As long as the feature is feature-detectable.  Compile a shader and see if it fails is reasonable as long as the error/pass cases are consistently checkable.
* KN: think prefer the browser just reports which features are available, no async compile tests.
* KG: I feel that user namespaces have value for e.g. the encapsulation problem for conflicting var in entrypoints. I don’t know as we want to sign up more generally to agree to use namespaces for spec additions. I would like to see a concrete example, where namespaces solves a problem there, rather than saying “generally we will solve it with namespaces”. But, if we want to use namespaces *for* versioning, we do then need them in the initial release.
* MM: 
* JB: Don’t be guided by implementation complexity.  Casual design decisions by language designers have long term pain for users to deal with.
* KN: Do we need to have named namespaces for the encapsulation problem? Anon namespaces would solve that with lower complexity/impact.
* MM: I don’t necessarily think that namespaces are directly related to modules per se. I think that namespaces are simpler than that.
* DN: For visibility, everything is currently visible, so that’s sort of a decision about how to do modules. Second, we do need a solution for how to reference in-namespace entrypoints and vars from the API side.
* MM: Can use the fully-qualified names.
* DN: Ok, though anon spaces won’t work for conflicts there.
* KN: Was imagining that entry point names would still have to be unique across the shader. Encapsulation doesn’t seem as important for entry point names (vs bindings).
* MM: Not a huge fan, but acceptable.
* MM: If we do make them unnamed, we probably shouldn’t call this NAMEspaces
* DN: “extra scopes”?
* 

_(First break)_


---


### [Should unreachable-code be an error or warning? #2378](https://github.com/gpuweb/gpuweb/issues/2378)



* RM: I feel like things have gotten kind of stuck. We can easily change things in one direction in the future: We can allow more programs, but not the other way around.
* RM : I would like to stick to a most restrictive approach until we get data that it is bothering real users. We did get feedback that forbidding dead code was harsh for users, so we did allow that now. For analysis in dead code, that would be an error, and we can just wait for users to complain to us about it.
* DM: And that would include uniformity errors? (yes) What I said there was that we would be analyzing what we think the author wrote, rather than what they [meant?].
* RM: I think that’s like type checking. And I’m not sure that type checking and behavior are distinct checks.
* MM: I do think that we are validation what authors wrote, since they did write it, and they get what they get.
* DM: We would check validation for uniformity, but we would have to e.g. check uniformity past the point where we realize there’s e.g. a return and where dead code begins.
* E.g. should be rejected:
```
return; 
If (thread_id == 1) 
  control_barrier();
``` 

* This should be allowed:
```
If (thread_id == 1) { 
  return; 
  control_barrier(); 
}
``` 

* AB: As a user encountering this, I would be scratching my head.
* I would like the same kind of thing for rejecting the first one:
```
Return;
If (42u == 43.0)
```
* DN: The difference for me between value checking and behavior checking is with how e.g. uniformity analysis bleeds out of its immediate surrounds. BC said, once you get to * dead code, nothing in dead code affects behavior.
```
switch(thread_id) { … replace with a function body if needed, to make the point
  default: {
    break;  
    discard;  // this should not poison the textureSample below.
  }
}
textureSample
```

* DN: Don’t want behaviour of dead code to leak out badness to beyond it. I care about this more than whether we check dead code intrinsically.
* KG: Maybe to crystalize, maybe then “whether behavior analysis validates dead code” would fall out of a definition for the analysis. If all the edges leave, then the dead code couldn’t violate (some example of) an analysis. (I think a point DM made)
* AB: Type checking is precise.  We don’t get the types wrong.  There’s no ambiguity.  But for flow analysis, the author can know more than the compiler might be able to.  Author knows this code won’t execute.  People will complain on Twitter.
* RM: I do want to push back on DN’s “type checking is lexically scoped”, because behavior checking is lexically scoped also.  Option 2 does what David wants. I see behavior checking as a type of type checking.
* DM: One point to reemphasize is that static languages always check types, even though dynamically typed languages don’t.
* RM: Surprised by this claim that types are precise. I feel like it’s very similar to saying a piece of code has certain behaviors, though at runtime it has a subset of the possible behaviors of that snippet [in the abstract].
* KG: Uniformity analysis can be consistent in several ways.  It’s a choice.   Can be tautological.
* RM: For this analysis “dead code” is   s2 in   “s1 s2” where S1 does not have Next in its behaviors.   It’s a definition.
* MM: I don’t think discussion on philosophy is helping make progress here. What specifically would move the needle on peoples’ opinions here? In particular, can we wait until users complain?
* DN: I would need to see why it’s helpful to the user. Adding a cutting-off “return” is an intent by the user to say “stop control flow here”.  The implementation already has a “should warn” obligation in that case when there is code that follows that cutting-off return.  I would need to see why it’s helpful to a user to add potentially _additional_ errors based on control flow when the user knows the flow won’t get there.
* RM: 
* AB: I would like behaviour analysis to be as precise as possible.  Then uniformity analysis should provide useful errors when it can detect them.  The closer I can get to precise behaviour analysis, then we can pay less attention to the uniformity analysis. 
* KG: Concretely you want us to iterate more on behaviour analysis to reduce the false negative cases, such that it’s precise enough so people wouldn’t have frustrating problems with it.
* AB: There are two ways we can go, (taking aside errors vs. warnings).  Prefer to decide on sets of rules like, where “s1 s2”, if “s1” has behaviour {Return}, then analyze “s1 s2” as having behaviour {Return}, i.e. ignore the behaviours coming from s2 in this case.  Compared to the current draft the spec, this allows more valid programs, and gives more precise info about the actual actions of the program.  Would add rules to the table, but that’s ok.
* AB: Uniformity analysis already needs to know behaviours.  Propose adding more behaviour cases where it reflects reality of the execution.
* RM: Can change rule to avoid adding behaviours from dead code is easy (spec wise and conceptually).  Would be ok to modify the rules to disconnect a subgraph when that subgraph is proven to be “dead”.  But much harder to modify uniformity analysis to not even run/analyze on dead code (building a subgraph for it in the first place).
* AB: We could make uniformity analysis be more aware of behaviours.  Now we’re talking about mechanisms. But not further on philosophy.
* RM: Could try to write versions of the analysis to show the options.
* DN: Would be most helpful if that came with separating cases.
* AB: what would it take for me to change my mind.  Will people understand.  Like the idea of workshopping different sets of rules.  E.g. if get 800 lines of spec that’s not graspable by folks.
* RM: **Can try to write proposals, and re-open /revisit Alan’s earlier PR.**
* AB: Will look at that and discuss internally.


### [Literal unification, bottom-up #2227](https://github.com/gpuweb/gpuweb/pull/2227) 



* David’s latest:   [Slides](https://docs.google.com/presentation/d/1x66oDVgtC6BoBCyYVtApMxssxr8-ayOe4uZBnFLjn0o/edit?usp=sharing)
* DN: Status: I wrote up a PR, was reviewed, review said “rules are confusing”, so I feel like this is maybe not quite working. I went to rework it, and decided this is “override resolution”, went to e.g. C++, and pulled out a solution to handle this. That’s why this is going to be bigger, and is still forthcoming, but I think it will reflect what people more intuitively are used to, despite apparent complexity.
* RM: I like it.


### (MM:) “How to encapsulate/associate resources with a module without them colliding in name?” E.g. two modules with different resources, but using the same name, like “u_tex”. [https://github.com/gpuweb/gpuweb/issues/2426](https://github.com/gpuweb/gpuweb/issues/2426)



* Covered in “What is our plan for how to not break stuff”


### [Add optional trailing comma for the attribute syntax. #2563](https://github.com/gpuweb/gpuweb/pull/2563) 



* MM: workgroup_size can take 1,2 or 3 parameters
* AB: interpolation is similar.  Can have 1 or 2.
* **_Consensus to land_**

(break!)


### Burndown of [https://github.com/orgs/gpuweb/projects/2](https://github.com/orgs/gpuweb/projects/2)



* (Let’s triage what we can out to post-v1)
* [https://github.com/gpuweb/gpuweb/issues/579](https://github.com/gpuweb/gpuweb/issues/579)
    * MM: Can we come back to this?
    * RM: We were strongly opposed to having just loop/continuing, but now that we have for loops, we’re satisfied here. I would like to propose `while` loops, but not needed immediately.
    * MM: So therefore, I think we can just close this issue.
* [https://github.com/gpuweb/gpuweb/issues/1394](https://github.com/gpuweb/gpuweb/issues/1394)
    * DN: No, because we don’t have Storage for const
    * DM: At a global level we don’t have storage so much as named expressions, so we can close this.
* [https://github.com/gpuweb/gpuweb/issues/2092](https://github.com/gpuweb/gpuweb/issues/2092) 
    * MM: Can we…have a builtin instead?
    * RM: I think it’s better to have C-style compat here, and that we should try to fix it, and only punt if we fail to do it reasonably well.
* [https://github.com/gpuweb/gpuweb/issues/2211](https://github.com/gpuweb/gpuweb/issues/2211)
    * MM: Please come back to this
    * MM: We want to close this, to be superceded by #2575
* [https://github.com/gpuweb/gpuweb/issues/1063](https://github.com/gpuweb/gpuweb/issues/1063) 
    * KG: We have this now, from MOD, yes? (yes)
    * RM: There’s one ambiguity but we can close this.
    * See: [https://github.com/gpuweb/gpuweb/pull/2539](https://github.com/gpuweb/gpuweb/pull/2539)
* [https://github.com/gpuweb/gpuweb/issues/921](https://github.com/gpuweb/gpuweb/issues/921) 
    * MM: To butcher DM’s thought (maybe!) “native APIs have this and get value out of it, so we should consider this?
    * BC: Should we reserve `demote`?
    * KG: Probably and `demote_to_helper` to be safe
    * KN proposed `@demote_to_helper discard;`
    * **BC to reserve these**
* [https://github.com/gpuweb/gpuweb/issues/1202](https://github.com/gpuweb/gpuweb/issues/1202) 
    * ?: May want to come back after implementation experience
    * ?: To start, rely on implementations to try to fix this (bounds check elision etc)
    * MM: What could we even do from a spec standpoint?
    * JB: Can add things like iterators/foreach that don’t need bounds checks (like Rust)
    * MM: Do we need this for v1? Probably not?
* [https://github.com/gpuweb/gpuweb/issues/1867](https://github.com/gpuweb/gpuweb/issues/1867)
    * DM to comment and close
* 


---


# Day 2a (Wednesday Feb09 10am-1pm)


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Myles C. Maxfield**
    * **Robin Morisset**
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * **Ben Clayton**
    * **Brandon Jones**
    * **Corentin Wallez**
    * **David Neto**
    * Ekaterina Ignasheva
    * **Kai Ninomiya**
    * **Ken Russell**
    * James Darpinian
    * **James Price**
    * Rahul Garg
    * **Ryan Harrison**
    * Sarah Mashayekhi
    * Jaebaek Seo
    * **Austin Eng**
* Intel
    * Hao Li
    * Jiajia Qin
    * Jiawei Shao
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * **Yunchao He**
    * Zhaoming Jiang
* Microsoft
    * Damyan Pepper
    * Greg Roth
    * **Jesse Natalie**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * **Dzmitry Malyshau**
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
* **Dominic Cerisano**
* Eduardo H.P. Souza
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* **Michael Shannon**
* Pelle Johnsen
* Timo de Kort
* Tyler Larson


### Air time for Google, Apple, Mozilla and anyone else to present features or changes they want for V1



* (Ideally each company could have some slides/materials prepared if there are still changes they wish to make to WGSL)


### [WGSL Pre-V1 Changes Google.pdf](https://drive.google.com/file/d/1AwCEBVo-QncqDf0WJcbV0DkAeTxL77pt/view) 



* AB: (presenting)
* Texture Built-in Function Integer Parameters
    * DM: overloading sampling functions even more for signed/unsigned seems heavy-handed. Let’s not do this if we can avoid it.
    * AB: Do you have a preference?
    * DM: Have filed an issue internal-gpu.
    * BC: I raised this when we did some internal demo days. Mismatch between invocation ID being unsigned vs. coordinate.  Another way to fix this is optionally allowing invocation ID builtins being i32.
    * DM: So it’s how opinionated do we want WGSL to be.  Should we give the user flexibility, or be strict.  I think we should be strict.  When we have literal inference of 0 as either signed/unsigned, we should make the integer coordinate parameters unsigned, and be strict.
    * MM: Comes down to whether it’s conceivable for these to go past 2B. We discussed for array size and decided “yes”. But for texture size? Workgroup size? Probably not.
    * AB: There are some convolution filters that have some negative offset math; transforms you can want to do that would be simpler with i32.
    * DM: There’s also the spec complexity and implementation complexity, and complexity for end users.
    * AB: I don’t think the spec is the hard part here.
    * DM: Would still be restricted for some params but not others?
    * AB …
    * DM: We have a lot of overloads already, and we’ll need to introduce generics for the spec’d params here.
    * MM: Are you making a perf point?
    * DM: No
* Concise Shader Stage Attributes
    * AB: Saw Mozilla feedback in that it reduces searchability.
    * JB: Later slide also suggested for builtins.  Builtins is a big set. These changes bring things to the top-level.  Understandability issue: when you see a word you have to know what kind of thing that is; have to remember. Advantage of having fully spelled out syntax is you can skim it easier, and you don’t have to know the details.  They tell you what you need to know; if you’re not focused on that detail, then you can skip over it easily. You’re removing hints. (some redundancy)
    * AB: Other languages don’t give these hints, e.g. HLSL. Does ring true the feedback that you reduce searchability.  Already have some context to give you hints. Does lean a little to those more familiar.
    * MM: Agree context does give you some info already.  Another comparison is types.  We don’t annotate every place saying something is a type, saying it’s a type.  This proposal matches that design.
    * DM: At least you know to expect a type there.  But there’s potential confusing for these cases. E.g sample index vs. interpolation. Need to remember more.
        * We’re more concerned about understanding and educational aspect.  Lean to readability rather than writability.
* Concise interpolation attributes.
    * AB: Same, but for interpolation.  Flat stands relatively independently of the others.  The others have defaults. Can save the amount of material to write.  Ceases to be a problem once you’ve seen a few shaders.
* Concise Builtin values
    * AB: Similar vein.
    * AB: Also flexibility in signedness of integral built-in values.
* MM: Specific thought about stage.  We might add stages in future. Thinking about raytracting. E.g. intersection shaders; they have parameters. They have a different set of parameters that affect the way the intersection shader is invoked; e.g. is this about intersecting triangles, or rectangles. E.g.  intersection(triangle). The stage thing might take sub-parameters in the future. So @stage(intersection(triangle)). That kind of thing would be more clean if you promoted “interesction” to the top level.
* DM: Does that kind of thing apply to compute stage with workgroup size?
* MM: A little beyond that because the kinds of things you can put in there changes; any subset of about 5 things.  E.g. intersection(), or lots of combinations.
* JB: Another concern, everything is scoped / namespaced. So you can add more builtins and it will never collide.  Having more structure is helpful here. 
* DM: Compute workgroup size: think it’s helpful to spell it out.  It’s not so clear when it’s smushed into the “compute” word.
* MOD: For translations, concise can be better. One example: vector instructions is easier to translate compared to having more specified such as vector-unit instructions or vector-operanded instructions.
* MM: Makes sense when the word has a direct meaning translation. But “compute” may be too generic.
* AB: **Let’s split discussion: conciseness,. Signedness**.
* Optional semicolon on last member in struct declaration
    * AB: Small thing.  
    * DM: What’s the motivation. Write struct 
    * AB: Like trailing comma being optional.  Avoid annoying syntax error.
    * KG: Feel different from commas. Doesn’t feel easier to me.  It feels like adding a rule where you add a rule for the sake of it.
    * AB: We did discuss whether the semicolons should be commas instead.  But didn’t have consensus on that.
    * MM: Extremely weakly against this.
    * DM: Think we either leave it as is, or switch them to commas and have the trailing comma optional.  Its commas in Rust.
    * (KN: fwiw typescript allows both commas and semicolons and the last one is optional in both cases)
* Static assert
    * AB: Think this is nice-to-have for developers.
    * KG: Like it
    * RM: Like it
    * MM: Good idea.
    * MM: Let’s not add a new concept of constexpr. Should use the existing idea.  Should be able to put it anywhere, and use the normal scoping rules.  So e.g. `const x = 7` in function scope then use `static_assert(x==7)` should be fine.
    * **_All: yes._**
    * AB: Don’t want to bikeshed the name.
    * BC: And no string type.
* Workgroup broadcast
    * AB: Really ties into uniformity discussion. 
    * AB: Has two forms: value and pointer.  Pointer version is for potential efficiency win.  The idea is that uniformity analysis would know this as a special construct.
    * RM: Generally agree this will work.  Don’t know if the polyfill is exactly what we want but it seems reasonable.
    * AB: This is a generic polyfill. An underlying implementation can do something better if you know more about it.
    * AB: A downside to this polyfill is it consumes some workgroup memory, and so eats into your budget.
    * DM: Think this is useful, and is basically the conclusion to the discussion about the uniform loads.  Can we expose something lower level, and allow the user do the barriers and declare the workgroup variables themselves.  So the user can see the accounting of the workgroup space.
    * AB: Think the variable can be exposed, but difficulty is pulling the barriers out.
    * MM: Think that’s worse.  Think the contract of this builtin is that this function needs some source.
    * MM: Concerned about portability. This polyfill prescribes the first thread id. Need to try on various implementations to see what’s best.
    * AB: The only constraint is it’s *some* single thread. Could be any of them.
    * MM: If it really is the first thread’s value, then we should be specific about that in the spec.
    * …. workgroupBroadcastFirst?
    * CW: The particular thread id only matters for the value-form.  If it’s the pointer form it doesn’t matter.
    * AB: Agree you have to specify which threads value is broadcast.  In the ptr version, the pointer value has to be uniform.  Helped here because you must only call this from uniform control flow so all the threads are there already.
    * MM: **This is actionable by itself, so yes, pull it out of the uniform-load issue**.

### [Proposal: Make ( ) optional for if, switch, loop and for #2575](https://github.com/gpuweb/gpuweb/issues/2575)

* BC: Parens around these things don’t serve any purpose. The parens can be absorbed by the expression.  Some folks at Google expressed preference against this, from background in C.  Many modern languages don’t require this. I keep getting compilation errors.
* BC: The for loop is the most contentious of these.  Golang allows the form shown.  You know in the grammar when the thing is over.  Because we enforce braces.   
* RM: Fine going either way for for loops. For if and switch, precedent of Rust shows it’s readable without the parens.
* DM: Appreciate removing these parens is backwards compatible.  But for-loop looks weird. Would rather keep it.
* KG: My feeling as well.
* MM: An argument in favour of the “for” is Jim suggested having new kinds of for-loops; for those, you would not have parens.   You’d want whether parens are required to be the same for all the kids of ‘for’ loops.
* KG: Can be backward compatible to remove them later.
* KG: Like that it allows these to be optional.  Not so foreign to me any more. I write python too, and it’s optional there.
* KG: The point about making it harder if we want to omit the curly-braces.  I’ve been proponent of making braces optional in the past, so a bit sad. But at least it’s the same number of characters saved.
* MM: Might be reasonable to say you can omit the braces but only if you keep the curlies, and vice versa.
* **_Resolved needs spec for “if” and “switch”.  Not resolved for “for”.  To revisit the “for”, make another issue._**

### [Reconsider adding while to the language #2578](https://github.com/gpuweb/gpuweb/issues/2578)

* RM: Since prior argument long ago, we added ‘for’ as syntactic sugar. Want to repeat the pattern to give a while loop, as sugar over   for ( ; cond ; ) {  }
* RM: Benefit is familiarity.  Cost is small. Benefit is real.
* BC: In internal discussion, there were no strong feeling. My concern is having three keywords for expressing a loop.  An alternative is      loop (cond)  { }
* BC: Golang only has `for`, to express all these things.  It’s “the loop” of the language.  This is quite minor.  I can live with three keywords.  It’s remember what keyword maps to what kind of loop you’re dealing with.
* JB: Rust also has a “loop” keyword which has an infinite loop. It’s important in Rust. We have definite-assignment rule. We have type inference. Both  benefit knowing there is no exit other than a “break”.  So if you have while(true), you have to look at that condition, which is a burden. Having a syntactically specific form that has no exit is beneficial for a bunch of these analyses.  Seems we have analyses that would benefit.
* KG: you don’t think we will improve our analyses to recognize while(true).  Or is it something we need to teach best practice that loop { } and while(true) look like they do the same thing but they have material difference on analysis.
* JB: You do have to teach folks that sometimes loop is better to use.
* JB: It’s important to distinguish between analyses that the language guarantees will occur, and those that it could perform opportunistically. Sometimes it’s important to have a loop that is guaranteed to be infinite, independently of how smart the compiler is.
* AB: while is small amount of sugar on top of other things.  Seems reasonable.
* DN: Agree.  When we describe it in spec; prefer to see it as sugar over loop {  } Rather than two levels of sugar.
* **_Resolved needs spec._**


---


## Non-WGSL (Corentin)

Administrivia



* Meet at TPAC Vancouver from 12 to 16 September 2022?
* **No, not safe until it is declared to be safe.**

Status updates



* Mozilla
    * Specification (Dzmitry) [slides](https://docs.google.com/presentation/d/1at-kQh7U9jRk5PZ14VLe76M7rQoTeRaizUec-CX2DH4/edit?usp=sharing)
    * Gecko implementation (Dzmitry) [slides](https://docs.google.com/presentation/d/1LRdNpZVeUdf9Ig5tpPMP3xmA-QkC0m-mygjPyOqsryk/edit?usp=sharing)
* Google:
    * CW: Hard to say everything that was done, but Chromium is approx feature complete
        * Missing view-formats, features from the last month
        * Missing Stencil8 format
        * Missing Canvas options
        * Need to enable device.destroy
        * Ongoing work on zero copy texture import, and color handling for inputs
        * Awaiting resolution on pipeline overridable constants
    * CW: Enormous progress on Tint, to the point that we’re close to removing spirv-cross
    * BC: Things that aren't implemented in Tint.
        * Uniformity analysis TODO.
        * Behavior analysis is done.
        * Parity across major backends: HLSL, MSL, SPIR-V. Modulo a couple of bugs.
        * Remaining big things
            * Constexpr
            * Overridables
        * Enabling today: support out of order declarations.
    * CW: Also BC made a Deno Dawn module, useful to run tests without the whole browser
    * MM: Does it have to be Dawn? Is the shared header still alive?
    * CW: Yes, it (mostly) uses the shared header
    * CW: Massive amount of work on CTS. We’re tracking list of tests, there’s a lot, we’re hammering on it.
    * CW: Working with various people help them use WebGPU. It’s going ok, WebGPU is new, so it’s a lot of work, but it’s ongoing.
    * BC: Shoutout to RH for a ton of work on CTS for builtin precision testing and associated helpers
* Microsoft
    * RC: We continue to integrate the work from Chromium in Edge. Edge Canary is ~5days behind Chromium, but have the same channels.
    * RC: Contributing less upstream than in the past. But incubating with Michael at Microsoft and Enrico at Intel a SPIRV-to-DXIL in Mesa. Will be used to vk2d3d project.
        * RC: Hopefully we can use it in Chromium and other browsers. It passes all the dawn tests, the CTS, dneto’s spirv-samples and repo, and more! As well as Vk1.0 CTS.
        * RC: Compilation is 4-6x faster than FXC and peak memory a lot.
        * RC: Also have prototyped a SPIRV-to-DXBC converter that passes all the Dawn tests. Just a couple engineering weeks to try it, but if there's interest happy to discuss.
        * KG: The projects are super exciting!
    * KR: Have you looked at the separability of that code from Mesa source tree?
        * RC: We discussed it and mentioned that we could use similarly to how ANGLE is imported in WebKit. It is separable but you need some of the rest of Mesa as well.
    * DC: Is Babylon native integrating WebGPU?
        * RC: Not on the Babylon team, but know they are integrating WebGPU. Don't know about Babylon native.
        * CW: WebGPU for Babylon native not even started yet AFAIK.
    * DM: Khronos portability would be interested in the SPIRV-to-*. Also can you comment on the paths you take for shader translation in Edge?
        * RC: Today there is no difference between Chromium and Edge for that path. In the future it would be nice to use SPIRV-to-DXIL in Chromium. Would have Tint produce SPIRV and then convert to DXIL.
        * DM: Will Google use it?
        * RC: Hope so :)
    * MM: Mentioned the comparison with FXC. Did you compare with DXC?
        * RC: No didn't compare with DXC. DXC is less likely to ship, essentially LLVM and it is much bigger.
        * MM: FXC is part of the Windows SDK but not DXC so need to ship it?
        * RC: Both Chromium and Edge ship FXC, but DXC is much bigger.
        * JN: Version 47 of FXC is shipped on Windows.
        * RC: But it can be outdated in the system and have bugs, so browsers ship their own.
    * DN: From Google perspective, looking to ship something now. Contemplating the DXIL path in the future.
    * RC: Goals of the project are performance and conformance. But also FXC will not evolve anymore. So we need something to allow us to have new things in D3D12 in WebGPU.

DrawnApart brief and overview



* (KG) [Overview](https://hackmd.io/DZJkx5XzQjKHItpWEKBKRw?view) 
* KG: Wrote an overview of where we are.
* KG: Last year at least Mozilla and Google were approached by researchers that have a fingerprinting vuln in GPU APIs called DrawnApart. Idea is that with two GPUs that are the same, you can still deduce some fingerprint that persists for users.
* KG: Paper going through peer reviews, multiple reviews from the WebGL WG.
* JB: What causes the fingerprinting?
    * KG: The theory in the paper is that when you do GPU commands, the assignment of work to "execution units" is static, so you can run work on specific EUs by checking the VertexId.
    * KG: The idea is that you assign workloads to the same EU that has the same performance characteristics. "if vertexId == 0, run a big, fixed workload, else nothing". Then do this for various EUs. Profiling the various EUs.
    * KG: However when we talked to some IHVs, their answer was that HW doesn't work this way. Might be variance in how HW works.
    * KG: After profiling they feed the data in a machine learning classifier to increase the contrast and get a better fingerprint. Paper says they can get a good classifier after the ML.
    * KG: They claim with 30% accuracy (times when they can correctly guess which HW it is), much better than random guesses!
    * KG: Early proof of concepts done in a lab, mostly Intel (but also had success on Nvidia, Apple Silicon, Mali).
    * KG: WebGPU is mentioned in the second to last paragraph along with webgl2-compute. They used webgl2-compute for prototyping with compute shaders. They had workers race on a mutex and record the order in which they got the mutexes. Apparently it was much more successful: 98% accuracy and much lower runtime.
    * KG: Previously in the paper they were using more complex operations for the classifier.
* KG: So the question is about this, what do we do in the WebGPU group?
    * KG: Will describe the consensus in the WebGL WG as "tentatively unconvinced". Coming from earlier iterations of the paper that was less convincing. Paper is much better now. Had some technical feedback from IHVs: "if they are measuring something it is not what they claim to be measuring in the paper".
    * KG: The paper has theories about how the mechanism works, but it is an interesting model because they have results.
    * KG: There's a "google collab" (think Jupyter) where they run the classifier and show the results live.
    * KG: The results have been very difficult to reproduce (at least in Mozilla) so it seems that the ML classifier is important to reduce the noise.
* KG: Mostly wanted to introduce this to the WebGPU WG, not looking to solve it today. It is now up to us to figure out our response. Try to figure out how it works, find mitigations, etc. Need some engagement with ML experts, the "collab" should help with that.
* KG: Paper went through peer review and review from WebGL WG.
* KR: The mitigation for the WebGL version would be horrible as it would require randomizing the order of the vertices. Asked to see if the randomization with modulo would break the fingerprint but had no answer. Didn't have time to implement the mitigation.
    * KR: Colleagues at Google pointed out a paper that shows power consumption differences on CPU cores. Think this is the same idea, but on the GPU. Probably thermal curves.
    * KR: If this is true with the mutex thing, don't know how we would mitigate it.
    * KG: That's why we should evaluate first before jumping to conclusions.
    * KR: Couldn't reproduce at all.
* MM: Ken described that a modification would be to change the order of vertices. Wouldn't that break conformance?
    * KG: That was one of the earlier ideas. Another one was that you could reverse the order of vertex IDs, etc. As long as it satisfies the API guarantees.
* MM: Think you BTW this was super helpful.
* MM: Did the work involve timing how long things took?
    * KG: Timing for WebGL, but just regular code for "compute".
* MM: We (group or Apple) should treat this paper seriously. Even if it is wrong in the theory behind it, maybe the ideas can still be made to work on the GPU. There may still be a program that achieves what the authors are trying to achieve.
* MM: So the fingerprinting isn't distinguishing if you are on this GPU model or that GPU model, but instead they have a full lab of the same GPU and fingerprint on that?
    * KG: Correct.
* MM: Ken mentioned that this was also a problem on CPU. If you run JS benchmarks on a CPU you get results different from another CPU.
    * KR: That's why I want us to not do something drastic immediately because of that. But dramatically reducing the capabilities of the WebGPU API would be the wrong response.
    * MM: JS wasn't stopped because of the benchmarking through JS.
    * KG: Tor browser is not vulnerable to this because it uses software for WebGL. Have possibilities for more privacy focused things.
* JB: Speaking for myself. The inventive for security researchers is to produce novel research. When you look at it and compare it to what users actually experience you see that problems for users are things from many years past. We need to protect our users and focus on what matters for them. Does DrawnApart make the top of the list of problems that WebGPU needs to address?
    * JB: Also in the larger context of browsers, if we spend the engineering to mitigate something that can be done in 20 other ways in the browser, does it matter? We have the obligation to put this in perspective. If it can be done on CPUs, doesn't make sense to look at this.
    * MM: Looks like the first try at a new area of research. Maybe the area is larger than just this paper.
    * KG: Both can agree: need to put in perspective but still take it seriously and evaluate what this means for us. Actually need to do that before we can prioritize it.
* AB: How is what they are doing different than other microbenchmarks? The ML engine seems to remove noise. If we have enough benchmarking, why is this fingerprinting?
    * KG: The main difference is like keys for locks have multiple pins. One pin is 1/10 whereas more pins require more data.
    * AB: They seem to have written a single microbenchmark that's good. If I have 10 microbenchmarks then that's not fingerprinting? Once you can gather information you can get information, and we can't stop all information.
    * KG: Joins JB's point. We accepted that browsers are leaking information. We need to understand the impact to see what to do.
    * MM: Would like to note that the more we talk philosophy, the less we're going to make progress. Not useful to discuss the general ideas of privacy and information.
    * KR: One thing the paper does is take advantage of the deterministic assignment of work to EUs. Since there are about 12 EUs, they are able to amplify the signal better. They didn't try to do the same with fragment shader (maybe they tried, maybe they found that fragment is more highly scattered). If you had a broader set of benchmarks you could get a set a signals to amplify with ML. The concern is that if you can have a compute shader that does contention on a mutex and it gives you a good fingerprint, that's more scary.
* DC: KG + KR, about webgl2-compute. Looking at the specification it says it has been obsolete in favor of WebGPU.
    * KG+KR: It's dead. Intel did a great prototype but we decided to not pursue it.

Should be easy:



* Why are bind group layout fields spelled differently from their WGSL counterparts? [#2469](https://github.com/gpuweb/gpuweb/issues/2469)
    * KG: Not against renaming to the "x32" variants, unless we have f16 in the future.
    * KN: Don't know about the future, but don't care about the width. So maybe "int".
    * DM: The are different set of types, plus the width.
    * **Consensus: keep what we have** 
* Sparse colorAttachments in a render pass [#1250](https://github.com/gpuweb/gpuweb/issues/1250)
    * **Consensus take it.**
* Feature request: ignore shader writes to color attachments [#2060](https://github.com/gpuweb/gpuweb/issues/2060)
    * Render pipeline with more attachments is valid to work with render pass with less attachments
    * Wrote test - works in all APIs with debug layers, all APIs seem to have features for this
    * DM: How does this work differently than pipeline with write mask of 0?
        * CW: Only compatible with render passes with 2 attachments.
        * Pipelines and render passes have to match exactly.
        * Decouples set of things written by shader, from things that pipeline expects.
    * DM: **OK, let's go with it.**
    * JB: this won't prevent catching errors in states?
    * KG: mistake could be equivalent to what you intended to do.
    * JN: D3D12 validation layers have a warning about this.
    * MM: warnings aren't part of the spec, so sounds good.
* rgba8unorm canvas textures and IOSurface [#2535](https://github.com/gpuweb/gpuweb/issues/2535)
    * CW: Chrome-specific issue?
    * KN: don't think so. Safari also uses IOSurfaces heavily. I don't understand the restrictions, and think they're undocumented.
    * KN: understanding - you can't get an RGBA texture from IOSurface in OpenGL on macOS. Don't know whether this restriction extends to Metal. Can create RGBA IOSurfaces though. Experimented with getting Metal textures out of them - was able to freely reinterpret between BGRA/RGBA at that point.
    * KN: maybe not as many restrictions here as thought originally.
    * KN: don't think there's documentation.
    * MM: there is [documentation on MTLDrawable](https://developer.apple.com/documentation/quartzcore/cametallayer/1478155-pixelformat?language=objc). It says, don't ask for RGBA. Remember trying it recently, didn't work. More detail, I can get back to you.
    * KN: if you can post doc page on issue, that'd be helpful.
    * MM: Pixel format field on CAMetalLayer doesn't have RGBA formats.
    * CW: spec mandates that browsers support the list of formats. There's also the preferred format. Devs should use that to figure out what the format is.
    * CW: they may see that BGRA is the preferred one on desktop OSs; maybe fine to keep RGBA, and we do a blit on Mac if we can't do otherwise.
    * MM: think that'd be a mistake.
    * CW: I say this because there are other OSs - Android's RGBA8Unorm. Would have to do the inverse here. Best to drive people toward that so they have the best performance on all OSs.
    * MM: there are 0 formats that work on every OS without a blit?
    * CW: yes.
    * KN: unclear whether requires blit. Compositor may be able to read out of texture if different format. But can't use overlays - can't scanout from texture.
    * MM: **can Google post a link to Android documentation later on the issue?**
    * **KN: yes, will have to look it up.**
    * CW: talk with Shannon Woods.
    * JN: [https://developer.android.com/ndk/reference/group/a-hardware-buffer](https://developer.android.com/ndk/reference/group/a-hardware-buffer)

Rely on canvas size instead of allowing size to be configured [#2416](https://github.com/gpuweb/gpuweb/pull/2416)



* Handling out-of-memory when resizing canvas backing store [#2520](https://github.com/gpuweb/gpuweb/issues/2520)
* BJ: previously, all allocation done after context creation, including the size. Concerns from folks who maintain canvas in the browser that it breaks the pattern of other canvas-based APIs. They prefer for us to stick with what's already there, like in WebGL the width/height of canvas determine size of drawing buffer.
* BJ: DM talked with canvas folks at Firefox - use the attributes parameter of getContext. We use this for antialiasing, etc. Then have sizing be based on width/height.
* BJ: if we go with getContextAttributes - would immutably bind a context to a specific device, format, texture usage. Don't know whether that's detrimental. Less configurable. If you want single canvas and want to rapidly swap out devices that point to it, not the way to go. I hear usually that people want to use a single device and broadcast to multiple canvases.
* BJ: also doesn't have the same overhead as WebGL contexts - destroy context, destroys all resources. Maybe not bad if devs have to …
* KN: use case of different devices with same canvas - context loss. WebGL requires new device object. WebGPU restores on same device. Can reuse same canvas - not a blocker.
* BJ: # resources per canvas - probably not large. Not expensive operation. Less user convenient, but maybe consistency with web platform's more ergonomic.
* KN: concerned with cost. Happens on device lost - will have to do resource reinit. Replacing canvas - have to redo DOM layout.
* DM: do we have to replace it? GPU canvas context != device child. A thing of itself, can return the same thing each time. But can't ignore canvas context attributes, including device.
* BJ: should have mentioned earlier. getContext() earlier - once you get a context with a particular name ('webgl', '2d', etc.) the next time you pass something in with the same name, even if attributes are different, you have to get the same context back without changing the attributes.
* BJ: this is why this immutably ties us to a specific device. Once attributes set, you don't get a different one back for the same canvas. Only thing you can change are the dimensions.
* KG: there's a desire to "close" a canvas and reset it to its original state. 2D canvas, now want WebGL context against it. Haven't heard opposition to it, just nobody's proposed it. Another way to satisfy this. Esp. for WebGPU. If you could reset canvas and create a new thing with different attributes - different answer.
* BJ: where's that conversation taking place?
* KG: canvas spec, there's an issue there. Can try to find the issue.
* BJ: would like to link from notes.
* BJ: that's one option. The thing that'd make the canvas folks happiest. There's another spectrum of options - leave as is, stop using canvas attributes as defaults, allow reconfiguration of devices/formats but only rely on canvas size, etc.
* MM: some of the ideas here are dramatically changing WebGPU context behavior. Seems to me we should scope our discussion to the size attribute.
* BJ: think that's a viable option. If minimizing changes on our side - great. Just noting we've had different conversations with different people.
* MM: there's an optional size attribute. Does that pick up the canvas's size if not specified?
* BJ: yes, at the time of configure().
* MM: so they're asking - you have a canvas, you change its size, and WebGPU should automatically return textures of the new size.
* BJ: yes. We changed it because there were concerns about timing. Maybe no longer concerned about the timing.
* MM: sounds like reasonable request. Sentinel value in size field would be natural solution.
* 
* 
* 
* 
* 
* 
* 
* 
* 
* 
* 

Streaming implementations and indirect draws/dispatches [#2189](https://github.com/gpuweb/gpuweb/issues/2189)

(finally resolve?) texture_2d_depth for loading depth values might be unnecessary? [#2094](https://github.com/gpuweb/gpuweb/issues/2094)

Cannot upload/download to UMA storage buffer without an unnecessary copy and unnecessary memory use [#2388](https://github.com/gpuweb/gpuweb/issues/2388)

Expose WGSL errors in the API [#2308](https://github.com/gpuweb/gpuweb/issues/2308)

Consider removing GPUPipelineLayout [#2550](https://github.com/gpuweb/gpuweb/issues/2550)

Limit for the maximum buffer size [#1371](https://github.com/gpuweb/gpuweb/issues/1371)

mapSync on Workers - and possibly on the main thread [#2217](https://github.com/gpuweb/gpuweb/issues/2217)


---


# Day 2b (Wednesday Feb09 4-5pm, APAC)


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Myles C. Maxfield**
    * Robin Morisset
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
    * **Brandon Jones**
    * Corentin Wallez
    * **David Neto**
    * Ekaterina Ignasheva
    * **Kai Ninomiya**
    * James Darpinian
    * James Price
    * Rahul Garg
    * Ryan Harrison
    * Sarah Mashayekhi
    * Jaebaek Seo
* Intel
    * **Hao Li**
    * **Jiajia Qin**
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * **Shaobo Yan**
    * **Yang Gu**
    * **Yunchao He**
    * **Zhaoming Jiang**
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * **Dzmitry Malyshau**
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
* **Dominic Cerisano**
* Eduardo H.P. Souza
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* **Michael Shannon**
* Pelle Johnsen
* Timo de Kort
* Tyler Larson


### FP16 Feature



* [Slides](https://docs.google.com/presentation/d/1PyF0PnKW7zcFWQSsa5p9n8Sp6tRBP-7OQp6SljnYyhU/edit)
* (Intel)
    * Presentation discusses when the emulation can be high performance. Writing is the one that can slow down a lot when emulated.
        * Overview of Vulkan feature bits, and how well they are supported in the field
        * Metal supports f16 everywhere
        * D3D12 requires DXIL support.
* ZJ: When should the extension be published?
* AB: Happy to include it in the spec now, as non-required feature. 
* AB: Question: Are there changes required from the API side?
* ZJ: No API changes required, except to enable the extension.
* MM: Should we add half-precision types to the Vertex-type enum
* KN: Why is that relevant?
* MM: If people want to use those in a vertex buffer.
* ZJ: Vertex buffer attributes already include half-precision, but the values will be converted automatically to 32-bit floats when feeding the pipeline inputs.
* MM: I was thinking of a case where an application uses a compute shader to write the vertex buffer.  In that situation, the graphics pipeline needs to understand that each scalar value is only 2 bytes, so it can cause the translation to occur as required.
* KN: the half-float vertex format already exists in the API.
* KN: It’s an entirely different feature to do 32bit-format-to-f16 or 16bit-format-to-f32 format.
* KN: The vertex formats are already whatever is supported by underlying APIs.  There’s no dependence on shader features’ ability to write 16bit values from a shader as they don’t care if you write the vertex buffer by compute or from the CPU.
* KG: Should we anticipate features building on top of each other. So we don’t have to change this extension later.
* KN: Right  Might be able to get performance by avoiding a conversion from 16-to-32 bit on pipeline input boundary. Came up recently that *maybe* this is possible for textures too ([#2469](https://github.com/gpuweb/gpuweb/issues/2469)/[#2561](https://github.com/gpuweb/gpuweb/pull/2561)), as well as vertex attributes.
* KG: Some GL drivers (Apple?) does require annotating texture samplers as highp in order to get f32s out of them, since samplers technically default to lowp. This doesn’t give strong indication on our modern APIs, but there is some prior art here.
* AB: Vulkan requires sample type to be 32-bit components. Did exploration with 16bit format data, and it was hit or miss as to whether you would get performance wins.
* DM: Does that apply to storage textures?
* AB: Any sampled type is 32-bits
* DN: And when you got the value (from texture sample or texture load) it would always have 32-bit components.
* MM: Should determine if it is any faster on any hardware.
* DM: If it’s supported on some API, we can polyfill on the others.
* MM: The polyfill is easy. But if you offer something developers have to know whether to use it. If it’s faster in some places but not others, then they don’t know whether to use it. The performance characteristics shapes the feature offered.
* KG: It would be backward compatible if we added it later.  Don’t want to get into situation by having extensions on top of other extensions.
* GR: Its an optional extension. Don’t tie it to a v1 timeline.  It comes out when ready.
* MM: It’s important, let’s not wait months to do this.
* DN: To me it’s human time allocation. As long as someone is pushing it forward, that’s great. We’ll do reviews, but our teams are bandwidth limited. No intent to block or ignore.
* DM: Where should the list of investigations be listed.
* ZJ: I will update the GitHub issue.
* KN: Regarding v1. No opinion. Want to clarify that our optional features are not written in separate documents.  They would be included in the v1 version of the doc.  But it’s a living spec, so the “1.0” release is not important.  I do think it’s ok to add stuff *to* optional features in the spec over time, as long as it’s feature-detectable. Doesn’t have to be yet another optional feature bit. Can be feature detected by a different mechanism.
* AB: Agree much with Kai.  We don’t strictly have versions. V1 is a concept we’re using to drive conversion.
* MM: Question about Vulkan bits. storageBuffer16bitAccess, uniformAndStorageBuffer16bitAccess. Is one a superset of the other.
* AB: Yes.
* DN: The uniformAndStorge .. encompasses the other one.
* AB: Vulkan extension, when enabled, requires storageBuffer16BitAccess to be supported.
* MM: There are just a few devices in the diagram, there are some devcies that can do 16bit shader arithmetic but not do 16bit storage buffer access.
* KN: Few device models, but those models are really popular. Likely many devices in the wild.
* AB: My Pixel 3 (Adreno 630) is in that slice. I’m the reason that slice exists at all.
    * Pixel 4 (Adreno 640) too. Pixel 5 is OK (Adreno 620).
* MM: One option is to cut the feature in half:  do fp16 ALU in core, and put the storagebuffer access as the optional part.  Then you can get the ALU speed everywhere.
* ZJ: The diagram shows that if a device supports fp16 ALU, then it very likely also supports storageBuffer16BitAccess.  In some use cases, like TF.js the storage buffer feature is very important.
* MM: the converse, if there is a device that does not support writing to storage buffer, then it probably does not support ALU, and so that device probably won’t support fp16 ALU.
* KG: How important is it to group this.
* MM: Ideally we would never have to split.  But fp16 is good. Want to give authors the ability to use it.
* KG: I’m slightly frowny face about having two extensions.  
* DN: I dislike having a split-functioality feature.  I suspect it’s likely that the devices that are in the “benefit from one and not the other” may be aging out (their OS security updates may be ending soon enough). I also would love to see market split across NorthAmerica, EMEA, APAC for mid-range and above models.  I think we can drum up that data, but have not yet done so.
* MM: There is at least one option on the table where there is a core feature, so we should not delay getting the info we need to make this decision.
* MM: Suggest next step is to make a draft PR, and many of us will review it.


### Unicode Identifiers in WGSL



* The issue is [#1160](https://github.com/gpuweb/gpuweb/issues/1160)
* Important comments from the issue besides the OP:
    * Normalization in Web and other languages: [https://github.com/gpuweb/gpuweb/issues/1160#issuecomment-1032812727](https://github.com/gpuweb/gpuweb/issues/1160#issuecomment-1032812727)
    * State of Unicode libraries where people might do WGSL tooling: [https://github.com/gpuweb/gpuweb/issues/1160#issuecomment-1033063977](https://github.com/gpuweb/gpuweb/issues/1160#issuecomment-1033063977)
    * The four ways to go forward: [https://github.com/gpuweb/gpuweb/issues/1160#issuecomment-1034274427](https://github.com/gpuweb/gpuweb/issues/1160#issuecomment-1034274427)
    * Just a note that it might be rude to call some sequences of codepoints looking different to you with offensive adjectives like weird, and certain layouts not well are problems of layout engines: [https://github.com/gpuweb/gpuweb/issues/1160#issuecomment-1032044417](https://github.com/gpuweb/gpuweb/issues/1160#issuecomment-1032044417) 
* MM: Want to motivate normalization.  First example. There is a character in unicode which is e-with-accent, another which is e, another which is combining-form-of-accent.  Conceptually the first is equivalent (from seeing the glyphs) to the last two as a combine pair.  Normalization processes the data so that equality can be performed.  Why does an implementation care? Trouble is single files are edited by teams, so you may have different parts of the same file recording the same visual thing in different ways.  And ipad keyboards emit different sequences for those things.
* KG: And the intent is that those two different utterances
* MOD: Think normalization is good, but question is whether implementations will support it.
* AB: Two questions: unicode identifiers, and second whether to do normalization. Don’t have technical input.  For sake of users, we should support unicode identifiers in the core language.
* KG: My weak vote behind 3 or 4.  Our spec should follow other web specs.  Will be used alongside/with JavaScript.  Unfortunately JS doesn’t implement normalization.
* MM: Filesystems do not do normalization.
* MOD: WebAssembly does not do normalization. But think normalization should be good.
* MM: Should I run a performance measurement?
* KG: Think CPU performance is not the main issue.  The higher question is whether to integrate with ICU. All browsers do, but it’s still a cost.  Cost goes on to middleware too.
* KN: We get the shader source all at once, and can be done in the browser. Doesn’t even have to happen in the compiler stack. Browser can pass down normalized data. So don’t have to worry about linking ICU into Tint. That makes it a lot easier.
* MOD: I can try spec’ing option 4.  Then see if normalization makes it hard, and can reduce-it.
* AB: Based on what Kai was saying, where do you spec normalization occurs.  Is it only identifiers, or the whole text.
* KN: Can spec it in the API as “this field gets unicode normalization before being considered as WGSL source”.
* MM: Error messages will have string locations. If you go with a string, then you may have to translate the locations back, or require the JS code normalize the string again to line up with the reported locations.
* MOD: In language specs I’ve seen, it’s specified on both API and language side.
* KG: String.prototype.normalize() does what you want. [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/normalize](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/normalize) 
* MM: It’s slightly user-hostile to have to tell users that the locations only refer to normalized text so they have to normalize their text to see the right locations.
* KG: That encourages best practice. Normalize your text.
* MOD: Just NFC normalization.
* MM: That’s fine. Do whatever JS does Use the same default as JS.
* MOD: JS default is NFC.


---


# 📆 Next Meeting Agenda



* Traditionally, the meetings in the week after an F2F are cancelled.
    * Any objections?
    * CW: we have a lot of things to get through, so let's meet next week. (No objections by the group.)
    * KG: think we can take a break on the WGSL side. **Let's skip the WGSL meeting, but have the API meeting. Still WGSL office hours though.**
* CW: couple of things on critical path
    * PING review
    * Horizontal reviews - we should make progress on them. Security, INTL should be easy to get. **Please help there.**
* Adding review of explainer of PING for next week's agenda?
    * MM:** I'll put myself on the hook for putting up my PRs against the adapter naming discussion later this week.**
* At the next APAC meeting: Updates on float16 and any further discussions that come up
    * 
    * 
    * 
    * 
    * 
* 