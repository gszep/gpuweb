# WGSL 2022-04-05 Minutes (APAC)

**🪑 Chair:** Kelsey Gilbert

**⌨️ Scribes:** KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 5-6pm Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-03-29 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1Qjt_1oItfH9I0-EXN5S6jqp8HEm5f-s4-RdUo2MgnDc) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Myles C. Maxfield**
    * **Robin Morisset**
    * **Daniel Glastonbury**
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * James Price
    * Rahul Garg
    * Ryan Harrison
    * Sarah Mashayekhi
    * Jaebaek Seo
* Intel
    * Hao Li
    * **Jiajia Qin**
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * **Shaobo Yan**
    * **Yang Gu**
    * Yunchao He
    * **Zhaoming Jiang**
* Microsoft
    * Damyan Pepper
    * Greg Roth
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


### Daylight savings shifts are complete for now!



* I believe all daylight savings time changes have happened now, but this all happened between now and our previous APAC meeting, so people either may or may not see this meeting at a different time than our previous meeting.
* **We use Americas/Los_Angeles, and the Google Calendar invite should show correctly for you.**
* Just in case: [https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting+APAC&iso=20220405T17&p1=137](https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting+APAC&iso=20220405T17&p1=137) 


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * dsinclair@google.com


---


# ⏳ Timeboxes


### [wgsl: Allow increment/decrement statements in for_init #2721](https://github.com/gpuweb/gpuweb/pull/2721)



* **Ship it**


### [Add identity constructors for matrix types #2714](https://github.com/gpuweb/gpuweb/pull/2714)



* DN: **I’ll take Kelsey’s suggestion for less confusing wording, then merge**.
* MM: Do we have a function to construct identity matrix?  (No.)
* 


### [Proposal: Support DP4a as WGSL built-in functions #2677](https://github.com/gpuweb/gpuweb/issues/2677) 



* JS: For MM’s 1, we expected to only have this where hardware supports.
* For 2: It’s probably enough to have OpSDot and OpIAdd and have the compilers optimize this into dp4a calls.
* MM: There is a lot of precedent for builtin functions for things that don’t map to single instructions. E.g. reflect. As an implementor, I see no downside to seeing implementing this even if the underlying hardware doesn’t support it.  The interesting question for us is, what would the author do in response to the hardware being present or not.  They just call the function and it would work.
* JS: We would want to add it to have a performance gain. Polyfilling may be slow.  Those platforms may want to implement f32 or f16 models instead. If it doesn’t have performance gain,then it’s better to not have it. If they still want it they can implement it themselves in their own WGSL code.
* MM: You can’t stop an implementation from polyfiling it. It’s in an implementations’ interest to polyfill it.
* AB: In VK, it’s required in 1.3, and the property struct just tells you if it’s accelerated, but it always functions. **Given that’s how it works there, makes sense to go with MM’s suggestion to just polyfill where missing**. As for where we put them in, we’re supportive generally, but unsure if we care about them being in v1.
* MM: **Agreed, don’t want to hold up v1 for this**.
* JS: We didn’t expect the group to want to put it in core, so this all works for us.
* MM: AB’s telling of how VK does this is new to me, and I think that going that way is concerning to us.
* AB: Not proposing that we expose that “feature is hardware accelerated” bit.
* 


---


# ⚖️ Discussions


### [FP16 Extension against spec #2696](https://github.com/gpuweb/gpuweb/pull/2696) 



* Update:
    * [https://github.com/gpuweb/gpuweb/issues/2512#issuecomment-1082808969](https://github.com/gpuweb/gpuweb/issues/2512#issuecomment-1082808969) 
* ZJ: I have slides
* ZJ: Do we have any comments on the ranking changes?
* DN: We don’t have consensus on that approach yet, so let’s try not to block fp16 on that.
* MM: I can give more background. During office hour, there were concerns about the h suffix, and I remembered back to a previous call where we briefly discussed an extension idea to change the default float type. It sounds like that extension is likely to change our decision on h-suffix, but until someone proposes that extension, we should let the h-suffix extension stand.
* AB: I expect it is in good shape. **Have to review this version**.
* ZJ: We’re somewhat concerned that there aren’t many comments on the other parts of the PR
* AB: Sorry, planning to do a more detailed review, but from what I remember we’re mostly satisfied with the overall shape of this.
* AB: Given that we only have these live meetings once a month, are you **ok with offline comments**, or did you want another round of online comments?
* ZJ: Either is good.


### [Add creation-time expressions #2668](https://github.com/gpuweb/gpuweb/pull/2668)



* AB: Waiting on more reviews.  Keep on rebasing.
* KG:** I promise to review this week.**


### [WGSL parsing assumes context-aware tokenization #2717](https://github.com/gpuweb/gpuweb/issues/2717)



* DN posted PR: [https://github.com/gpuweb/gpuweb/pull/2735](https://github.com/gpuweb/gpuweb/pull/2735) 
    * LALR(1), with context-aware tokenization
    * Removes parsing ambiguity between function call and type-constructor expression by refactoring the grammar.
* DN: We’ve been relying on treesitter, that our grammar is unambiguous. However treesitter also goes beyond more limited parsing. Effectively it does have a way to address what we’ve called our layering violation here. So it implements the 2007 paper that limits what it can tokenize next from its valid lookahead table. Thus I feel this is a perfectly fine thing to do, so I’ve written this up explicitly in the spec. Also removed some other ambiguities.
* RM: I agree that having some context awareness is required. I’m wondering what you mean when you say “set of valid lookahead tokens”.
* DN: When you generate the LR states, and you compute the first set of non-terminals to be consumed, *taht* is the (valid) lookahead token. So you’re in a state with a bunch of items, and each item has a set of items that’s allowed to have a valid transition. Any token that would cause you to immediately go to an error, would not be part of the valid set.
* RM: A token that allows you to do one reduce would still be allowed?
* DN: You only reduce when you consume the token at the end. It’s hard to explain.
* RM: Trying to figure out how to implement this in a recursive-descent parser. Do I need to keep track of more state, or can I figure this our purely from known local state?
* DN: Not sure. I wondered, “do I need an LR parser in order to get these sets?” Not sure still.
* AB: consider:
    * let x : array&lt;i32, 4>= ...;
    * the ">=" would be tokenized as ">" and "=" instead.
* AB: Currently, this would be an invalid token to see there.
* AB: The real thing we want consensus on is “will everyone’s parser [be able to] do this”. E.g., will Naga do this?
* JB: I think maybe the code will be ugly, but like, big deal. Worth doing in our code, to save millions of users from having to do e.g. what c++ used to need.
* AB: DN did have another approach, fwiw.
* MM: I think the interesting question is exactly “is this worth having in the grammar”, and I’m not sure if we can evaluate without evaluating more holistically
* DS: This example is only a problem with constexpr, so this is not currently an issue. Is there a case where you would actually parse with the wrong token, but then comiple successfully but get the wrong answer.
* MM: I was previously more confident that splitting-vs-not-splitting wouldn’t lead to two valid parsings, but now I’m less sure.
* AB: I think with this new approach that we’re using with treesitter, means that we are safe from this.
* DN: Treesitter’s hack generally does what you want. 
* MM: So in the >= case, the >= would win over the >, so this would be an error?
* DN: Normally it would, but the context-awareness would realize it’s parsing a template arg list, so it would know it can’t use a >=.
* DS: Shift?
* AB: Shift is still ambiguous, so we do need e.g. parens there.
* JB: Do we know that we will not have ternary expressions here?
* AB: The grammar where you define the array here knows that it can’t have that. That’s why I explain that if we do add ternaries it could certainly cause problems.
* MM: I don’t think we should assume we won’t have ternaries.
* DN: I think you can fix this with multiple roots for the grammar for these, one with parens, one without.
* DS: Can we just force the constexpr tree to be parenthesized.
* MM: I think you want to say const a = 4+5, and that if we can’t say that, we’re doing users a disservice.
* MM example let x : array&lt;i32, 13 >> 2>;
* MM example: let x : array&lt;i32, 4>= 3 ? 2 : 1> = ...;
* JB: If the len expression of an array jumps into the middle of precedence levels, the precedence levels here are very low, we would already be required to paren expressions just in order to have it at all. By jumping into the middle to the precedence stack, we’re already requiring that we have parens around all the weird things.
* DN: I think I get the spirit about what you’re saying. I think if we try MM’s example, treesitter will say “there’s an ambiguity”, and I’d solve that by having different grammar roots used there. 
* MM: We’d like some time to make sure we don’t need to keep a global structure for where we are in our recursive-descent parser.
* JB: Naga is also RD, so while we want to solve this core issue for users, I agree that I want to make sure it’s possible
* DN: Tint is also RD :)
* **_Leave this for a week. Call to action: review your recursive descent parser to see _**


### [context-aware tokenization: Sometimes >> is better parsed as two copies of > (similar for ]] ) · Issue #2092](https://github.com/gpuweb/gpuweb/issues/2092)



* Offline:
    * Backlash against removing shift tokens.
    * David: Reconsider option 3, since #2717 shows we already rely on context-aware tokenization.  Think of a simple fix:   Require parentheses around any shift expression.
* MM: This is the same as #2717.
* DN: I don’t think we need more discussion here, since we have consensus on #2717.
* MM: What was the pushback?
* DN: Programmers really didn’t like having to use builtins instead of symbols.
* MM: Ah, the popularity argument :)
* DN: Since we have to fix tokenization anyway, let’s just fix that instead of switching builtins.
* DS: Should we revert it?
* KG: **Let’s keep it for now**, fine with having the revolt stew while we try to get the real fix.
* MOD: Would be nice not to thrash things while i18n review is happening.
* 


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, April 12, 11am-noon (America/Los_Angeles)
* [#2734 ](https://github.com/gpuweb/gpuweb/pull/2734) WGSL Internationalization Adjustment for Code Points
* [#2736](https://github.com/gpuweb/gpuweb/pull/2736) WGSL: Don't reserve words that are types in other languages
    * THIS LANDED
* [Add creation-time expressions #2668](https://github.com/gpuweb/gpuweb/pull/2668)
* [FP16 Extension against spec #2696](https://github.com/gpuweb/gpuweb/pull/2696)
    * David and Alan reviewed and approve