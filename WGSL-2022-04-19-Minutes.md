# WGSL 2022-04-19 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday 11am-noon Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-04-12 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/14r1bOemITIhFnmMAHedc85NF3rXaLSqb9AApSIdT0R4) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Myles C. Maxfield**
    * Robin Morisset
    * Daniel Glastonbury
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **Dan Sinclair**
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
    * Rafael Cintron
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
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


---


# ⏳ Timeboxes


### [[editorial] Change vast majority of must language to link to shader-creation error #2746](https://github.com/gpuweb/gpuweb/pull/2746) 



* AB: Spec says “must” a lot and it’s almost always shader-creation error.  Link those to the “shader-creation-error”, and adjust the wording in the handful of other places. Nice to be explicit, and the link navigation makes it easier to go through all of them.
* DS: Find it nice to have the link.
* MM: Our definition of “must” is different from that RFC.  Our definition of “must” involves setting error states. That RFC has no concept of error states. Valuable to distinguish between the two.
* AB: There were a few places where it’s defining a contract that are to be fulfilled by the implementation. There were 3 instances.  I left them unlinked.  
* KG: If folks like, it take it.
* MM: Think this patch is good. Think the existing text describing “must” as specified by RFC is sufficient. 
* _Land when editorially ready._


### [[wgsl] Add literal examples; fixup f16 hex floats regex. #2761](https://github.com/gpuweb/gpuweb/pull/2761)



* DS: Adds a bunch of examples.  Treesitter will actually check them. THis allowed me to find a missing ‘h’ in the hex float regex
* _Ship it._


### [Consider allowing creation-time expressions or constants in switch case selectors #2764](https://github.com/gpuweb/gpuweb/issues/2764) 



* AB: Just missed this.  Will fix. And look for other cases.
* _Intent to make this change._


---


# ⚖️ Discussions


### [WGSL parsing assumes context-aware tokenization #2717](https://github.com/gpuweb/gpuweb/issues/2717)



* DN: Working on parser generator/analyzer. which can now determine LL1-ness, but WGSL is not. Not the intent to make it so.   Working on upgrading to LALR-table generation.


### [Pointers in uniformity analysis · Issue #2741](https://github.com/gpuweb/gpuweb/issues/2741)



* AB: When there’s a pointer parameter, should analysis be extra-conservative and pollute the call graph, or go more coarse grain and treat it as a return-value.  Think there is no big issue in performance when analyzing.   We could go either way here.
* MM: Don’t think we have an opinion.
* KG: Default desire is to add it to the pool of outputs. Like other returns. Outputs are returns and any other pointer params.
* MM: One day there will likely be an extension to allow pointers-to-pointers. How would that fit with this?
* AB: Depends on where you allow those pointers-to-pointers to exist.  The reason this doesn’t apply to workgroup and private is that those are always non-uniform because they are module-scope.  The only thing that could be uniform is function-scope.  Feel this would stay relatively contained. If we want to do something more broadly with module-scope variables, then consider that later.
* JB: Confident that once we start rejecting programs, we’ll make many adjustments.  Think we should work quickly to an implementation.
* AB: Don’t know what direction that is.
* JB: Smallest change that gets us to an implementation.
* DN: Thought both options were equally complicated, so leaned toward the more precise option.
* KG: Agree.
* MM: To Jim’s point about iterating.  It’s unlikely those source programs that will be tripped up will have pointers in them, so likely won’t have pushback on this particular issue.
* KG: Propose we try the precise analysis, and see if it works.
* JP: If we’re going with option 2, should have feedback this week.


### [[wgsl] Encoding for shader text needs to be specified · Issue #565](https://github.com/gpuweb/gpuweb/issues/565) 



* KG: The finder point is, are we defining a file format here. If so, it should be UTF-8.
* MOD: Internationalization feedback: If we say that UTF-8 is valid, then you have to give a method for determining the encoding of the file
* MM: Think we should not spend much time on this.  Think we shouldn’t have to go farther
* KG: Folks keep bothering us about this.
* MM: Weird that in-memory JS representation will be UTF-16.
* DN: My recollection:  We register text/wgsl MIME type, and in this spec say in WGSL “if you transfer this across a wire, use text/wgsl” and hence make it UTF-8.
* KG:  We are chartered to specify the string that enters create-shader-module.  Outside of that folks are on their own.
* MM: can separate what’s in a file vs. what’s given in the browser.
* KG: In part the i18n feedback is “looks like you’re specifying a file format, shouldn’t it be utf-8?”
* BC: Don’t want to say a file must be utf-8.  Expect tools
* MM: Say “should”.
* KG: I’ll propose a PR.


### [Clarify undefined behavior of built in functions #2765](https://github.com/gpuweb/gpuweb/issues/2765)



* KG: A big list.  Let’s make proposals offline.
* Atan2
    * DN: Is there value in defining a rule that are also bad asymptotically?
    * MM: Philosophy should be err on the side of “lets be portable until performance requirements prove otherwise”. Transcendentals are already very slow.  A few can go the other way, like clamp.  Another is inversesqrt.  Log2.  Cases are a little less clear there.
    * KG: Suggestion is use the same behaviours as JS.  
    * MM: Yes, for the ones where perf doesn’t matter, follow JS and WASM.
    * KG: Sounds like mostly spec needed.** I can make a PR**
* BC: When you say “dial it back” do you mean update the spec, or add an enable to provide alternate behaviour.
* MM: Either one.  Could follow what we decided for clamp.  Would only need an enable when adding new functionality or breaking old functionality.  (Even when adding new functionality, not clear).
* BC: Expect we would need an enable for turning off these polyfills that enforced define results for these edge cases.  Would need an enable to change those behaviours . Not opposed to having the enable.
* MM: Could think about adapting fast-math ideas here.


### [Add staticAssert #2759](https://github.com/gpuweb/gpuweb/pull/2759) 



* AB:  Defined it as a built in function without return value.  But DN pointed out you can’t have that at module scope. But wanted to use it there for sure. So make a new kind of statement, that can appear anywhere a global decl or statement.  And no parens.
* MM: Sounds good!
* KG: Yup.


## KG and DN plan to triage issues for closure.


## Annotating spec with &lt;assert>. 

BC: Dan found this in Bikeshed doc.  When bikeshed sees this, it will generate a hash id of the enclosed text. Can then tie that to tests to make sure all changes and items are covered. Dan plans to make a first PR to show what it looks like, and will request feedback.

KG: Sounds good, let’s see this.

MM: Would have to implement the tests before “shipping” the spec.  Then remove them?

DS: Intent is to leave them in the spec. Because that’s how you track if something in the spec has changed that needs to be updated in the CTS.

DS: It doesn’t show up visually in the HTML output.  It lets us tag the source in a way that survives as a span in the HTML (which is not visual).

DS:  Look for “&lt;assert>” in the bikeshed doc. Last para in a certain section.


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, April 26, 11am-noon (America/Los_Angeles)
* [#2774](https://github.com/gpuweb/gpuweb/pull/2774) Don’t reserve buffer, texture, in, out, input, output
* [#2773](https://github.com/gpuweb/gpuweb/pull/2773) Clarify conversions: floating, and out-of-range AbstractInt
* 