### What is mocking

The purpose of mocking is to isolate and focus on the code not on the state of external dependencies.
In mocking, dependencies are replaced by closely controlled replacements objects ; Fakes/Stubs/Mocks

- ##### Fakes

  : replace the actual code by implementing the same interface.
  : usually hard-coded to return fixed results.

- ##### Stubs

  : return a specific result based on a specific set of inputs.

  : usually won't respond to anything outside of what is programmed for the test.

- ##### Mocks

  : return value like Stubs

  : can be programmed with expectations in terms of how many times each methods should be called.