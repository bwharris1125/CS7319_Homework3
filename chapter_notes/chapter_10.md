## Chapter 10: Layered Architecture Style

### Layered Architecture Summary
The Layered Architecture _(n-tiered)_ - standard for most applications, because of simplicity, familiarity, and low cost. However, this is at the trade-off of modulatiry, maintainability, depoloyment, and scalability.
The style also falls into several architectural anti-patterns (architecture by implication, accidental architecture).

Most layered architectures are devided by a well-defined set of responsibilities. A common example of these functions are: presentation, business, persistence, and database. 

The layered architecture is a technically partitioned architecture (as opposed to domain-partitioned architecture).
Groups of components, rather than being grouped by domain, are grouped by their technical role in the architecture. As a
result, any particular business domain is spread throughout all of the layers of the architecture. A domain-driven
design does not work well with the layered architecture style.

Each layer can be either closed or open.
- closed - a request moves top-down from layer to layer, the request cannot skip any layers (this is the standard / default approach)
- open - the request can bypass layers (fast-lane reader pattern)

## Layer Isolation
The layers of isolation - changes made in one layer of the architecture generally don't impact/affect components in
other layers. Each layer is independent of the other layers, thereby having little or no knowledge of the inner workings
of other layers in the architecture. Complying with this provides a replacability benefit, where a component can be easily replaced or exhanged due to a clearly defined barrier. Violation of this concept produces very tightly coupled application with layer
interdependencies between components This type of architecture becomes very brittle, difficult and expensive to change.

## Overview
This architecture makes for a good starting point for most applications whe it is not known yet exactly which
architecture will ultimately be used. Be sure to keep reuse at minimum and keep object hierarchies. A good level of
modularity will help facilitate the move to another architecture style later on.

Watch out for the architecture sinkhole anti-pattern - this anti-pattern occurs when requests move from one layer to
another as simple pass-through processing with no business logic performed within each layer. Failure to do this results in performance and scalability issues due 
to increased processing and memory requirements to accomodate this. For example, the
presentation layer responds to a simple request from the user to retrieve basic costumer data.