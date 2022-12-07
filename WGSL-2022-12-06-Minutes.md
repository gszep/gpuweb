# WGSL 2022-12-06 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** JB, DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** (APAC) Tuesday **4-5pm **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-11-29 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1SrScipp1_ssnjmHnXzLP6JDJIS4yTW5i4bbpZIV0raM) 

 

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
    * Hao Li
    * Jia A Chen
    * Jiajia Qin
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * **Shaobo Yan**
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
    * **Rafael Cintron**
    * **Tex Riddell**
* Mozilla
    * **Erich Gubler**
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * Teodor Tanasoaia
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



* Landed workgroupUniformLoad [https://github.com/gpuweb/gpuweb/pull/3586](https://github.com/gpuweb/gpuweb/pull/3586) 
    * Also enhances uniformity analysis so it understands that a pointer can be uniform while the value it points to is not uniform. [https://github.com/gpuweb/gpuweb/issues/3602](https://github.com/gpuweb/gpuweb/issues/3602) 
* text/wgsl media type registration
    * DN:  I sent email for prelim review (again). This time I registered for the mailing list so hopefully the email goes through this time.
    * DN: Also landed a change to incorporate the registration info in an appendix.  Found that in the recommended instructions for W3C media types.
    * (MOD: I added a comment in the issue, mailing list status is different between W3C doc and the mailing list top itself [https://github.com/gpuweb/gpuweb/issues/1682#issuecomment-1340150833](https://github.com/gpuweb/gpuweb/issues/1682#issuecomment-1340150833) , not sure though, but I see new requests sent to the new list)
    * 


---


# ⏳ Timeboxes (until XX:15)


### [WGSL: allow bgra8unorm as texel format with extension bgra8unorm_storage #3640](https://github.com/gpuweb/gpuweb/pull/3640)



* DN: The PR is well-formed, and the only question is, should we add it. We want to defer this question to the API folks. We don’t want to pollute the extension name space with a zillion different extension names. The main question is just whether to add it or not. This seems to be mostly an API concern.
* KG: In the issue, Kai (Ninomiya) is saying, this something we probably don’t need at the WGSL layer.
* JS: I think we need this decoration in WGSL because we need to call getBindGroupLayout from this pipeline, and we need to return a bindgroup layout with this format, and check the texture format. So we need this at least for getBindGroupLayout 
* KG: Isn’t getBindGroupLayout just a way to get around specifying the layouts yourself? Do e really need to add something to WGSL just to help auto-generated BGLs?
* JS: without the WGSL feature, you’d need to manually specify the format.
* JS: Another question: it’s weird to have BGRA8 without having something that can be directly translated into SPIR-V
* KG: It’s a layering thing. You might have a BGRA8 resource at the API layer, but the SPIR-V would always work with it as if it were RGBA8. The binding would do the swizzling. So technically there’s nothing we need to do in WGSL. IT’s only on the API side that you can present something that’s BGRA8 anyway.
* DN: I think that’s right. In Vulkan, you specify swizzles applied to those axes outside the shader itself.
    * What I found is that the rearrangement of the components (swizzling) is specified in the creation of the texture view:
        * [https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VkImageViewCreateInfo.html](https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VkImageViewCreateInfo.html)  is the call to create an image view in Vulkan
        * One field there is `VkComponentMapping         components;`
        * `components` is a [VkComponentMapping](https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VkComponentMapping.html) structure specifying a remapping of color components (or of depth or stencil components after they have been converted into color components).
        * And that says which components to pick out from the data to present to the shader in the .x .y .z. And .w  components. 
* AB: Language clarification: We have the .rgba accessors, is the swizzle visible to these? Would the first component be the ‘b’, or the ‘r’?
* DN: In the WGSL swizzle context, `.rgba` is just a synonym for `.xyzw`
* ZJ: I thought accessing .r in WGSL should always get the R channel in both RGBA and BGRA format?
* KG: I think that’s how it’s set up today. If you create a Vulkan bindgroup that has BGRA storage, and then you write ‘r’ in the shader, you get the ‘r’ value. The ‘r’ is semantic, not necessarily referring to exactly how the bytes are laid out in storage. If the storage is BGRA, the shader doesn’t have to know. ‘R’ is always ‘r’.
* KG: So I think we’re set here, except for getBindGroupLayout. We could just say, don’t use that, then. But it does mean it’s ambiguous what the layout should be. I think we should just default to the RGBA8 format.
* KG: If all that’s correct, I think we shouldn’t have BGRA specified as part of WGSL, if the only purpose is for getBindGroupLayout.
* JS: So we should allow the getBindGroupLayout with the BGRA enum match the WGSL with RGBA8? 
* KG: Those would not be compatible layouts. The getBindGroupLayout time would be the time at which that would be selected.
* JS: I’m thinking of the case where we manually created a getBindGroupLayout object specifying BGRA8 format. When we use that with a shader, we need to decide if that matches the shader’s expected texture.
* ZJ: We can always specify that BGRA8 is mapped to …
* AB: That’s only for a sampled texture. For storage textures, that’s where the format needs to match.
* KG: The constraint here is, you have a bindgroup layout and the storage format will be either RGBA8 or BGRA8, and any image you attach must match. So if you call getBindGroupLayout to infer a layout, then you will have to use RGBA8 resources with it. If you wanted to use BGRA8 layout, you’d have to manually create a bindgroup layout, but then you would only be able to use … formats. So the only thing to add here is maybe something that helps getBindGroupLayout, but that doesn’t seem necessary for V1.
* JS: So, you mean we cannot create a pipeline with a layout that has BGRA8 storage texture format? It has to match the declaration in WGSL, and we don’t have such a declaration.
* KG: it would be compatible with that.
* JS: So BGR8 and RGB8 would both be compatible with a shader that specifies RGB8?
* KG: The getBindGroupLayout would determine the format of the storage texture.
* AB: I think they’re asking about storage textures. There’s a line in the API that the texel format has to match. We would either have to relax that, or you couldn’t use it in a storage texture. The step is at the end of “validate shader binding”(???) So either this is already doable, or WGSL needs a format to satisfy that validation step
* KG: If the WGSL format is RGBA8, then the pipeline layout format must be either RGBA8 or BGRA8, at pipeline creation time.
* JB: The API spec validation does not say that.  Need to update that part of the API spec validation to allow matching rgba8unorm in the declaration of the texture type in WGSL with either rgba8unorm or bgra8unorm in the API storage texture format.
* KG: **Sounds like we can kick this back over to the API side.**


### [wgsl: add sign builtin function on integer scalar and vector #3658](https://github.com/gpuweb/gpuweb/issues/3658)



* PR: [https://github.com/gpuweb/gpuweb/pull/3659](https://github.com/gpuweb/gpuweb/pull/3659) 
* KG: Seems reasonable
* **Resolution: Let’s take it**


### Language evolution [https://github.com/gpuweb/gpuweb/issues/3149](https://github.com/gpuweb/gpuweb/issues/3149)  



* Technically we *can* release v1 without it but it sure would be nice to have a plan.
* Eg. a min-version-sensing function for use in a static assert.
* DN: We stopped talking about this for a while. We can ship v1 without a plan, but it would be unfortunate if we need to add something to check the version, but then it’s not available in the first version, so even when you’re checking the version you need some dancing to get around its absence in v1. **Google is working on this internally.**
* AB: Myles had talking points about this, so we can’t reach any conclusion about this until he returns, but **we can work on it**.
* 


---


# ⚖️ Discussions


### [Add uniformity analysis opt-out for derivative-using builtins #3644](https://github.com/gpuweb/gpuweb/pull/3644)



* JB: Mozilla talked about this some. As Mozilla understands it, the committee has consensus on the following:
    * In compute shaders, using barriers in non-uniform control flow can have serious consequences for stability. WGSL should apply the uniformity unconditionally in compute shaders, and opt-out annotations should not be supported.
    * In fragment shaders, the consequences of computing derivatives in non-uniform control flow are not as serious: non-portable or meaningless values and rendering artifacts, but not undefined behavior. These problems are often considered tolerable even in production.
    * In fragment shaders, an annotation on calls to builtin function that compute derivatives (implicitly or explicitly) to say that the call need not be executed in uniform control flow makes sense. Mozilla supports the attribute as described in #3644.
    * Making fragment shader uniformity failures hard errors, without providing opt-outs, localized or otherwise, puts one’s users in a situation from which they have no recourse, and it is inevitable that users would object. Mozilla feels that such a process provides no insight into how helpful users would find an analysis with localized opt-outs.
    * Applying fragment shader uniformity analysis to a production code base translated into WGSL provides only limited information on how helpful the analysis is to people writing new shaders: the shaders have already been debugged, so there should be few problems to detect in the first place.
    * Mozilla suggests that the fragmentShaderDerivativesCheckedUniformity processing option should demote uniformity errors in fragment shaders to warnings. Since the attribute on derivative collective operations fully silences warnings, this gives users a quick path to getting things up and running, and then lets them silence warnings as time permits. This preserves gentle pressure on users towards portable behavior.
* AB: Want clarification.  How I read the last part is, if the default remains the same….?
* JB: Starting point is error. If flip the option to false, they become warnings.
* KG: Sympathize David’s concern that NDEBUG defaults to true.  Would be better that the default eval is false.
* DN: Fine with flipping the sense of the global option.
* TR: For Microsoft, this sounds fine.  Comment from David though.
* DN: I posted a comment today summarizing Google’s developed understanding that the value of the analysis is outweighed by the false positives.
* JB: Those are on already-debugged shaders. (And hence doesn’t provide the signal we care about.)
* …
* BC: Have heard from experienced developers writing _new_ shaders, and they complained.
* DG: Discussed briefly with Myles.   We had assumed uniformity analysis was required for security. If non-uniform control flow for derivatives is used to smuggle info out of other shaders, we don’t see it as an opt-out. If it’s not an issue that way, then we don’t have strong opinions on enforcement. E.g. could be a lint in dev tools.
* KG: Think in fragment shaders it’s not a security concern.
* DG: Observation on Google’s testing shaders.  They are game shaders, because they run well enough, doesn’t mean it’s secure. Can’t be asserted that they are secure (including not leaking information).
* JP:  Keep coming back to the question of whether the analysis is useful.  Think that misses the point we’re trying to make. Just because the analysis can be useful to *some* people, doesn’t justify that it must run in CreateShaderModule.  Question is does that provide extra value over and above running in other tools.
* KG: There is value in having it run “on accident”, ie. unintentionally. These are things people don’t understand, and won’t know to seek out.
* JB: Want to reinforce. We have an opportunity to shift the industry. Introduce a really wide audience to a good analysis.  Having it in CreateShaderValue is an opportunity to widely broadcast the opinionated thing that we think are good.  It’s within our power to take out, but not add later. Think we should be brave and try it. If everybody hates it, then take it out later.  That’s the backward compatible thing.
* JB: Re: security. If anybody finds a case of info leakage via derivatives, then I don’t think we have any way to address that without going to a no-opt-out shader model. That’s not what we have in WebGL, and no professional developer will tolerate.  No-opt-out is a non-starter.  There must be some opt-out in fragment shaders.  Should not end up with something less useful than WebGL.
* AB: Trying to conceptualize the security risk, between shaders. Can understand how invocations can leak info to each other, but not between shaders.  Derivatives are not special in that way.  You might ask about indeterminate values.
* KG: We say indeterminate values that are safe, i.e. don’t leak.
* KG: Repeat Jim’s point that WebGL definitely doesn’t protect this derivatives case itself.
* AB: Re: be brave and propagate FXC.  FXC doesn’t complain about many of these. But we go further.
* JB: Didn’t want to index too much on FXC.  Just wanted to acknowledge prior work.
* TR: FXC had more strict validation, and it was relaxed because developers needed things that FXC couldn’t prove uniform. Believe it’s not enforced for derivatives.
* BC: Want to flip the fact-finding thing. Do we have evidence/facts that people want this and it would have saved them from bugs in shader code.
* KG: Think it’s hard to answer.
* JB: Ben is correct there’s a duality to the question. Will try to talk to folks to get that information.
* KG: Do personally believe this provides value.  Even folks who have written lots of shaders before, and I didn’t know the depths and paucity of the guarantees that are given to us.  I extrapolate that to other people.  Think we have an overall consensus and differ only on degree.   Think nobody is critically disadvantaged by the consensus we’re offering.
* GR: Think there has been maybe contempt by shader authors.  I know the quote of “faster horses”. That suggests we know best, and risk of people ignoring this shader language entirely.  Not sure if it’s productive.  But that’s my paramount concern.
* KG: We’re in an unusual situation, to push back in laxness that exists in shader authoring. Many folks don’t realize we have these constraints.  And even if we get it wrong, can offer getting around it.
* BC: Agree with what Greg said.  That’s part of the reason Google pushes hard on this. Don’t want to offend the community of people who have been in this space longer than most of us.  Nobody we have spoken to believe derivatives in fragment shaders believe it should stop them from doing what they need to do.  Before we make it more difficult to write shaders, is it a problem that needs solving.
* JB: Mozilla agrees expert shader authors need to be able to write what they need to write.
* KG: Do you think requiring pro shader authors to ask for this opt-out is unreasonable and will turn them off WebGPU in a harmful way.
* BC: It won’t turn off folks who are forced to write WebGPU code.  But we will get negative publicity.
* JB: Feels like a very scoped concern.  Have made millions of choices.  We are here to say what we think is good.
* EG: Asking context to Ben.  I’ll ask async.
* GR: It’s been said, we’re forcing people to write WGSL to use WebGPU. If there are users who are excited to use WGSL, would love to hear their opinions.  Have heard folks who have used WebGL are resistant in a number of areas. Want to hear more.
* KG: I would have liked to have this feature.
* JB: Do hear some people actually like it.
* DN: Raph likes WGSL, and has switched his project from the GLSL pipeline.
* KG: Hard to get feedback because the people who know in their bones the actual constraints, there are very few of them.
* KG: Want the group to consider taking #3644,  modulo reversing the sense of the global opt-out, and with the amendment that we’d like to see warnings required.  They’re warnings, so it’s down to the UA to actually present them or not.


### [Designing a uniformity opt-out #3554](https://github.com/gpuweb/gpuweb/issues/3554)



* PR [https://github.com/gpuweb/gpuweb/pull/3644](https://github.com/gpuweb/gpuweb/pull/3644) : proposed a fine-grain and coarse-grain opt-out
* 


---


# 📆 Next Meeting Agenda Requests



* Next week: (**APAC**!) Tuesday, December 13, **4-5pm** (America/Los_Angeles)
* **Write any PRs you’ve promised!**
    * There are no other v1.0 issues outstanding (other than the ones on this agenda!) that are not blocked awaiting proposals.
* WGSL schedule:
    * December 20: Non-APAC
    * (Christmas Eve&Day Dec24&25, Saturday&Sunday)
    * _December 27: No Meeting_
    * January 03: APAC
        * DN: Suggest we cancel
    * Note: Based on our current issues remaining for v1 though, we are very likely to cancel meetings due to lack of discussion topics, so we can likely be liberal with skipping meetings.
*  
* 