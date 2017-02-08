---
layout: post
title: "Behaviour Specific Scenarios"
description: "A definition of target scenarios, in which candidate control/planning systems will be evaluated."
tags: [control, simulation]
---

In order to effectively design control and planning systems for an agent, we must clearly define the scenarios in which it will be evaluated and proven to be stable. What is described below is a comprehensive test suite and will likely be refined

## Generic obstacle scenario classes
- $$a$$ No obstacles
- $$b$$ Generic static obstacle test
  - $$b1$$ Static random sparse
  - $$b2$$ Static random dense
- $$c$$ Generic dynamic obstacle test (continuous heading)
  - $$c1$$ Other agent bow
  - $$c2$$ Other agent stern
  - $$c3$$ Other agent port
  - $$c4$$ Other agent starboard
  - $$c5$$ Multiple other agents, one from port and one from starboard

## Core scenarios
Each of the following scenarios defines various parameterizations. All of the generic obstacle scenarios classes defined above are applied to each combination of the parameters listed below.

Let
- $$\alpha$$ be the apparent wind angle relative to the stationary vessel's heading at time $$t = 0$$.
- $$\phi$$ be the apparent wind angle to the waypoint.
- $$\sigma$$ be half of the upwind no-go angle.
- $$\tau$$ be half of the downwind no-go angle.

$$\alpha \in \{-170, -160, \ldots, 180\}$$

$$\sigma \in \{35, 45, 55\}$$

$$\tau \in \{5, 10, 15\}$$

### Scenario 1: Reaching point to point

$$\phi \in \{60, 75, 90, 105, 120, 135, 150\}$$

### Scenario 2: Upwind point to point

$$\phi \in \{-20, -10, 0, 10, 20\}$$

### Scenario 3: Downwind point to point

$$\phi \in \{-160, -170, 180, 170, 160\}$$

## Test set caveats
Additional test sets must be defined for the following:
- Race course performance evaluation
