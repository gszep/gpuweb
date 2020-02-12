This document contains guidelines for contributors to the WebGPU CTS (Conformance Test Suite) on how to write effective tests, and on the testing philosophy to adopt.

The WebGPU CTS is arguably more important than the WebGPU specification itself, because it is what forces implementation to be interoperable by checking they conform to the specification. However writing a CTS is hard and requires a lot of effort to reach good coverage.

More than a collection of tests like regular end2end and unit tests for software artifacts, a CTS needs to be exhaustive. Contrast for example the WebGL2 CTS with the ANGLE end2end tests: they cover the same functionality (WebGL 2 / OpenGL ES 3) but are structured very differently:
 
 - ANGLE's test suite has one or two tests per functionality to check it works correctly, plus regression tests and special tests to cover implementation details.
 - WebGL2's CTS can have thousands of tests per API aspect to cover every combination of parameters (and global state) used by an operation. 

Below are guidelines based on our collective experience with graphics API CTSes like WebGL's. They are expected to evolve over time and have exceptions, but should give a general idea of what to do.

## Test hierarchies

Because of the glorious amount of test needed, the WebGPU CTS is organized as a tree of arbitrary depth (a filesystem with multiple tests per file). Tests are grouped in large families and the root and first few levels looks like the following (some nodes omitted for simplicity):

 - **`api`** with tests for full coverage of the Javascript API surface of WebGPU
     - **`validation`** with positive and negative tests for all the validation rules of the API
     - **`operation`** with tests that checks the result of performing valid WebGPU operations, taking advantage of parametrization to exercise interactions between features
     - **`regression`** for one-off tests that reproduce bugs found in implementations to prevent the bugs from appearing again
 - **`shader`** with tests for full coverage of the shaders that can be passed to WebGPU
     - **`validation`**
     - **`execution`** similar to `api/operation`
     - **`regression`**
 - **`idl`** with tests to check that the WebGPU IDL is correctly implemented, for examples that objects exposed exactly the correct members, and that methods throw when passed incomplete dictionaries
 - **`web-platform`** with tests for Web platform-specific interactions like `GPUSwapChain` and `<canvas/>`, WebXR and `GPUCommandEncoder.copyImageBitmapToTexture`

At the same time test hierarchies can be used to split the testing of a single sub-object into several file for maintainability. For example `GPURenderPipeline` has a large descriptor and some parts could be tested independently like `vertexInput` vs. `primitiveTopology` vs. `blending` but all live under the `renderPipeline` node.

In addition to the test tree, each test can be parameterized. For coverage it is important to test all enums values, for example for `GPUTextureFormat`. Instead of having a loop to iterate over all the `GPUTextureFormat`, it is better to parameterize the test over them. Each format will have a different entry in the test list which will help WebGPU implementers debug the test, or suppress the failure without losing test coverage while they fix the bug.

Extra capabilities like in WebGL extensions are tested in the same file as the rest of the API, but skipped using `pfilter` when the extension is not present, and the test executed with and without the capability enabled. For example a compressed texture format capability would simply add a `GPUTextureFormat` to the parametrization lists. However a capability adding significant new functionality like ray-tracing could have separate subtrees.

## Test fixtures

Most tests can use one of the several common test fixtures:

  - `Fixture`: Base fixture, provides core functions like `expect()`, `skip()`.
  - `GPUTest`: Wraps every test in error scopes. Provides helpers like `expectContents()`.
  - `ValidationTest`: Extends `GPUTest`, provides helpers like `expectValidationError()`, `getErrorTextureView()`.
  - Or create your own. (Usually not necessary - helper functions can be used instead.)
  
### GPUDevices in tests

Currently, the harness creates a new `GPUDevice` for **every test case**.

However, because devices are largely stateless (except for `lost`-ness, error scope stack, settable attribute values), they can be reused for multiple tests.

As it becomes more necessary, we'll implement this. There will probably be a pool of several devices so that multiple asynchronous tests can run at once.

## Asynchrony in tests

Since there are no synchronous operations in WebGPU, almost every test is asynchronous in some way. For example:

  - Checking the result of a readback.
  - Capturing the result of a `popErrorScope()`.

That said, test functions don't always need to be `async`.

### Checking asynchronous errors/results

Validation is inherently asynchronous (`popErrorScope()` returns a promise). However, the error scope stack itself is synchronous - operations immediately after a `popErrorScope()` are outside that error scope.

As a result, tests can assert things like validation errors/successes without having an `async` test body.

Example:
```typescript
t.expectValidationError(() => {
    device.createThing();
});
```
does
- `pushErrorScope('validation')`
- `popErrorScope()` and "eventually" check whether it returned an error.

Example:
```typescript
t.expectContents(srcBuffer, expectedData);
// 
```
does
- copy `srcBuffer` into a new mappable buffer `dst`
- `dst.mapReadAsync()`, and "eventually" check what data it returned.

Internally, this is accomplished via an "eventual expectation": `eventualAsyncExpectation()` takes an async function, calls it immediately, and stores off the resulting `Promise` to automatically await at the end before determining the pass/fail state.

### Asynchronous parallelism

A side effect of test asynchrony is that it's possible for multiple tests to be in flight at once. We do not currently do this, but it will eventually be an option to run `N` tests in "parallel", for faster local test runs.

## Test parameterization

The CTS provides a helper (`pcombine`) for creating large cartesian products of test parameters. The harnesses runs one "test case" for each one.

Test parameterization and `pcombine` should be applied liberally to ensure the maximum coverage possible within reasonable time. You can skip some with `pfilter`. And remember: computers are pretty fast - thousands of test cases can be reasonable.

Use existing lists of parameters values (such as [`kTextureFormats`](https://github.com/gpuweb/cts/blob/0f38b85/src/suites/cts/capability_info.ts#L61), to parameterize tests), instead of making your own list. Use the info tables (such as `kTextureFormatInfo`) to store and retrieve information about the parameters.

## Validation tests

Like all `GPUTest`s, `ValidationTest`s are wrapped in both types of error scope. These "catch-all" error scopes look for any errors during the test, and report them as test failures. Since error scopes can be nested, validation tests can expect that there *are* errors from specific operations.

There are many aspects that should be tested in all validation tests:

 - each individual argument to a method call (including `this`) or member of a descriptor dictionnary should be tested including:
     - what happens when an error object is passed
     - what happens when an extension enum or method is used
     - what happens for numeric values when they are at 0, too large, too small, etc
 - each validation rule in the specification should be checked both with a control success case, and error cases
 - each set of arguments or state that interact for validation

When testing numeric values, it is important to check on both sides of the boundary: if the error happens for value N and not N - 1, both should be tested. Alignment of integer values should also be tested but boundary testing of alignment should be between a value aligned to 2^N and a value aligned to 2^(N-1).

## Operation tests

Operation tests are usually `GPUTest`s. As a result, they automatically fail on any validation errors that occur during the test.

Use helpers like `expectContents` (more to come) to check the values of data on the GPU. (These are "eventual expectations" - fire and forget).

When testing something inside a shader, it's not always necessary to output the result to a render output. In fragment shaders, you can output to a storage buffer. In vertex shaders, you can't - but you can render with points (simplest), send the result to the fragment shader, and output it from there. (Someday, we may end up wanting a helper for this.)

## IDL tests

These tests test *only* the rules set by IDL, plus rules which we couldn't express in the IDL. For example:

  - Test with no optional arguments/members to ensure they are optional.
  - For each required argument/member, test that there is an exception if it's not present.
  - Test values out of range.
  - Test values of incorrect type.
  - For sum types, test each type.
  - Lengths of static-sized arrays such as `GPUColor`.

Every overload of every method should be tested.

Finally, this is probably also where we would test that extensions follow the rule that: if the browser supports an extension but it is not enabled on the device, then calling methods from that extension throws `TypeError`.

  - Test providing unknown properties *that are definitely not part of any extension* are valid/ignored. (Unfortunately, due to the rules of IDL, adding a member to a dictionary is always a breaking change. So this is how we have to test this unless we can get a "strict" dictionary type in IDL. We can't test adding members from non-enabled extensions.)


## Example: operation testing of vertex input id generation

Somewhere under the `api/operation` node are tests checking that running `GPURenderPipelines` on the device using the `GPURenderEncoderBase.draw` family of functions works correctly. Render pipelines are composed of several stages that are mostly independent so they can be split in several parts such as `vertexInput`, `rasterization`, `blending`.

Vertex input itself has several parts that are mostly separate in hardware:

 - generation of the vertex and instance indices to run for this draw
 - fetching of vertex data from vertex buffers based on these indices
 - conversion from the vertex attribute `GPUVertexFormat` to the datatype for the input variable in the shader

Each of these are tested separately and have cases for each combination of the variables that may affect them. This means that `api/operation/render/vertexInput/idGeneration` checks that the correct operation is performed for the cartesian product of all the following dimensions:

 - for encoding in a `GPURenderPassEncoder` or a `GPURenderBundleEncoder`
 - whether the draw is direct or indirect
 - whether the draw is indexed or not
 - for various values of the `firstInstance` argument
 - for various values of the `instanceCount` argument
 - if the draw is not indexed:
     - for various values of the `firstVertex` argument
     - for various values of the `vertexCount` argument
 - if the draw is indexed:
     - for each `GPUIndexFormat`
     - for various values of the indices in the index buffer including the primitive restart values
     - for various values for the `offset` argument to `setIndexBuffer`
     - for various values of the `firstIndex` argument
     - for various values of the `indexCount` argument
     - for various values of the `baseVertex` argument

"Various values" above mean several small values, including `0` and the second smallest value to check for corner cases, as well as some large value.

An instance of the test sets up a `draw*` call based on the parameters, using point rendering and a fragment shader that outputs to a storage buffer. After the draw the test checks the content of the storage buffer to make sure all expected vertex shader invocation, and only these ones have been generated.