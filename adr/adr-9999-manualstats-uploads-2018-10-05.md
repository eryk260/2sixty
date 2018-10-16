<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 9999: Manual Upload service

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

Many products and initiatives at Essence require some sort of file upload. The file often needs to be checked for valididty,
formatting and some business rules. Essence has several current implementations of this, and at least one proposed
example to my knowledge.

### Olive MediaConnectors

[https://o.essencedigital.com/mediaconnector](https://o.essencedigital.com/mediaconnector)

This is the point of entry for manual stats upload in to Olive that come via Intern or the Sheet add-on. Capable of validation
of fields and custom business logic with multiple different templates available, although it has basically been abandoned
and is only used for delivery and events uploads. In terms of moving Olive to the cloud MediaConnectors may be a blocker
as it is fairly wrapped up in MIS3.

Notes:

- Multiple "templates" to define different type of uploads
- Field validation logic is possible, along with custom field validation like MRF codes
- Wrapped up in MIS3 and the Olive platform

### Camry

[https://camry.essencedigital.com](https://camry.essencedigital.com)

Camry uploads initially went through the Olive MediaConnectors interface, however it was found to be too difficult to
develop on the Olive platform. Camry can essentially be viewed as Olive MediaConnectors 2.0, and has the same capabilities
and more on top.

Notes:

- Multiple "templates" to define different type of uploads
- Required and optional columns
- Field validation logic is possible, along with custom field validation like MRF codes
- Row validation logic is possible (fields depending on other fields).
- Whole file validation / mutation is possible.
- Gzip compression in browser to facilitate larger and faster uploads.

### ROMI

[ROMI Data Collection](https://docs.google.com/document/d/1vfo_YlAz7KfRBdGM65929ydf79fSGVQiF5nP-zE59_I/edit)

Analytics are looking for a way to perform manual uploads. The Camry uploader already supports many of the features
that they need, and adding the extra features should be trivial.

## Decision

Take the existing Camry frontend and upload service and convert it into a generic 2sixty csv uploading platform.

## Consequences

The Camry project will change slightly, becoming just the ETL part of the process. The uploading frontend of camry will be
rebranded and decoupled from the ETL backend and provide upload events for other backends to consume.
