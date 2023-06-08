# GPU Web 2023-06-07/08 (Pacific time)

Note that unless stated otherwise this is a GPU for the Web community group meeting and not a working group meeting.

Chair: KG

Scribe: KR

Location: Google Meet


## Tentative agenda



* Administrivia
    * Corentin: Gathering list of attendees for a future meeting with the PING!
* CTS Update
* “Tacit Resolution” [Queue](https://github.com/gpuweb/gpuweb/issues?q=label%3A%22tacit+resolution+queue%22)
* (socialization) Space efficient wide color support [#4108](https://github.com/gpuweb/gpuweb/issues/4108)
* (socialization) Cannot upload/download to UMA storage buffer without an unnecessary copy and unnecessary memory use [#2388](https://github.com/gpuweb/gpuweb/issues/2388)
    * Recap the design constraints for this problem
* (Intel) Why GPUCommandBuffer and GPUCommandEncoder cannot be reused? [#4138](https://github.com/gpuweb/gpuweb/issues/4138)
* Supported sources for copyExternalImageToTexture() [#4165](https://github.com/gpuweb/gpuweb/issues/4165)
* (stretch) Render to 3D texture slices [#2877](https://github.com/gpuweb/gpuweb/issues/2877)
* Agenda for next meeting


## Attendance

WIP, it is the list of all the people invited to the meeting. **In bold the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * **Mike Wyrzykowski **
    * **Myles C. Maxfield**
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Google
    * Austin Eng
    * Ben Clayton
    * **Brandon Jones**
    * Corentin Wallez
    * Dan Sinclair
    * David Neto
    * **Gregg Tavares**
    * **Kai Ninomiya**
    * **Ken Russell**
    * Loko Kung
    * Shannon Woods
    * Shrek Shao
    * Stephen White
    * Ryan Harrison
* Intel
    * Brandon Jones
    * Bryan Bernhart
    * **Hao Li**
    * Jiajia Qin
    * **Jiawei Shao**
    * Jie Chen
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
* Microsoft
    * Jesse Natalie
    * Rafael Cintron
* Mozilla
    * Erich Gubler
    * Jim Blandy
    * **Kelsey Gilbert**
    * Teodor Tanasoaia
* Nvidia
    * Anders Leino
    * Thomas Meier
* Snap
    * Corvyn Kusuma
    * Dhritiman Sagar
    * Sumant Hanumante
* Unity
    * Brendan Duncan
    * Dave Shreiner
* Connor Fitzgerald
* Francois Daoust
* Mehmet Oguz Derin
* Michael Shannon
* Rob Conde
* Russell Campbell


# Minutes


## Administrivia



* Corentin: Gathering list of attendees for a future meeting with the PING!
    * To discuss fingerprinting concerns.
    * Reply to Corentin's email to get on the list.


## CTS Update



* KN: working on shader tests.
* KN: I’m also working on finally improving the default chunking of the CTS into WPT tests. This should keep things under WPT’s 5 second limit and make imports easier for WebKit. TBD whether I can also make things easier for Firefox. (There has also been someone working on this in Servo, hopefully works for them too.)
* KG: where in WPT does the 5 second limit come from?
* KN: think it's in the WPT documentation - will find it.
* KR: will this adequately address WebKit's / Firefox's concerns?
* MM: if CTS has .ts and 100 deps - that's what it is - I'd probably have argued about this in the past but we're focused on making it work.
* KG: same here, focused on making it work, period. Can't say 100% that we utilize the output from the WPT harness that the CTS produces. Only 95% sure. We put it in some form and output custom stuff in our WPT runner. Not sure what happens in the middle. When we talk about changes / improvements to the WPT output, AFAIK, it's just "changes" and they're scary.
* KN: understood. Been thinking about this in the context of a couple recent changes. Will talk with Eric too to make sure things don't get more complicated for Firefox.


## “Tacit Resolution” [Queue](https://github.com/gpuweb/gpuweb/issues?q=label%3A%22tacit+resolution+queue%22)



* Empty.
* KN: Editors did meet this week but did not resolve any open issues


## (socialization) Space efficient wide color support [#4108](https://github.com/gpuweb/gpuweb/issues/4108)



* MM: couple different points
* MM: there is the concept of wide color, it's different from HDR
    * Can show "more saturated" / "more red" than sRGB
    * Can't show "brighter" colors
* For rendering: the space of representable colors gets a little, but not a lot, bigger
* To represent wide color using the same format as for HDR, float16, would be a waste. 2x the memory as RGBA8, but color volume isn't 2x as large.
* We're looking at formats that have a little bit more representable values.
* E.g. RGB10_A2 formats
* 2 ways to expose these.
* 1. Take the volume expressed by RGB, say, this is the volume I want to map to the wide-color gamut. RGB(1,0,0) is the most red the new gamut can represent.
    * KN: and you can do that today by setting canvas's color space to display-p3.
    * MM: yes. Because color volume's bigger with wide-color, it's OK mechanically to say I have a normal RGB texture and its values are in the bigger color gamut, but the downside is that two adjacent colors will have visible differences - will show banding in the larger color volume.
* 2. Say that some small range of values outside the (0.0, 1.0) range in the new color volume are legal. sRGB colors in the new volume are unchanged, so blending works well. Downside is it's a different mental model compared to other texture formats.
    * KN: current status: we in principle can do this now. If you use float16 format, you can go outside the 0-1 range. Using extended sRGB colorspace, it'll map meaningfully. What we don't have is a way to do this aside from float16. That's what the XR formats do.
* KG: following the overview, but quibbles about some assertions. Can table these for now.
* KG: don't think that things being more prone to banding means that banding occurs much in practice. Have concerns about the XR stuff, but not about the 10 bit stuff.
* MM: good segue. The XR color pixel formats in mac/IOS world - let's say the oracle tells you the in-memory bits are 0. Sample it - result of sample won't be 0. It'll be -0.25 for example. Similar for 1.0 - result will be ~1.25.
    * Windows has similarly named pixel format, but its caps are different. Docs are sparse. Can't be sampled from? Not sure.
    * KN: Rafael is tasked with finding this answer.
    * If the Windows formats can be sampled from and rendered to, then there's a possibility of adding them to WebGPU. If not, then no avenue for this. (Don't want to spec mac/ios specific feature).
    * In that case, wouldn't spec XR formats. Render into regular 10_a2 formats, and annotate the canvas - the canvas is now p3 (and have to add 10_a2 as a canvas backing store format).
* KR: XR stands for eXtended Range?
* MM: yes.
* MM: we're interested in wide-color, you can use the same facilities as HDR, but has memory consumption downside. Looking for suitable replacement.
* KR: what about shared exponent formats? Are they used more for HDR? They have 9 bits per color channel.
* MM: not sure, know they're used for HDR, not sure about wide-color applications.
* MM: also - if you're a game - not enough for your dest to support wide color. Need for your texture assets to use wide color, too.
* KG: the way the HW APIs work - less to do with the pixel format. That's just the shape of the data. Doing math on abstract values in the shader. What does 1.0 red mean? In particular, curious to know what we're missing that's not captured by RGB10 formats.
* MM: if app chooses to render into XR format rather than 10_a2 format - the thing we get from the platform is cheaper compositing. Don't have to invoke the colorspace logic. If app renders into 10_a2 format but then says 1,0,0 doesn't mean sRGB max red, but other colorspace's max red - have to turn on color matching facilities, and those are expensive.
* KN: almost certainly can't put the XR formats into the API, but they seem like sRGB formats, and apply some mapping to the values. A 1:1 mapping between shader value and the value stored in the texture. Instead of being linear, it's something slightly more complex. But colorspace still stays out of it until you have a surface. In our case, if we had this format, you should only be able to use it with sRGB. Doesn't make sense to use it with other things (use 10_a2 otherwise). Windows restrictions probably make it unusable. But we could use the XR formats under the hood. We use 10_a2 and you say display-p3, but then color conversion has to happen. There's a middle ground where the app can take advantage of it, but might be confusing.
* KN: have suggested to do the D3D12 thing, on the issue.
* KG: interested in being able to reach system compositor fast paths. Aside from that, should pursue existing mechanics. We don't have space-efficient wide color support - or, if we do, it's because we haven't gotten around to exposing rgb10 formats at the canvas level. Today, all Windows machines with P3 or larger gamuts just stretch across the whole gamut with 8 bits, and banding isn't a persistent problem. We don't see >8-bit-per-channel as a hard requirement.
* MM: it's true that 10 bit per channel isn't a hard requirement for wide color gamut. Our goal isn't to add XR formats to WebGPU; it's to let apps run their whole pipeline in wide color and for it to look good.
* **Continue next week**


## (socialization) Cannot upload/download to UMA storage buffer without an unnecessary copy and unnecessary memory use [#2388](https://github.com/gpuweb/gpuweb/issues/2388)



* Recap the design constraints for this problem
* MM: wanted to touch base before going off and doing a bunch of engineering
    * We're interested in UMA working well
    * Interested in a potential solution where the same code would "do the right thing" on UMA and non-UMA
    * This group posited that that was not possible
    * I think it might be
    * Want to nail down what the original objections were
* KR: from our side we need enga@ and cwallez@ present for the conversation. Would like to advance this on the Github issue or mailing list.
* Postpone for a week?
* KG: I can try to synthesize
* KG: on non-UMA archs you sometimes need 2 copies, and on UMA you can get to 1 copy. How to pipeline, prioritizing bandwidth/latency, is where Corentin and I ran aground trying to find a single API to do both.
* KG: My position - if you try to figure out the API for these things, you'll either prove us wrong or right, and that's great
* MM: that's reassuring. Think we're in a different situation now than 2021. Now we have 2 ways of getting data on the card. I'd be coming back with a 3rd way. Adding a 3rd way isn't great for the platform, but if an app cares about the tradeoffs, we'd have more options for them.
* **Continue this next week**.


## (Intel) Why GPUCommandBuffer and GPUCommandEncoder cannot be reused? [#4138](https://github.com/gpuweb/gpuweb/issues/4138)



* JS: wonder whether we can have something like RenderBundle for compute shaders. We find we keep re-encoding the same command buffers over and over again for compute use cases. Consumes a lot of CPU time.
* KG: are you concerned about this because of the extra work you're doing? Are you running into perf issues from reencoding these buffers?
* JS: yes, ML models have hundreds of dispatch calls. Have to call them again and again.
* MM: if we added this - how much perf increase would you expect?
* JS: we haven't tested, but we see hundreds of dispatches. Consume significant CPU time.
* KG: concerned more about the CPU time and less about the number of calls. RenderBundles encapsulate thousands of calls - more concerned about that. Caveat - JS calls are expensive in engines. State of the art - what Vulkan says - don't. Just re-encode all the time. Would like to rely on that.
* GT: looking into RenderBundles (not compute). For 10K calls, 2.3 ms -> 1.1 ms (in JS). Optimizing, went from 3 ms -> 1.1 ms. Pretty significant. Had a Chrome issue where passing 10K RenderBundle references in 1 JS array is slow - need to fix that. It's fast in Firefox. A Unity game is doing 3-4K at least. Not sure about ML stuff.
* KN: right now JS calls are a little expensive. No matter how fast they get, 1000 JS calls + 1000 backend calls is more expensive than 1 JS call + 1000 backend calls. At least, costs you power. We don't know how fast they'll be in the long term. Making JS calls and getting them over to the other process takes a lot more work than doing it one time. Think it's worthwhile to have something. It's a straightforward API letting you record/replay. Either something for compute, or something more general than RenderBundles.
* MM: we've got indirect command buffers, and compute indirect cmdbufs, present on all hardware, and we don't see any cost to this. We don't see any obstacles. Should fit right into the API.
* KG: just think it's nonzero eng / impl cost and that we haven't proven to ourselves that this is a problem we need to address. Think it's hypothetical at this point. Technically room to do better, but these are precious investment points and I think we can do better not spending them here.
    * But, if you come to us with a demo and show us a speedup, that would be more compelling.
* JS: understood.
* MM: I found Gregg's numbers compelling.
* KG: would like to see more structure to them before I find them compelling.
* MM: **if Intel runs some measurements is that the next step**?
* KG: yes.
* JS: that's acceptable from our side.


## Supported sources for copyExternalImageToTexture() [#4165](https://github.com/gpuweb/gpuweb/issues/4165)



* KG: just an oversight?
* KN: not technically, but the difference isn't important. Just didn't put it in there to keep the video integration simple. We do need it. There are some questions about how to do this but Chrome hasn't shipped VideoFrame integration yet. It's not feature detectable (easily) which is annoying.
* MM: could make a test video and round-trip it through WebGPU. :)
* KG: there are some web standards people who would say that's feature detectable. :)
* KG: think we should just add a PR adding it to the other one too, and we'll accept that PR.
* KN: OK.
* KG: **make it so.**


## (stretch) Render to 3D texture slices [#2877](https://github.com/gpuweb/gpuweb/issues/2877)



* KG: true, represents regression compared to WebGL 2.0.
* KG: since we originally added this, think we can do this. Would need to be a 3D view of a 3D texture?
* MM: never written code which uses this. 3D texture is 3D. There are 3 dimensions you can use to slice it? Which dimension is it sliced on?
* KG: slice size is depth=1.
* MM: so full range of X and Y, and Z is constant?
* KG: that's what OpenGL does.
* KN: sure that D3D12/Metal call this the same. Vulkan too.
* KG: just need to figure out how we can do it.
* KN: yes, sounds like proposal / figuring out the details.
* KG: ok with milestone 2?
* KN: yes, think we're there.
* KG: I think we're still finishing Milestone 1.
* Discussion about which milestone to target the PR for.
* KG: for Milestone 2 we probably wouldn't merge this yet. Catch up implementation wise.
* KN: from impl perspective I want to call this Milestone 2. I don't have a firm position on when to put it in the spec. Think this should go into impls after other stuff is done.
* KG: since merging into spec is sign-off on consensus - don't want to commit eng resources to figure that out, since we have other more pressing things before Milestone 2 to work on. But if people want to work on next week's homework, go ahead. Moz won't be able to do due diligence on this till Milestone 2.
* KG: I think we're probably OK with this regression for now?
* KN: I think we're probably OK with this regression, but would like to have a better answer for the process of adding things to the spec. Don't want to build up a lot of backlog for spec additions. Not specific pushback.
* KG: the blocking thing is sufficient consensus for review on things being added into the spec.
* **This will stay Milestone 2.**


## Agenda for next meeting



* UMA
* Colors, again
* KN: other agenda items, please add them here.
* 