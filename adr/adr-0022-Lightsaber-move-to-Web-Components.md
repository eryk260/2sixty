
# ADR 0022: Lightsaber move to Web Components

## Status

- [ ] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

Our reusable and themeable components library, Lightsaber (LS), is an Angular Storybook providing support for Angular apps only. As Angular had been our de facto Frontend framework at 2Sixty, the guiding principles of LS (enabling teams to iterate and deliver faster) were being realised. 

As we grow as a business, we are encountering different scenarios and tech stacks. React is now a part of our stack and with the evolution of 2Sixty in the coming months, React's footprint in the Frontend will increase.

As such, we need LS to become framework agnostic to remain relevant and be able to cater to all Frontend stacks.

## Decision

We will transform LS into [Web Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components) to make it framework agnostic in order to provide universal support for all Frontend frameworks (Angular, React, Ember etc...) - a second generation of the library.

**Interim step:** Currently we have an interim solution which extracts the look-and-feel aspects of Lightsaber into what we call the 2Sixty universal UI Kit - this is consumed by our React app and can be consumed by any other stack.

**Next steps:** We will use Angular Elements to transpile LS components into Web Components. All future LS components built in the Frontend will follow the Web Components pattern.


## Consequences

This is a new pattern in the Frontend Chapter - Chapter Lead would need to ensure Devs are up-skilled.


