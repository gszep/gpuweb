# WGSL 2024-03-12 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** JB, DN, KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **4-5pm **Americas/Los_Angeles (**Pacific-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+is%3Aopen+milestone%3A%22Milestone+0%22), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+is%3Aopen+milestone%3A%22Milestone+1%22)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2024-02-20 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1QIzwHNVAk1rZeBu1XptpmEYzRO_M8NjLdKmr7CbBwV8) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send **dneto** a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * Mike Wyrzykowski 
    * Myles C. Maxfield
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * Alan Baker
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * dan sinclair
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
    * Zhaoming Jiang
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Microsoft
    * Damyan Pepper
    * Greg Roth
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


# 📢 Announcements/Meta


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
* 


---


# ⏳ Timeboxes (until XX:15)

None!


---


# ⚖️ Discussions


### [Feature request: textureQueryLod equivalent · Issue #4180](https://github.com/gpuweb/gpuweb/issues/4180) 



* DN: I was worried about the MIP LOD bias that the sampler can have in Vulkan and D3D12. But WebGPU does not have that. So it doesn’t matter whether the first component is with or without the bias: for us it will always be zero. So the proposal seems right. Metal may acquire the bias feature that Vulkan and D3D have, but for now we don’t need that complexity. Don’t feel like I understand this super-well. I’m not sure exactly what the HLSL functions actually do in the presence of the bias. The proposal seems to match the HLSL form, which makes sense if we’re only going to be using one or the other. If we get a biased version, it’d probably follow the HLSL biased versions.
* DN: **I’d like another week to check the math.**
* MOD: The definitions in the specs don’t mention the bias, it seems like they delegate that to other parts of the specification. So I thought we wouldn’t need to take that into account
* DN: In GLSL, it says, look at the lambda calculation in thus-and-such equation, return that. Vulkan has the same thing written out slightly differently, so it probably does end up being: compute the level of detail as the sampler would, but before truncating/clamping it. There may be clarifications needed.
* MOD: When you say “Vulkan”, do you mean the SPIR-V specification?
* DN: In the issue, I linked to the Vulkan LOD spec, and in there it explains how it calculates the value. It shows the various clampings, and truncations in the next section (“image level selection”). Functionally the SPIR-V instruction gets you the “lambda” from the first section and the level from the next section.
* MOD: In case we define this function, should they live in the API spec or the WGSL spec?
* DN: Ideally the WebGPU spec should explain how sampling occurs. The Vulkan spec has a good presentation to use as a model.
* KG: At Mozilla, we discussed whether it should be one function or two. It seems fine either way, because picking the wrong one is easily fixed by adding the other style later.
* **DEFERRED TO NEXT MEETING**


### [Use enum tokens when they are the only possible grammar usage. · Issue #4505](https://github.com/gpuweb/gpuweb/issues/4505) 



* (needs milestone)
* KG: This should wait until Dan can join us, since he filed the issue
* MOD: Concerned this issue brings up valid points about user declarations overriding the enumerants in the attribut parameters. I’m concerned this valid topic is presented in a way that could silently override the previous consensus about generalized attributes.  The F2F consensus was about shrinking the number of rules.
    * Substantive: behaviour change
    * Aliasing predeclared enumerate values
    * Generalized attribute
    * Human navigability of the spec.
* MOD: Suggest we split these issues to focus areas. 
* KG: Personally what matters most for me is, historically, my proposal was that we would allow identifier[-ish] tokens in this case. But the intent was not that they would incur shadowing behaviour. I call that out because I don’t want to operate only that “we had a previous consensus” because evidently we did not (because we understood different things).
* BC: regarding the idea of splitting things apart.  I’m opposed splitting them is because there’s a lot of interaction.  The original proposal made had knockon consequences.  We have to remember the interactions.
* MOD: Agree they play with each other. For the shadowing behaviour change, it could be done in declaration and scope section (add a semantic rule).
* DN: I think the two general wings of this are [editorial spec-reading] side and user-facing behavior side. Of these, the latter is more important.
* DN: [Willing to break backwards compat for e.g. extra parens like `@builtin(((position)))`]
* JB: I think it’s important to think of the template argument case separately from the attribute param case.  They have different constraints. For template arguments it’s fine to live with the template arguments. The urgent one is shadowing builtin position.  When we express an opinion we should be careful to say what we are talking about.
* MOD: Agree priority is the author experience of the language. Is it not the same for user experience whether these rules are set in static semantics for lexical scoping or syntax?
* DN: I don’t worry about how it’s expressed (as semantics words, vs. grammar).
* KG: Let’s be example driven.  See the examples and decide.
* **MOD: Let’s fork an issue** to focus on the issue of whether these builtin params should be shadowable or not.
* DN: I do want to keep the current behavior for template case.
* JB: I agree.


### [HW Clip Planes support · Issue #390](https://github.com/gpuweb/gpuweb/issues/390#issuecomment-1956104959)



* [From the API meeting](https://docs.google.com/document/d/1PLjCjOJM6K3SIh8CiiOQGrXk7BjfvR9XhRlTHX7mp3I/edit#heading=h.vnhxdq3d21ui): “Finish discussing the shape of the WGSL builtins in the WGSL meeting.” (Added by KN who probably won’t be here and doesn’t know needs to be discussed exactly. Feel free to punt if needed, but I think Jiawei wanted to keep working on this.)
* KG: HLSL exposes it as up to 2 vec4f, and others use arrays.
* KG: My taste is to make it arrays of floats.
* (JiaweS: Agreed with meeting emoji)
* (JB: Agreed with thumbs up)
* KG: Think the translation is relatively small, and affects (MSL?)
* KG: I propose an array of 8 floats.
* DN: Agree.
* BC: Agree. Prefer don’t baking in the two vectors.
* **_Consensus for array of floats._**
* DN: Need to make sure the output slots counting is clear.  E.g. that array-of-4 floats really only counts as one output slot.
* KG: What happens if you don’t write the outputs?  
* JS: If not written, then they are not clipped.  
* JB: Background question: by using these clipping planes we can chop up a triangle in more ways.
* DN: You’re adding more clipping half planes.
* JB: Never generates anything that’s concave?


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday March 26, 2023, **11am-noon** (America/Los_Angeles)
* [Feature request: textureQueryLod equivalent · Issue #4180](https://github.com/gpuweb/gpuweb/issues/4180) 
* [Use enum tokens when they are the only possible grammar usage. · Issue #4505](https://github.com/gpuweb/gpuweb/issues/4505) 
* 