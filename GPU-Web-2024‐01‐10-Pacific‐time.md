# GPU Web 2024-01-10 Pacific-time

Note that unless stated otherwise this is a GPU for the Web community group meeting and not a working group meeting.

Chair: KG

Scribe: KR

Location: Google Meet


## Tentative agenda



* Administrivia
* CTS Update
* “Tacit Resolution” [Queue](https://github.com/gpuweb/gpuweb/issues?q=label%3A%22tacit+resolution+queue%22)
* WebGPU compat:
    * Disallow sample_index builtin in Compatibility. [#4443](https://github.com/gpuweb/gpuweb/pull/4443)
    * Compatibility Mode: disallow two-component (RG) texture formats in storage texture bindings. [#4444](https://github.com/gpuweb/gpuweb/pull/4444)
    * Compatibility mode: depth bias clamp must be zero. [#4445](https://github.com/gpuweb/gpuweb/pull/4445)
    * Compat: renderable float16, float32 textures [#4417](https://github.com/gpuweb/gpuweb/issues/4417)
* Make Setting canvas.width and canvas.height lighter weight [#4388](https://github.com/gpuweb/gpuweb/issues/4388)
* createTexture does not validate viewFormats against usage [#4426](https://github.com/gpuweb/gpuweb/issues/4426)
* Agenda for next meeting


## Attendance

WIP, it is the list of all the people invited to the meeting. **In bold the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Mike Wyrzykowski **
* ByteDance
    * Yunchao He
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Google
    * Austin Eng
    * Ben Clayton
    * Brandon Jones
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
    * **Shaobo Yan**
    * Yang Gu
    * **Zhaoming Jiang**
* Microsoft
    * Jesse Natalie
    * **Rafael Cintron**
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
* Deyu Tian
* Francois Daoust
* James Moir
* Mehmet Oguz Derin
* Michael Shannon
* Rajesh Malviya
* Rob Conde
* Russell Campbell


## Administrivia



* KG: Working toward getting the spec out of draft and toward Candidate Recommendation
    * Things like horizontal reviews from I18N and PING
* 
* 
* 


## CTS Update



* Lots of Compat testing
    * GT: trying to make it work. Shouldn't affect folks who aren't working on Compat.
    * KN: getting exceptions in to make them work in Compat mode. Have a pretty good test suite for Compat right now.


## “Tacit Resolution” [Queue](https://github.com/gpuweb/gpuweb/issues?q=label%3A%22tacit+resolution+queue%22)



* Nothing this week


## WebGPU compat:


### Disallow sample_index builtin in Compatibility. [#4443](https://github.com/gpuweb/gpuweb/pull/4443)



* Merged! 


### Compatibility Mode: disallow two-component (RG) texture formats in storage texture bindings. [#4444](https://github.com/gpuweb/gpuweb/pull/4444)



* Merged!


### Compatibility mode: depth bias clamp must be zero. [#4445](https://github.com/gpuweb/gpuweb/pull/4445)



* Merged!


### Compat: renderable float16, float32 textures [#4417](https://github.com/gpuweb/gpuweb/issues/4417)



* KG: Tolerance for precluding certain devices from running Compat?
    * Numbers for Mali-T720 were 0.7% of some amount of users - decent chunk
    * Not a showstopper though, if users can get WebGL2 but not WebGPU/Compat
* KG: valuable to have one of these in core. Not just two options for renderable floats - in order to gain 0.7% of users. So can we require EXT_color_buffer_float?
    * KR: I don't think we can do that - iOS devices at least used to only support rendering to 16-bit floating point attachments.
* KN: the Mali-T720 looks prevalent and doesn't have these extensions on any recent Android version. So I think that's enough to preclude requiring these in Core.
    * KG: 2013 device?
    * KN: yes, probably.
    * KG: these probably aren't great devices anyway. Banning them probably won't break the ecosystem.
* Discussion about how performant these devices would be if they have compute but not FP renderability.
* Will think about this more.
* 
* 
* 


## Make Setting canvas.width and canvas.height lighter weight [#4388](https://github.com/gpuweb/gpuweb/issues/4388)



* GT: can we make this lighter weight? Record the new width/height, and make the next texture the right size. Currently requires destroying the texture, which is problematic - and create the new one. Is that observable?
* GT: I have something which calls "free()" now, and have to refactor a bunch of code to mark it as freed.
* KG: point is - if you want to know how this can work, look at how 2D canvas and WebGL handle this, they already do this optimization.
* GT: I want this to be fast. Portably and reliably. It's easy to fall off the fast path. Can we word things in a way that will make it more obvious? The function does nothing except set the width/height - can't see the effect until you call getCurrentTexture. You can view the destruction of the old texture.
* KG: you can see the clearing of the canvas in the other specs - e.g. 2D canvas resets the context upon resize. The way you have to set width and then height, all the browsers record these and do things lazily.
* KN: it's not just a perf issue - if you write the code that seems correct, it'll do the wrong thing. That seems like the thing to try to figure out
* KG: I hear you, but in context of the other canvas contexts, what we have today is what the others are doing, things are going fine there.
* KN: trying to figure out what's happening in Gregg's screen recording. Behavior I see is not what I'd expect from the spec. I'd expect the canvas to go blank during the resize, potentially. Don't understand the order of things in the frame that causes the stretching behavior.
* KG: often the stretching behavior's because the GPU can't produce new frames because you're allocating over and over. Previous frame wasn't done rendering.
* GT: it's just a ResizeObserver setting the canvas width/height, and rendering on every rAF. It might just be a bug in Chrome.
* KN: trying to figure out whether this is just a Chrome bug. ResizeObserver should be happening between frames. If you're rendering in each frame I think it shouldn't be happening.
* KG: one concern of going down this path of making it more explicit when these things happen - in the spec we'll try hard to spec when things happen, but in a way that's more complex than they are today. And later we may find we need more optimizations, so we're stacking spec optimizations on top of implementation optimizations. Would be simpler if we had a simple reading of the spec. Maybe behave as-if. Browsers can do coarse-grained perf tests in the CTS - e.g. resizing 1000x behaves OK.
* KN: to clarify - 2 different things. One is perf (I didn't read this till now). I think I agree with you - if we can make it fast, no worries about complexity in Chrome. I'm concerned about allowing applications to do the right thing and get a good user experience.
* KG: high level, my interpretation of the spec - it matches the analogous wording of the other 2 specs, and those provide a usable developer UX.
* KN: it's intended to match those. Hopefully it does.
* GT: don't want to make it more confusing. :) But right now the spec's more complex because of the current wording. If you just change it to set two variables that are taken into consideration when calling getCurrentTexture, it's far simpler.
* KN: when you read out of a canvas, we always give you an image with the canvas width/height. 2D canvas was designed as a synchronous API. Canvas is beholden to that. That's why it works like this.
* KG: would be beneficial to take this example and do it in WebGL.
* GT: I did that, it doesn't have the same problem.
* KN: then this sounds like a Chrome bug. We'll talk about it more.


## createTexture does not validate viewFormats against usage [#4426](https://github.com/gpuweb/gpuweb/issues/4426)



* KG: Austin/Teo think these are Vulkan validation errors that are wrong.
* KN: think we should validate this, regardless of what Vulkan validation says.
* KG: you can create a view with different parameters.
* KN: basically, yes. OK, I don't want to add this validation if we don't have to.
* KG: let's table this then unless it comes back as a spec issue.


## Other topics



* SY: nothing from Intel today.


## Agenda for next meeting



* Don't need to meet next week.
* Plan on meeting on Jan 24. Atlantic time.
* KN: great that we're getting this Compat stuff done before the meetings. Thanks everyone for doing the pre-work.
* 
* 
* 
* 