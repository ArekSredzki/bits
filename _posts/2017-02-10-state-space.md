---
layout: post
title: "State Space & Boat Model"
description: "A definition of the scenario state space & boat model used."
tags: [dynamics, control, simulation]
---

To effectively design a motion planning solution, we must first define the target state space as well as a model for the vessel. This post contains a working definition of the state space & boat model that I will use in my conference paper.

## Background

A characteristic somewhat unique to sailboats is the constraint of "no-go" zones. These are sectors of a circle relative to the wind in which the forward-going performance of the vessel is very poor. All sailboats have a no-go zone that is "into" the wind that is referred to "irons". The angular length, or size, of irons is heavily dependent on the physical properties of a vessel. However, it usually extends roughly $$45\unicode{xb0}$$ to either side of the wind. Depending on the construction of the vessel. In addition to the nonholonomic motion dynamics of the vessel, its propulsion is heavily dependent on its angle relative to the wind.

## State Space
### Vessel Configuration

### Obstacle Data
Obstacle data will be available in the form of a three dimensional occupancy grid.

The first two dimensions are in 2D (x, y) Euclidean space, with an additional binned time dimension.

The height and width of each entry in the Euclidean space of the occupancy grid is 1m.

The time interval for obstacle data is 1 second.

## Assumptions
In order to simplify and accelerate the initial development of the system, the following assumptions are made. Each of these provides room for improvement in future works.

- The boat state and occupancy grid available to the planning/control systems are without noise.
- The information in each cell of the occupancy grid is binary.

## Boat Model
In order to maximize our ability to accurately compare the performance of various controllers/planners, we define a complex physics based model for the vessel. It is worth noting that the following model is likely to be more complex than those used for the realtime systems. This will allow us to ensure that any assumptions we make to improve the performance of on-vessel systems doesn't compromise on performance.

_TODO: Expand on the following discussions from meeting 4 once further experimentation has been completed_

- Discussed various approaches to simulation
  - Discrete time vs. ODE45
  - The degree of model accuracy that should be used in simulations vs planning
    - Physics based model vs polar speed diagram (Empirical vs Theoretical)
- Control integration with planning
  - No-go zones make sailing an interesting challenge. When an action is to be taken, it's
  - Mode based control
    - Line following & then "tack"
    - Though this simplifies the necessity for the generated path to be perfectly executable
    - Problematic in the case of realtime obstacles
      - Consider interrupt-based exit of modes in the presence of an obstacle being detected in the area left for it.
  - More exploration must be done in this matter. Simulating various approaches is likely the best way to move forward.


_Last updated: March 9th, 2017_
