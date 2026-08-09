# MBSE model walkthrough

This document explains each project diagram as a connected engineering argument rather than a collection of screenshots. All images are the original project diagrams extracted from the supplied presentation; the activity details are frames from the presentation's embedded model walkthrough.

## 1. Package diagram — model organization

![Package model organization](../assets/diagrams/01-package-model-organization.jpg)

**Purpose.** The package diagram defines where model information belongs and separates problem analysis from candidate solution definition.

**Content.** The model contains a Problem Domain and Solution Domain. Within the Problem Domain, the Black Box packages hold Stakeholder Needs, Measurements of Effectiveness, System Context, and Use Cases. The White Box packages hold Measures of Effectiveness for Subsystems, Logical Subsystems, and Functional Analysis. A System Requirements package connects those analyses to the solution.

**Engineering meaning.** This organization follows the main MagicGrid principle: understand the system from the outside before decomposing it internally. It supports navigation, ownership, and traceability across requirements, behavior, structure, and parameters.

## 2. Requirement diagrams — from needs to specifications

![Requirement hierarchy](../assets/diagrams/02-requirement-hierarchy.jpg)

The hierarchy begins with stakeholder-level needs and decomposes them into design aspects, travel-cost concerns, and vehicle requirements. The model then derives more specific requirements for the frame, battery, motor, controller, converter, and brakes.

![Requirement table](../assets/diagrams/03-requirement-table.jpg)

Key requirements visible in the source model include:

- Extend the vehicle frame by 600 mm.
- Keep total vehicle length at or below 1.8 m and wheelbase at or below 1.3 m.
- Use a frame diameter between 65 and 75 mm.
- Provide 150 mm ground clearance.
- Target a seat height of 650–700 mm and vehicle width of 350–400 mm.
- Add footrest, armrest, luggage space, and separated seating provisions.
- Use a 3 kWh lithium-ion battery concept.
- Evaluate an 8.5 kW mid-drive IPM motor requirement.
- Use a sine-wave controller for smoother acceleration.
- Provide a bidirectional converter capable of producing 12 V from a 36/48/60 V source.
- Provide combined braking and dual-disc braking.

The table view is important because it exposes identifiers, hierarchy, and requirement text in a reviewable form. It also reveals concept conflicts when compared with the functional-analysis tables; those conflicts are documented in the design-review file.

## 3. Use-case diagram — system responsibilities and actors

![Use-case diagram](../assets/diagrams/04-use-case-diagram.jpg)

**System boundary.** The electric bike-taxi context contains the system-level work needed to create and operate the concept.

**External participants.** Male and female customers, the organization, bank, vendor/manpower, driver, power source, and charging infrastructure appear outside the system boundary.

**Use cases.** The source model includes Make Bike Taxi, Make Battery, Make Charger, Make Frame, Align Seating Position, and Availability of Charging Stations. The inclusion relationships show that the vehicle concept depends on frame, battery, charger, seating, and infrastructure decisions rather than treating the bike as a single isolated component.

## 4. System-context diagram — external interfaces

![System-context diagram](../assets/diagrams/05-system-context-diagram.jpg)

The E-Chariot/E-Bike Taxi is modeled in an operating environment rather than alone. Customers request rides and make payments. The organization coordinates the service. The bank and vendor/manpower participate through payment or investment flows. The power source supplies electricity, and the driver operates the service.

This view is the boundary-control artifact: it identifies what the system owns, what remains external, and which physical, financial, or informational exchanges cross the boundary.

## 5. Activity diagram — end-to-end behavior

![Activity overview](../assets/diagrams/06-activity-diagram-overview.png)

The first part moves from the Electric Bike Taxi program through payment, organizational creation of the vehicle/service, driver assignment, vehicle readiness, and key-on activation.

![Powertrain activity detail](../assets/diagrams/06a-activity-powertrain-detail.png)

The vehicle-control portion connects the controller, converter, battery, throttle, motor, brake, horn, light, and left/right indicators. These actions represent operational dependencies; they should later be allocated to blocks and refined with typed signals and power flows.

![Service-completion activity detail](../assets/diagrams/06b-activity-service-completion.png)

The service portion covers customer booking, distance and travel-cost estimation, driver assignment, ride acceptance, payment processing, driver dispatch, and arrival at the customer location.

**Traceability role.** The activity model refines use cases into action flow and supplies the behavior that sequence diagrams, logical subsystems, and state machines must support.

## 6. Sequence diagrams — booking and payment interaction

![Ride and payment sequence](../assets/diagrams/07-sequence-ride-and-payment.jpg)

The primary sequence includes booking a ride, entering the destination, estimating and declaring travel cost, assigning a driver, declaring waiting time, accepting the order, and initiating payment. The lifelines include the customer, E-Bike Taxi, organization, and bank.

![Payment continuation](../assets/diagrams/08-sequence-payment-continuation.jpg)

Alternative fragments model payment branches. A digital-payment path checks balance, returns balance status, and confirms the ride. A cash path records payment and change. A time-related alternative allows cancellation after inactivity.

**Engineering value.** The sequence model makes interfaces testable because each message can become a software/service requirement with timing, data, and exception behavior.

## 7. Block Definition Diagram — system decomposition

![Block Definition Diagram](../assets/diagrams/09-block-definition-diagram.jpg)

The E-Bike Taxi block is decomposed into vehicle frame, horn, wheels, brake system, seat, converter, controller, motor, and battery. The block also contains concept values such as vehicle name, battery, motor, controller, converter, seat, braking system, wheels, horn, and light.

The BDD answers **what exists** and establishes reusable types. It does not by itself show how parts exchange energy or signals; that is the job of the IBD.

## 8. Internal Block Diagram — internal interfaces

![Internal Block Diagram](../assets/diagrams/10-internal-block-diagram.png)

The IBD instantiates the E-Bike Taxi and connects external power to an internal power-source element and electrical units. It also shows the vehicle-frame structure and electrical connections to the lighting system, braking system, and horn.

The ports and connectors are the foundation for interface control. A production model should refine each connector with direction, voltage/current range, signal type, connector definition, failure behavior, and verification method.

## 9. State machines — service and vehicle modes

![Service state machine](../assets/diagrams/11-state-machine-service.jpg)

The service-oriented state machine begins at Off and progresses through initialization, booking, destination entry, travel-amount estimation, payment, balance checking, driver acceptance, waiting-time estimation, e-bike operation, and ride acceptance/completion activities.

![Vehicle state machine](../assets/diagrams/12-state-machine-vehicle.jpg)

The vehicle-oriented state machine separates Off and On behavior. The On region connects starting, controller, battery, motor, and braking behavior, while the converter enables horn, light, back light, and indicator behavior with left/right indication.

These views should ultimately use named events, guards, timeouts, and entry/exit actions so they can be simulated and verified.

## 10. Parametric diagram — quantitative verification

![Parametric diagram](../assets/diagrams/13-parametric-diagram.jpg)

The model introduces two requirements:

- Total vehicle mass shall not exceed 200 kg.
- Booking a ride and completing payment shall take less than five minutes.

The E-Bike Taxi Design block owns book-time and total-mass values and is connected to frame, control-unit, converter, battery, and hub-motor design blocks. A total-mass constraint is defined as `tm = x + y + z + w`.

This is a useful verification pattern, but the model is incomplete: only the frame shows a mass value (15.0), most values remain unset, the equation has four addends while more than four component blocks appear, and the binding lines need to be completed. It should therefore be treated as a parametric prototype, not evidence that the 200 kg requirement has already been verified.

## End-to-end traceability

The intended engineering chain is:

```text
Stakeholder evidence
    → prioritized needs (Kano and QFD)
    → system requirements
    → use cases and system context
    → functional/activity behavior
    → logical and physical blocks
    → interfaces and state behavior
    → parametric verification
```

The next modeling increment should make that chain explicit with `deriveReqt`, `refine`, `satisfy`, `verify`, and allocation relationships plus requirement-to-model matrices.

