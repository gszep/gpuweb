# WGSL 2022-09-06 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** ds

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** ****APAC!**** Tuesday **5-6pm **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-08-30 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1V2bu5Pr_dZKK--GAg6_bpFxYHS3HTtvZ8UpYC-Uzqec) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Daniel Glastonbury**
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
    * Ryan Harrison
* Intel
    * Hao Li
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
    * Greg Roth
    * Michael Dougherty
    * Rafael Cintron
    * **Tex Riddell**
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



* 


### TPAC



* TPAC is next week. We are not meeting face-to-face, but rather online on our usual schedule. (No special plans for TPAC)


### Charter expired (but don’t panic)



* WIP to renew it.  [https://github.com/gpuweb/admin/pull/18/files](https://github.com/gpuweb/admin/pull/18/files) 
* Causing some bot failures.
* Trying to get an extension for now.
* MOD: Some references to normative specs that are no longer relevant


---


# ⏳ Timeboxes (until 5:15)


### [Constant errors for integer expressions #3401](https://github.com/gpuweb/gpuweb/pull/3401)



* AB: Building on top of merged constant out of bounds errors. For any integer where non-standard behaviour (div by 0) or made safe for runtime are turned into errors so users can catch early. Runtime impl of clamp catches error if difference of behaviours.
* KG: Was on fence about that one, but convinced myself it's the right choice as it's ambiguous. Makes sense to me.
* KG: No objection? **We'll take this. Approved**
* JB: One question ,the division case rules out negative max int and the remainder case does not. What's the rational for that?
* AB: Was taking what was in the spec, possible we missed remainder previously, will look.
* KG:** AB will take a look**.
    * (In followup discussion, Alan pointed out the remainder operation handles that case, at line 5721 of the patch: [https://github.com/gpuweb/gpuweb/pull/3401/files#diff-709b5c343d59ae10cbabe69f9df1eee9e2dbb6e9d848632fa94357083d6e0a34R5721](https://github.com/gpuweb/gpuweb/pull/3401/files#diff-709b5c343d59ae10cbabe69f9df1eee9e2dbb6e9d848632fa94357083d6e0a34R5721) )


### [Inconsistent status of leading zeros #3415](https://github.com/gpuweb/gpuweb/issues/3415)



* MOD: Stems from float point integer case has limitation as integers but exponential case allows leading zeros. Either we change that to not allow or allow for everything. Looking for opinions
* KG: With AB. In particular, typing octal literals is usual a mistake, typing hex values are very useful to pad with zeros.
* DG: Confused, if we added octal we'd have a 0o prefix?
* KG: It's a potential, but don't want to discuss now, so no octals for v1
* DG: But wouldn't have leading zero for octals, so could allow them?
* KG: Could if we wanted too. Not trying to torpedo because I don't like octals, but one other concern in JS 0123 is 83. In the spirit of not being surprising to JS developers, leading zeros is something we want to tell folks to not use.
* MM: Is proposal to make leading zero a compile error, or make it work?
* KG: Believe it's a parse error currently
* MOD: Spec doesn't want leading zeros. But floating point with exponential doesn't reject.
* MM: So looking for consistency?
* KG: For exponential form you mean 10e3 form?
* MOD: Yes, 00010e3 is allowed.
* KG: Don't love it but don't care that much. Feels like general attitude is to not care about octals, but hex case we want leading zeros. Exponential case take or leave it. Inclined to forbid it as don't allow it in normal form case. Would be weird if we allowed it.
* DN: Would want to check what c and c++ do for floats for exponent and fraction parts. Agree with summary so far and think it's ok to have integers different from floats but want to make sure we're being consistent.
* MM: Checked JS 2e3 is 2000, 02e3 is error. 2e03 is 2000.
* DN: First character indicates octal but seeing e means exponent.
* MM: &lt;some things that are same result>
* MOD: Can you start floating point with zeros even with a dot inside.
* MM: 00.5 is parse error.
* DS: 0.5 as a parse error would be weird.
* JB: Reviewed regexes and patches to regexes, think they're fine and don't need to change. Please lets not implement this.
* MOD: Think we need to change regex to not permit leading zeros for floating point cases.
* JB: In the exponent of the floating point.
* KG: Happy to go towards JS here. Either PR to more closely match JS or to have a strong argument, not just symmetry.
* DN: Reading c++14, allows digit sequences, which allows zeros for fractional constant and exponent. So, we match c++14 for floats.  Section  [lex.fcon]
* KG: **If have specific things to change, add them specifically otherwise we'll close it.**
* MOD: **Will check and put up a PR if I find C++ to be different**.
* MM: Stated that humans have solved parsing literals and should take parsing rules from some other language. Innovating here isn't worth it.


### [Specification needs to clarify behavior of indexing of abstract composite type with runtime value #3210](https://github.com/gpuweb/gpuweb/issues/3210)



* KG: Feels editorial but technically a change? Recommendation from BC that we materialize the index before doing the index.
* BC: Don't think there is much else you can do.
* KG: Brain running away from what else you'd do. Resolved. If weird we can come back to it.
* MM: Trying to understand, vec where all components are const-expr and indexed with const-expr then result is const-expr. That's cool. Separate, have vector where components are const-expr and index with non-const-expr trying to determine that result is non-const expr?
* BC: Not about const-expr but about abstract. Can have vector of abstract and as written we don't materialize before the index. Given vector of abstract which isn't representable in backends you're dynamically executing on GPU but the thing indexing is non-representable. So have to materialize before the index as you can't emit the abstract.
* KG: Otherwise jumptable or something
* JB: Thought the spec said that anyway, so having it say that is fine with me.
* MM: Seems legit.
* KG: **We'll do that**.


### [wgsl: Add limits to avoid nuisance floating point literals · Issue #3255](https://github.com/gpuweb/gpuweb/issues/3255)



* DN: DS filed a thing and I was looking at hex floats. Was looking at what JS did it and said 20 digits and think that's fine. Think we should take what makes sense from other specs even if it's yy significant digits. Also applies to exponents and mantissa. There is interplay there. Just want to re-apply overall logic and think it gives solution here. If others are inline with that would be most sensible.
* KG: Is this actionable? Are we done waiting?
* DN: Writing down PR for decimal case this would be a quick follow-on.
* KG: **Still waiting then**.

OTHER TOPICS


### [Add precedence rules description into spec #3346](https://github.com/gpuweb/gpuweb/issues/3346)



* ZJ: Had internal discussion around operator precedence like (..) not sure what todo.
* DG: Spoke to MM about this this week. 
* DS: Issue is to add words about precedence. There are currently none.
* MM: What are concerns from JZ specifically.
* ZJ: We have different behaviour for & and | and bitwise logical and and or. Kind of confusing. We treat & for float and unsigned and the bool and vector fo bool say logical & but actually in grammar it's all treated identically. It's different from short circuit &&. However in spec we put short circuit && and logical & in one table while putting bitwise & for other table in bitwise expression. Kind of misleading. Maybe people will think for bool logical & and short circuit & will be similar but different from bitwise.
* DN: **Naming is in conflict, it's an editorial issue. Functions over bool are logical but grammar rules are named same**.
* ZJ: One idea is merge logical and with bitwise expressions of other types. Maybe we should figure out a ?? maybe we could call that bitwise  because we didn't' define bool bit patterns
* MM: Don't want to short circuit bitwise operations. Would be confusing if bitwis was all zero and we short circuited.
* ZJ: We split short circuit already. Vector of bool doesn't do short circuit. Only scalar bool can short circuit.
* DN: Concrete is to rename bitwise expression grammar rule to something better to hopefully clear up confusion. Are you OK with the structure that's there in that you can't mix addition with shift.
* KG: That was deliberate right
* DN: Yes. Just wanted to confirm.
* ZJ: Personally, appreciate making more clear but we have some existing code which just did a+b & c+d and when tint updated we had regression.
* KG: That's fair.
* ZJ: Maybe make more clear to have description in spec.
* KG: **Should find KN's image and put in spec**
* DS: It's linked from issue.
* [https://github.com/gpuweb/gpuweb/issues/1146#issuecomment-714721825](https://github.com/gpuweb/gpuweb/issues/1146#issuecomment-714721825)  ←  Kai’s diagram
* DG: That makes more sense then what read in spec. Should == and != operators be lower precedence then comparison &lt;, >, >=, etc.
* MM: So want a &lt; b == c &lt; d. Want a &lt; b and c &lt; b then then ==?
* DS: No, brackets
* DG: So, (a &lt; b) == (c &lt; d)
* MM: Context of previous resolution was it's a trade off worth it to improve clarity and reduce misunderstanding.
* DG: Trade off, other languages don't necessarily do this.
* ZJ: If we can put arbitrary operators together then we can compare precedence but we just can't put them together. \
KG: KNs thing has some groups and a tree of sets. Should both put the words in the spec can make sure it's what we implemented.
* MM: Sounds like there are at least 2 people who were surprised by this decision. Lets come back in a week and see if worth reopening.
* KG: **Recommend if we're surprised but not sad then we just need to explain better.**
* BC: Would like to add that people being surprised was due to Tints terrible errors. (No cookie for DS.)


---


# ⚖️ Discussions


### [Runtime behavior of NaN-producing math functions #3365](https://github.com/gpuweb/gpuweb/issues/3365)



* Previously
    * DS: Spec to write for compile time. Spec to write for half or narrow band domains. Table for next time single item ones discussion next time. Discuss sqrt or ones expected to be fast.
* AB: This was split, so are we talking both sides or runtime?
* MM: Had some compromise last week where we said if we could be more prescriptive about runtime then could treat those as compile errors if compiler can see literals.
* AB: Right so to firm that up, similar to int expressions do the same with floating point if we'd produce a nan or infinity call those errors. Disregarding speed questions could be descriptive at runtime and be portable safe. Unless we need speed on some of them. Think that would be a reasonable compromise. 
* MM: Think that's reasonable.
* JB: Didn't quite understand AB proposal
* AB: If const or override, producing nan or infinity is error. If runtime, and we don't' consider speed and you'd make one of those we'd prescribe the result. If it needs to be fast we can decide what to do .
* MM: Think that's the right thing to start with portability and then carve out for speed
* KG: Agree even though it makes me sad to lose division things
* JB: Did we decide to do fp division like int division?
* MM: What do you mean?
* JB: Int division by 0 is error, thought we'd agree fp div by 0 is not an error
* KG: saying here runtime allowed, compile time error if determinable.
* MM: Right
* AB: At least be able to detect where you'd have undesirable results. User would be warned away from non-portable behaviour
* JB: I'm OK with this. Keep thinking inf is OK but they aren't OK.
* KG: Want them to be OK, but makes sense to treat as not ok for now.
* AB: **Willing to take a look at that. Probably larger PR then int for floating point const expression**. Do we want portable for all, or do some need to be extra fast and we don't want to pay cost?
* KG: Right, this is the part about sqrt.
* MM: Think that kind of discussion should be data driven. Without data, start from portability. At point someone has data and says a common operation in corpus is %% slower we start relaxing things.
* AB: DNs table only covers Nan. If we want portable we need inf as well which makes it more tricky.
* KG: Other thing if we're saying portable first and work on loosening later is proposal. For example sqrt, does it give 0?
* MM: Reasonable. If AB has something in mind, good for him to propose otherwise I can make pass through library with proposed behaviours.
* AB: Do not as think infinity makes tricker. But maybe not thinking about them correctly. A lot of functions blowup at end and where is that in range.
* KG: Like when inv blows up at 0
* AB: Anything that's increasing at the end.
* JB: Any exp has a big tail where everything is infinity.
* **KG: MM if you could run though that would be cool.**
* **MM: Sure.**
* DN: Clamp has to handle inf as well, straight forward functions have edge cases for inf.
* KG: Concludes this topic for now. Talk about compile time or we're good?
* MM: Think we have a path forward.


### [Add a static alias analysis #3336](https://github.com/gpuweb/gpuweb/pull/3336) 



* (previously [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457))
* AB: Integrated feedback from DN and JB and converted to PR. Waiting on MM feedback.
* MM: Not prepared for feedback at this time. Another week?
* KG: Think that's OK.


### [wgsl: pointer-to-workgroup in user-defined functions · Issue #3276](https://github.com/gpuweb/gpuweb/issues/3276) 



* AB: Came up from TR comment a while back about inout in hlsl and pass shared variable if it isn't able to elide copies you can't rely on result as you don't know which invocation did inout copy. This is a what can you do and we have to take it out.
* MM: Questions, is the reason the compiler might not elide the copy-in/out cause data races not in the src program. 
* AB: Trying to remember how TR described, could imagine non-const access in array and have to do big case or jump table to figure out index to use. When that's the alternative it doesn't ellide and don't know which takes effect. If can't promote to register is one it could possibly fall down. Haven't thought through race. If it can't copy you always have a data race as everyone touches same memory for while workgroup.
* KG: These are concerns, are we saying because of this we  should remove ability to have pointer to workgroup and rely only on return values?
* AB: Right, or use specific global variable which is less helpful but still provides some ability.
* MM: Wish TR could comment and give examples for what should and should not work.
* KG: We can ask him, no promises. What's the plan then? Isn't that pretty annoying to not take pointer to workgroup?
* DN: TR did comment that he sees people writing code and it's a no-no. Advise internal folks to not do that. Looking at body of code viewed with skeptical eye in HLSL so not excluding that much.
* AB: Right because you can't do in hlsl or glsl. So large corpus where it doesn't work how you want
* KG: Makes sense. Fits in bucket of things folks shouldn't be doing and get rid of it and see who yells.
* MM: Think willing to accept, but not before someone from MSFT characterizes bounds of problem. What is important, why isn't.
* KG: We'll ask when they're here.
* ----
* Continuing
* ----
* MM: Question, we have been made scared of pointer to workgroup and hoping to characterize which situations it should work everywhere and which situations not confident about
* TR: Talking about generating HLSL inout as a result of ptr to workgroup, yes?
* MM: Yes
* TR: That's bad.
* MM: Can you tell us more?
* TR: It's bad because language definition says copy-in and -out on a per-thread basis. FXC and DXC in some cases, will eliminate the extra copy but not necessarily in all cases. Can't tell you a specific example of the line.
* MM: Confused, the name of the feature is inout so seems not totally surprising situations where copy in/out would occur. What's actual problem?
* TR: Would have to know you're only impacting certain elements on certain threads so if copying would have to know which thread local values need to be copied to which group shared elements on output.
* AB: The last invocation to copy would clobber all other work, that's disregarding if you have a data race and that's UB.
* TR: Right, it's undefined what you'd get. It's undefined value, not behaviour as it's a data race.
* MM: Trying to figure out where the race wouldn't occur with copy in/out but the demote to copy in/out would cause the race
* TR: It's copying into thread local values which the threads modify elements of full thing copied in and then on onputput copies thread local value to group shared elements which is a data race
* KG: And this is a copy because copy in/out is sometimes elided?
* TR: Problem when it _isnt_
* AB: Users would expect it to be elided. You'd split apart your memory and think only modifying some but on copy out it touches everything. Expected thread to just touch it's memory but in fact touches all on copy out
* TR: Right
* MM: So, say have array in thread group memory and have function taking inout of whole array and every array passes whole array to inout and then every thread clobbers whole array
* TR: Right that's the pattern seen. Function only modifies specific thread elements but that's the problem.
* KG: But if took pointer within the array
* TR: If you have pointers you can do it if you have the right pointer. You know you have a pointer to group shared memory.
* MM: In the program described above, surprised anyone would expect that to do what they wanted, passing whole array into inout which is telling you inout.
* TR: Yes, but FXC was clever and would compile to what you meant, not what you told it. That was a problem.
* JB: Data races are UB in HLSL right?
* TR: Yup.
* JB: So, compiler is saying program is free of data races so while passing this in no-one else is writing to it so can just passing in one element. So fi there is an issue then you made a mistake as you had a data race
* MM: Ah, so as the compiler if the program says copy out array to thread group then the only way that isn't a data race then there is only one thread so i assume one thread
* JB: Right. The whole thing is a datarace and will always be a data race but the language has rules and you broke them.
* AB: Also only works out when the compiler understand what piece of memory you're touching. Works in easy cases but not complicated.
* TR: you mean eliminating the copy?
* AB: Right.
* JB: Yea, if eliminating the copy was not sound then you'd have ub.
* TR: FXC, if you try to write to group shared, from something that isn't uniform, it will give a warning or error that DXC doesn't give. But funny it would do in normal case but passed as argument to a function, where modifying and not uniform it doesn't give an error.
* KG: One remaining thing that's a mystery is if this is all the case and we have to implement ptr to workgroup on HLSL is that just not possible?
* TR: Sure, it's just called inline the function. Don't know how else you'd do it.
* KG: Maybe static analysis?
* MM: Why does inlining solve this?
* TR: If you can inline and eliminate the copy then solvable.
* KG: If make pointer to third element and pass to function there is no way to do that in hlsl besides inlining and if name of thing to pointer replace with access.
* TR: Sinking pointer down into the function.
* AB: Helps in some cases, but practical cases.
* KG: Question for us, how much do we want to keep that around vs no pointers to workgroup
* DN: **Think we drop for V1**.
* BC: Cant' pass a pointer to uniform or storage either
* DN: **Think there is a PR**.
    * No, but I just made one: [https://github.com/gpuweb/gpuweb/pull/3422](https://github.com/gpuweb/gpuweb/pull/3422) 
* 


---


# 📆 Next Meeting Agenda Requests



* Next week: Tuesday, September 13, **11am-noon** (America/Los_Angeles)
* [https://github.com/gpuweb/gpuweb/pull/3403](https://github.com/gpuweb/gpuweb/pull/3403) 