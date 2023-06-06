# WGSL 2023-05-30 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-05-23 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1K2i2Hhgxxj4m-aGpssAy2J368KiW5BW8svNg5FXR6P0) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * Mike Wyrzykowski 
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
    * **Rafael Cintron**
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



* Kelsey and Corentin investigating getting a CR done


---


# ⏳ Timeboxes (until XX:15)


### [Allow mutable function parameters with the mut keyword. · Issue #4113](https://github.com/gpuweb/gpuweb/issues/4113)



* KG
* DN: I personally think it’s fine, but internally we have different opinions.
* DN: It’s tempting to say “just do `var foo = foo`” but we have shadowing rules that forbid this.
* DN: ?? said “don’t use mut use var” (JB: +1)
* DN: Also can’t do pointers, because they aren’t mutable anyways.
* JB: Fine with anything here. `mut` is fine, `var` is fine, also fine with permitting local shadowing of params. First preference is leave as is
* MM: Confused why we don’t want to do anything. I’m not against inaction, but not sure why we’re choosing it. 
* KG: Someone coming to us with a desire, even if implementable, doesn’t compel us to implement it. There’s also shading. Strong preference to do nothing now, and maybe revisit later.  Also could be a preference to resolve on “you already have a way to do this, especially if we do allow you to locally shadow params.”
* MM: Do other languages allow you to shadow params?
* KG: There are often different rules between param shadowing and local to local shadowing.
* KG: I love being able to shadow one thing with another of the same name but with different attributes like const-ness.
* JB: Rust does this all the time. It’s useful and helpful. Val redeclaration The object is not a new thing, it’s the same object, and tweaking the non-value characteristics of it.
* MM:Swift has something similar.  For optional arguments.  
    * guard let foo = foo else {return}. 
    *  It’s kinda similar in making new variable that shadows the other one, but another way to look at it is to say this is just removing the optionality.
* MM: Not gonna fight for this.
* KG: **Consensus is no change now.  But have couple of open choices if we want to make this more ergonomic.  Revisit later**.
* JB: Reason I’m not hot on doing this is because of workload.  Not being an advocate for user preference.
* MM: Would like to say: **May be a good idea, but not high enough priority for right now**.
* DS: Did we mention the possibility of shadowing parameters?
* (Heads nodding no)
* KG: Would still want to punt that into the “not now” category.
* JB: Personally **would like that feature, but not right now**.


### [Add dereference operator · Issue #4114](https://github.com/gpuweb/gpuweb/issues/4114)



* JB: Both Go and Rust and Java treat . as dereference-then-get-member. That would be totally fine.
* BC: “Dereference” doesn’t perform the memory access. Changes from ptr to ref.  
* JB: So the foo.blah has type reference-to-the-blah-type.
* DN: Fine with me.  Should do something in this space, experience after writing samples.
* BC: Alan said this lessens the “need” for reference type, it seems.
* AB: One difference would be, if you called variable pointer by default, then you don’t always need a star to dereference it.  I’m on the fence as to whether this is a reference or not.
* MM: Have concern overloading dot operator.  Not aware of languages that have both pointers and non-pointers.  Don’t have a pointer type by itself.
* JB: C# does that.
* BC: Go has pointer types, almost same syntax as C.  And dot yields what we’re discussing.
* JB: Think Go, Rust and C# are the good analogies. They handle this just fine.
* MM: Swift has function on a pointer type of (?) and memory rebound.
* JB: Rust does this a lot.  When dealing with pointers it’s so commonly done it’s reserved for doing the dereference.  If you want a function to acton a pointer, it has to be a free function, not a member function ont he pointer type.
* KG: Sounds rough consensus for doing the . as described?
* DN: “language extension”.
* BC: Part of a v2?
* AB: Well, [worth considering that the] main use we have for pointers is to create a short name. This won’t work for that; still need to throw in an ampersand. Having it return a pointer, would obviate needing to put in a & to get the pointer back. Seems like a too-quick decision
* JB: Not clear about the cases you’re concerned about. Please put them into the issue.
* MM: sounds like if I want to make a short name.
    * A.b.c.d.e; can make a short name for that. But then if I used a pointer ot make a short name, I still need to use & and *.  But sounds like you’re still wanting to make C++ references.
* Alan Baker
    * let a = &x.y.z;  // a is  pointer.
    * let b = a.c;  // To make  ‘b’ a pointer, would need to throw in an & to get a pointer back.
* JB: In C++, if you use arrow operator, you’d have to use & to get it back to a pointer.
* KG: This reads naturally. B is a value, not a pointer.
* MM: in this example, to make a value,   let b = a->c;  Type of b is value.
* BC: Existing Go
    * [https://go.dev/play/p/Uq5SC1LioPX](https://go.dev/play/p/Uq5SC1LioPX) 
    * // You can edit this code!
    * // Click here and start typing.
    * package main
    * 
    * import "fmt"
    * 
    * type C struct {
    * 	i int
    * }
    * 
    * type B struct {
    * 	c C
    * }
    * type A struct {
    * 	b B
    * }
    * 
    * func main() {
    * 	a := A{}
    * 
    * 	p_a := &a  // a pointer value.
    * 
    * 	p_b := &p_a.b   // yup, this is what Alan is talking about.  Is a pointer value.
    * 
    * 	fmt.Println(a, p_a, p_b)
    * 
    * }
    * 
* AB: Some time ago David proposed a symmetry of ptr   . and [] to yield pointers.  I personally prefer that over this.  That’s closer to what I’m saying now.  I like the symmetry, but can read the room that I’m in the minority.
* BC: It’s going against the trend of existing languages, and doesn’t address the change of having to put * all over the place.
* ..
* AB: I’d like to encourage people to stay in pointer land, to delay the memory access. It’s cheaper generally to stay operating on addresses.  You normally rely on on underlying compilers to clean it up for you. Think it’s beneficial to encourage that in the source language.
* JB: Please put examples in the issue.
* MM: **Think Alan’s argument is worthy writing in the issue.**
* AB: Will add it.


### [Add discussion about fixed size array limits #4143](https://github.com/gpuweb/gpuweb/issues/4143)



* AB: We have some limits for fixed size arrays in the WGSL spec. C**an add those into the issue in case OP doesn’t know**.
* DN: Can’t be 100% prescriptive.  Too many factors contributing.
* MM: Be helpful to authors when we can.


### [fract() returns values out of bounds on Intel Iris Graphics 550 #4144](https://github.com/gpuweb/gpuweb/issues/4144)



* DN: **We’re still looking internally, but we do strongly suspect this is a bug that we will work around**.
* 


---


# ⚖️ Discussions


### [Shader extensions are redundant · Issue #3961](https://github.com/gpuweb/gpuweb/issues/3961)



* KG: Recommend folks check how much you care about this issue.
* MM: **Want Mike on the call to discuss this when done in detail**.


### [Should there be a way to prevent loop unrolling? · Issue #4110](https://github.com/gpuweb/gpuweb/issues/4110)



* MM: Thinking about Kelsey’s argument that folks get value out of it for a long time.  I’m no longer on any side of the issue. 
* JB: Not objecting any more.  Would have to object to “inline” as well.
* KG: Consensus: wish we didn’t have to do this. Might be useful. If we had it, it’s not that costly, but unhappy to add it.  Prefer to not add hints that we may often want to ignore.
* AB: There’s HLSL support, and in SPIR-V. Is there anything in Metal?  It’s firmly a hint because we can’t even translate it to one of your platforms. Is that a good candidate for being in a core lang.  Feels even more wishywashy if can’t be applied everywhere.
* KG:Fundamentally it is a hint.  But people do have value from it.
* BC: Even if it doesn’t exist in MSL, Tint is gradually morphing into an optimizing compiler. We could implement it inside Tint.
* MM: Clang attributes do_unrolling does work in metal. Even if you write the attribute, doesn’t mean the backend compiler honours it.
    * [https://releases.llvm.org/4.0.0/tools/clang/docs/AttributeReference.html#pragma-unroll-pragma-nounroll](https://releases.llvm.org/4.0.0/tools/clang/docs/AttributeReference.html#pragma-unroll-pragma-nounroll) 
* AB: Makes me more comfortable that we can move the information along.
* MM: Seems we’d want both directions.
* KG: Feels “no_unroll” is a stronger need.
* AB: Think we add both.  Assume this is an attribute. Similar to diagnostic attribute in terms of where it can apply.
* [https://gpuweb.github.io/gpuweb/wgsl/#filterable-triggering-rules](https://gpuweb.github.io/gpuweb/wgsl/#filterable-triggering-rules)
* @unroll for (...) { }
* JB: Yes, reasonable to restrict to a subset of possible locations.
* KG: Why is ‘either’ a mistake?
* MM: I expect if you can put it in different places, it would be because it would imply that it does different things. 
* BC: In this case, the reason that both are available is that ‘before’ is that that would apply to e.g. the triple within the for () parens, and ‘after’ is just the block.
* JP: Should we allow unroll(n)? What would happen if it’s not supported by a backend?
* KG: E.g. impl might see unroll(2) and decide not to unroll, up to impl.
* ??: We should maybe defer for now and proceed with just @unroll and @no_unroll? 
* **Resolved: @unroll and @no_unroll (and not “@unroll(n)” for now - maybe later if necessary)**


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Pacific-timed**) Tuesday June 06, 2023, **4-5pm **Americas/Los_Angeles
* 

 
