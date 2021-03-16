Here is a list of questions for feedback from ISV after the first round of API evaluation:
* is it OK to require the `storeOp` in every attachment? [#1376](https://github.com/gpuweb/gpuweb/issues/1376)
* is it OK that we auto-synchronize between dispatches in a single `GPUComputePass`?
* are implicit pipeline layouts useful?
* is it useful or confusing to call `getCurrentTexture()` multiple times per frame?