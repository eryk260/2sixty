# ADR 0019: Use React for the VIE-UI

## Status

- [ ] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

The Vendor Integration Engine (VIE) project has a primary feature that builds interfaces dynamically from a manifest configuration. This involves multiple updates and restructuring the layout of the DOM. This experience needs to be fast and smooth as possible. 


## Decision

React will be used to build the VIE-UI as it provides a better approach for handling this use case as we can take advantage its Fiber architecture capabilities:

React rebuilt and improved their diffing algorithm with React Fiber Architecture (as of v16). This new approach is a re-implementation of the browser's call-stack customised specifically for React components. This provides the ability to interrupt the call-stack when required and manipulate (pause, abort, reuse etc...) stack frames

The theming and look-and-feel aspects of Lightsaber are externalised to be framework agnostic so VIE-UI can utelise.

To ensure a level of parity with Angular apps and consistency with the FE Chapter's practices, the VIE-UI will also adhere to the following:

- **Coding:** Typescript will be used instead of ES8...
- **Formatting and style:** Prettier and EditorConfig
- **Linting:** TSLint
- **Enforcement and pre-commit hooks:** Husky

Adding React to our tool suite means, moving forward, we will be able to use the *'best'* tool for a task and not one single tool or approach for any given scenario.

**NB:** this decision doesn't mean React is *'better'* than Angular. 


## Consequences

- Introduction and maintenance of a different framework (or library) paradigm in the FE Chapter 

- You *'cannot'* use Lightsaber components (Angular components) in React. 

- Potential resourcing issues - finding specialists from two different paradigms 

- Dilution of our tech stack - potential outdrawn discussions about what we should use.

