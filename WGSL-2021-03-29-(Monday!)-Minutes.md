# **WGSL 2021-03-29 (Monday!) Minutes**

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Monday noon-1pm (WebGPU API timeslot)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send Dan Sinclair a Google Apps enabled address and he'll add you.



---



# **📋 Attendance**

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



*   Apple
    *   Dean Jackson
    *   Fil Pizlo
    *   **Myles C. Maxfield**
    *   **Robin Morisset**
*   Google
    *   Corentin Wallez
    *   **David Neto**
    *   **Kai Ninomiya**
    *   James Darpinian
    *   Brandon Jones
    *   **Ryan Harrison**
    *   Sarah Mashayekhi
    *   **Ben Clayton**
    *   **Alan Baker**
    *   **James Price**
    *   Rahul Garg
    *   Ekaterina Ignasheva
*   Intel
    *   Yunchao He
    *   Narifumi Iwamoto
*   Microsoft
    *   Damyan Pepper
    *   **Greg Roth**
    *   Michael Dougherty
    *   **Rafael Cintron**
    *   Tex Riddell
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
    *   **Jim Blandy**
*   Kings Distributed Systems
    *   Daniel Desjardins
    *   **Hamada Gasmallah**
    *   Wes Garland
*   **Dominic Cerisano**
*   Joshua Groves
*   Kris & Paul Leathers
*   Lukasz Pasek
*   Matijs Toonen
*   **Mehmet Oguz Derin**
*   Pelle Johnsen
*   **Timo de Kort**
*   Tyler Larson
*   Eduardo H.P. Souza



---



# **⚖️ Agenda/Minutes**


## Meta


### Office Hour



*   **Wednesday 10am-10:50am**
*   **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
*   Everyone welcome
*   Mass calendar invite will have been sent out
    *   If you still need an invite, add your email here:
        *   [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)
        *   


## 

---



## Timebox


### [Support for vector splats (scalar operations on vectors) #1520](https://github.com/gpuweb/gpuweb/issues/1520)



*   E.g. `vec(x,y) * 2.0`
*   Previously:
*   JG: I’ll survey other shader languages to see what they have.
*   GR: I volunteer to get the answer for HLSL.
*   GR (later): HLSL accepts every arithmetic operation between vectors and scalars and matrices and ~~vectors~~ scalars for both DXC and FXC
*   GR: HLSL supports bitwise ops too in all scalar with sequential types
*   GR: and also the transposition of any and all operands
*   MM: Metal supports vector {*,/,+,-} scalar
*   MM: Metal supports scalar {*,/,+,-} vector
*   MM: Metal supports matrix {*,/} scalar
*   MM: Metal supports scalar * matrix
*   MM: Metal supports matrix * vector
*   MM: Metal supports vector * matrix
*   DN: on the issue, Mohammed found what GLSL supports.  [https://github.com/gpuweb/gpuweb/issues/1520#issuecomment-806004434](https://github.com/gpuweb/gpuweb/issues/1520#issuecomment-806004434) Pretty liberal mixing of scalar-vector and scalar-matrix, and  +,-,/ for matrix-matrix of same dimensions.
*   AB: GLSL also supports some bit-operations over these.
*   JG: I mainly care about arithmetic operations.
*   MM: Propose WGSL support the intersection of GLSL,HLSL,MSL
*   DM: When do we stop. What about shifts?
*   MM: Think the result falls out. See GLSL restrictions.
*   AB: GLSL supports i
*   DN: Small concern: Does this block us from having e.g. `1` being contextually 1i or 1.0f?
*   AB: GLSL does the conversion before splatting
*   MM: I think we have solutions here
*   GR: Do we want to include mat*scalar here?
*   MM: Probably, more for mult than div.
*   JG: Ok, I will write up proposal based on intersections


### [add syntax for very short const declarations inside a function #1552](https://github.com/gpuweb/gpuweb/issues/1552)



*   E.g. `x := expr;` ~= `const x = expr;`
*   AB: Why not just use ‘=’
*   MM: Think that causes more bugs than it helps.
*   _Probably not ‘=’_
*   JG: ‘const’ is longer than ‘let’.  ‘Const’ is 5 letters, ‘let’ is 3.  Don’t mind typing ‘let’ a lot, but ‘const’ is annoying.  But JS ‘let’ is mutable.
*   KN: := is arcane looking.  Looks incongruous with the rest of the syntax.  Symbols are harder to google.
*   AB: if you allow := here, would want := usable in the long const form.
*   GR: “let” is not that easy to search.
*   JB: Rust “let” or “JS let” yields meaningful results.
*   Brandon: Be nice to use let as long as syntactic divergence is not too bad.
*   MM: but then `let` being immutable is a difference from JS.
*   JG: That may be a tolerable difference.
*   MM: ‘let’ or ‘def’ would replace ‘const’.
*   KN: Our const is a lot more const-ier than JS.
*   MM: Webkit team ok with either const or let
*   DM: Main problem was writing the type all the time, can accept const or let.
*   


### [Describe location requirements #1519](https://github.com/gpuweb/gpuweb/pull/1519)



*   (Limit structures to 1-level deep only.)
*   MM: Question. Entry points have many arguments.  Is only one argument allowed to have these locations.
*   AB: They are allowed across many.
*   MM: Are builtins allowed among ordinary location-decorated?
*   AB: That’s allowed.
*   MM: That’s fine.
*   DM: Reason it’s on the agenda is restriction to 1-level depth.
*   MM: Allowing intermixing means it’s reasonable.  Otherwise may have had a different design.
*   _Resolved as approved._
*   _Merged._


### [Remove the void type #1460](https://github.com/gpuweb/gpuweb/pull/1460)



*   Deferred to tomorrow.


### [Add /* */ block comments. #1470](https://github.com/gpuweb/gpuweb/pull/1470)



*   JG: Motivation is commenting out large blocks of code.
*   MM: Also support block comments.
*   JB: I use an editor that helps and I still use block comments.
*   GR: Pain to parse, but l’m ambivalent.
*   DN: In the past I’ve seen incompatibilities.  Must have good testing for this to ensure consistency.
*   MM: What about nesting.
*   DN: Please no.
*   JB: Real purpose is commenting out code. Glad we don’t have a preprocessor; if we had preprocessor we’d use #if 0, which nests. So nesting is valuable.
*   MM: Value of block comments is to comment out code in O(1) effort.  But there are al
*   (nesting is complicated)
*   MM: Should we just try writing down the state machine for line-and-block comment nesting? I can do this
*   



---



## Discuss


### [Restrict where entry point IO types can be used #1526](https://github.com/gpuweb/gpuweb/issues/1526)



*   JP: We looked at a bunch of cases, concluded the best spot is to not restrict; ignore the locationIO/builtin when not used in that context.
*   MM: Sounds good.
*   DM: Feel like we should forbid structs from being host-sharable and for input/output
*   AB: Argument against that is we have default layouts to apply to host-shared.
*   BC: Rule is simple: interpret the annotation only when used in a meaningful context. 
*   MM: Consider comparing a type between a uniform and an input. In order to assign them, they need to be actually the same type.
*   DM: Ok that’s compelling
*   AB: Can’t actually do == on struct.
*   MM: Sure… will propose.
*   KN: can’t do == on struct in C++, unless you overload the equality operator.
*   MM: Ah, you’re right.
*   GR: Confirm HLSL can’t do == on structs.
*   [https://github.com/gpuweb/gpuweb/issues/1572](https://github.com/gpuweb/gpuweb/issues/1572) re equality on structs


### [Remove the requirement to have entry points #1340](https://github.com/gpuweb/gpuweb/pull/1340) 



*   DN: For the cases that you might not have an entry point, can always insert a dummy you’ll never call.
*   MM: Can lead to cargo-culting, where it’s always added even when not needed.  which would be unfortunate.
*   DN: Didn’t think there would be such strong feelings about it. Thought this was an opportunity to help the user when they forget the “stage” attribute.  If createShaderModule is given a module with no entry point, can always save a dummy empty module.  
*   JG: Give a warning if there’s no stage attribute.
*   _Consensus to remove the requirement._


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-03-30 (like normal)
*   Rename “value types” to “plain types”. [https://github.com/gpuweb/gpuweb/pull/1568](https://github.com/gpuweb/gpuweb/pull/1568)  
    *   Arguably it’s purely editorial.
*   ptr/ref:  [https://github.com/gpuweb/gpuweb/pull/1569](https://github.com/gpuweb/gpuweb/pull/1569) 
    *   Describes the consensus from [https://github.com/gpuweb/gpuweb/issues/1456](https://github.com/gpuweb/gpuweb/issues/1456) 
*   [Remove the void type #1460](https://github.com/gpuweb/gpuweb/pull/1460)
*   Maybe [Add /* */ block comments. #1470](https://github.com/gpuweb/gpuweb/pull/1470)
*   Maybe [https://github.com/gpuweb/gpuweb/pull/1571](https://github.com/gpuweb/gpuweb/pull/1571) 