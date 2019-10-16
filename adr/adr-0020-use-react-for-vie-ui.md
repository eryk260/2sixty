# ADR 0019: Use React for the VIE-UI

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

The Vendor Integration Engine (VIE) project has a primary feature that builds interfaces dynamically from a manifest configuration. This involves updates and restructuring the layout of the DOM. This experience needs to be fast and smooth as possible. React provides a better approach for handling this use case. 

React's previous implementation for manipulating the DOM, made use of a tree of React elements (or virtual DOM) to perform a reconciliation exercise using a diffing algorithm to determine what's changed, where, and the most efficient method to patch through an update to the individual node and not rebuild the entire DOM tree. 

Manipulating a virtual DOM with this approach is very cheap and faster than working directly on the actual DOM. 

React rebuilt and improved their diffing algorithm with React Fiber Architecture (as of v16). The previous approach recursively traversed a linked list tree based on the browser's default call stack. This new approach with React Fiber is a re-implementation of the browser's call-stack customised specifically for React components. This provides the ability to interrupt the call stack when required and manipulate (pause, abort, reuse etc...) stack frames. 

In future, Angular will be introducing the Ivy renderer (and bundler) which goes a long way in trying to improve the initial load, the way it handles change detection and general performance of Angular. Ivy is yet to reach general availability (GA).


## Decision

React will be used to build the VIE-UI to take advantage of the Fiber architecture capabilities.

The theming and look-and-feel aspects of Lightsaber are externalised to be framework agnostic so VIE-UI can utelise.

To ensure a level of parity with Angular apps and consistency with the FE Chapter's practices, the VIE-UI will also adhere to the following:

- **Coding:** Typescript will be used instead of ES8...
- **Formatting and style:** Prettier and EditorConfig
- **Linting:** TSLint
- **Enforcement and pre-commit hooks:** Husky

Adding React to our tool suite means, moving forward, we will be able to use the *'best'* tool for a task and not one single tool or approach for any given scenario.

**NB:** this decision doesn't mean React is *'better'* than Angular. 


## Consequences

- Introduction and maintenance of a different framework (or library) paradigm in the FE Chapter - it's worth noting that this is currently the reality with the Proteus stack.

- You *'cannot'* use Lightsaber components (Angular components) in React. We would have to move towards Angular Elements, which is a pattern for packaging Angular components as web components which are framework agnostic. 

