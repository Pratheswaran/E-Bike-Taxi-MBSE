# Requirements and design review

This document separates the original concept evidence from the decisions that still require engineering closure.

## Stakeholder baseline

| Group | Count |
|---|---:|
| Students | 69 |
| Research scholars | 4 |
| Employees | 28 |
| **Total** | **101** |

| Gender recorded in the source survey | Count |
|---|---:|
| Male | 66 |
| Female | 35 |

The source presentation records different needs by group. Students emphasize fast travel, comfort, and low cost; employees emphasize arriving on time, luggage/bag carrying, footrests, and cost; women emphasize avoiding body contact, personal safety, luggage, an internal footrest, and low cost. These notes are consolidated into low cost, comfort, body-contact avoidance, safety, and on-time arrival.

## Requirements baseline extracted from the model

| Area | Requirement or candidate value | Status |
|---|---|---|
| Frame extension | Extend frame length by 600 mm. | Candidate requirement |
| Overall length | Not more than 1.8 m. | Candidate requirement |
| Wheelbase | Not more than 1.3 m. | Candidate requirement |
| Frame diameter | 65–75 mm. | Candidate requirement |
| Ground clearance | 150 mm. | Candidate requirement |
| Seat height | 650–700 mm. | Candidate requirement |
| Vehicle width | 350–400 mm. | Candidate requirement |
| Passenger accommodation | Footrest, armrest, luggage space, separate seating. | Candidate requirement |
| Battery energy | 3 kWh lithium-ion. | Candidate requirement |
| Battery voltage | 48 V. | Functional-table concept |
| Motor — requirement view | 8.5 kW mid-drive IPM. | Competing concept |
| Motor — functional view | BLDC hub motor, 3.0–4.4 kW peak, 140 Nm peak torque. | Competing concept |
| Controller | Sine-wave controller for smoother acceleration. | Candidate requirement |
| Converter | Bidirectional; 36/48/60 V to 12 V. | Candidate requirement |
| Seat | Separate dual seat. | Functional-table concept |
| Brake — requirement view | Combined braking system and dual disc brakes. | Competing concept |
| Brake — functional view | Drum brake with regenerative-braking narrative. | Competing concept |
| Wheel/tyre | R10 wheel and R12 tyre. | Needs reconciliation |
| Luggage carrier | 200 × 200 mm. | Functional-table concept |
| Total mass | Not more than 200 kg. | Parametric requirement; not yet verified |
| Booking and payment | Less than five minutes. | Parametric requirement; not yet verified |

## Architecture review

### What is already strong

- Stakeholder evidence is visible and quantified.
- Kano and QFD provide an auditable transition from needs to priorities.
- The model spans requirements, behavior, structure, interfaces, states, and parameters.
- External service actors and physical vehicle elements are both represented.
- The model recognizes electrical auxiliaries and service/payment behavior, not only chassis hardware.

### Configuration items requiring closure

#### 1. Traction architecture

The model contains both a BLDC hub-motor concept and an 8.5 kW mid-drive IPM requirement. These choices affect frame geometry, unsprung mass, gearing, thermal management, controller selection, serviceability, cost, and efficiency. A trade study should compare them against weighted measures of effectiveness.

#### 2. Motor power

The 3.0–4.4 kW and 8.5 kW values are not interchangeable. The project needs a vehicle-level performance model covering gross mass, gradeability, top speed, acceleration, wheel radius, drivetrain efficiency, duty cycle, and thermal limits before motor power is baselined.

#### 3. Braking architecture

The requirements show combined dual-disc braking, while the functional table lists a drum brake and describes regenerative braking. The design must distinguish service brakes, parking brake, regenerative braking, brake blending, and the safety fallback when regenerative braking is unavailable.

#### 4. Wheel and tyre sizing

R10 wheel and R12 tyre entries require clarification. The design baseline should use complete rim and tyre designations plus load rating, speed rating, rolling radius, compatibility, and clearance.

#### 5. Electrical interfaces

The IBD should be extended with nominal voltage, maximum current, fuse/protection, connector type, signal direction, and controller I/O. The battery, converter, controller, motor, lighting, indicators, horn, and braking interfaces should each have an interface-control definition.

#### 6. Parametric completeness

The total-mass constraint currently uses four variables while the design presents more component blocks. Most mass values are unset, so the model cannot yet verify the 200 kg requirement. A complete mass roll-up should include frame, battery, motor, controller, converter, brakes, wheels/tyres, seats, auxiliaries, luggage carrier, passengers, and design margin.

## Recommended verification matrix

| Requirement | Analysis | Inspection | Demonstration | Test |
|---|:---:|:---:|:---:|:---:|
| Frame and seating dimensions | ✓ | ✓ |  |  |
| Passenger separation and ergonomics | ✓ | ✓ | ✓ | ✓ |
| Total mass ≤ 200 kg | ✓ |  |  | ✓ |
| Booking/payment < 5 min | ✓ |  | ✓ | ✓ |
| Braking performance | ✓ | ✓ |  | ✓ |
| Electrical conversion to 12 V | ✓ | ✓ |  | ✓ |
| Motor performance and gradeability | ✓ |  | ✓ | ✓ |
| Luggage-carrier load capacity | ✓ | ✓ |  | ✓ |
| Lighting, horn, and indicators |  | ✓ | ✓ | ✓ |

## Recommended next model increment

1. Assign unique requirement IDs and measurable acceptance criteria.
2. Create a requirement traceability matrix from stakeholder needs to system requirements.
3. Allocate every activity action to a logical subsystem.
4. Add typed item flows and interface blocks to the IBD.
5. Complete a motor/battery/brake trade study using QFD/MoE weights.
6. Baseline one configuration and update all diagrams consistently.
7. Complete mass, range, energy, gradeability, braking, and service-time parametrics.
8. Add `satisfy` and `verify` relationships and generate a verification matrix.

