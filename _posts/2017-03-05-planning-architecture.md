---
layout: post
title: "Planning Architecture Overview"
description: "An overview of the planning architecture for a fully autonomous sailboat."
tags: [planning, micro, local, global]
---

This page provides an overview of the planning architecture for a fully autonomous sailboat.

## Background

This system has a expansive scope that includes finding a least-cost path between any two points on the planet while avoiding dynamic obstacles in real-time considering the kinodynamic constraints of the vessel. To our knowledge, no systems with comparable scope exist, let alone for a sailboat, making it first in class. As such, it will be evaluated component-wise against existing systems with less functionality. More on the evaluation method [here]({% post_url 2017-02-11-evaluation-scenario %}).

### Existing Solutions
Two-stage planning systems are widely used in robotics, where the high level, low-resolution planner is referred to as Global and the high-resolution planner is named Local. Historically, the operation of these systems is confined to a relatively small areas, meaning that the Global planner handles an area between tens of meters (e.g. a room) and maximally tens of kilometers across (e.g. a bounded outdoor area). Though autonomous cars require navigation solutions on a global scale, the motion is restricted to a graph that is trivially constructed from the physical road network rather than in free space.

In contrast to most systems, our Global planner is a true, planet-level routing solution. Because of this, the distances between points in the resulting path are great...

_TODO: Expand on the limitations of existing solutions. Primarily naive obstacle avoidance._

In tests, we have consistently seen super-human performance with our previous generation control systems. This is primarily because of the vessel's ability to precisely sense pose state and execute smooth actions in response. However, in practice there exists disparity between human-controlled and existing autonomous sailboats because of limited environmental perception and planning capabilities.

The deliverables of this work are significant improvements to all levels of planning, enabling safe autonomous sailboat operation both from .

## Challenges
Because of no-go zones, sailing comes with a unique control challenge, requiring discontinuous "actions".

_TODO: Expand on this_

## Global
This system is tasked with finding a least-cost path between any two points on the planet, as long as they are in connected water bodies.

Our solution provides a method for optimal planet-level planning with dynamic risks. Using a geodesic grid, the issues introduced by map projections are avoided and optimal paths can easily be generated across the poles.  Moreover, the nearly equidistant grid naturally reduces the disparity between the time taken to travel edges. This is critical being able to effectively use time-based risks such as weather.

_TODO: Expand on the benefits of using a HexPlanet and what it is_

_TODO: Expand on:_
- Augment the HexPlanet graph by making distant neighbours available. This will double the angular resolution available to the vessel at any time, offering the following benefits:
  - Significantly reduce the tendency for the path to travel along the HexPlanet's original geometry.
  - Increase the accuracy/benefit of modeling vessel performance against apparent wind.
  - Note: watch for problems with hitting obstacles by "hopping" over them.

_TODO: Expand on:_
- Assign a value to time steps.
- Proposed strategy for an accurate time step length:
  - Store the minimum & maximum edge lengths seen while computing them all at the end of HexPlanet generation.
  - Set the time step length should be the average time required to travel one-half of their difference.
    - This will allow for the accurate propagation of time as planning goes on, keeping dynamic risks in sync with the time at which the vessel is believed to be at the given location.
- Evaluate the runtime and solution performance of the planner using various time step lengths.
  - Compare how disparate the solution time is between using the accurate time step length computation and one time step per any edge.

The cost for each edge is the sum of the following:

$$EdgeDistance$$

- $$EdgeDistance$$ is the corrected distance (in meters) that must be travelled for this edge given the predicted wind direction.
- Because of no-go zones we must compute the true distance that must be travelled
  - e.g. If the edge is going up wind then the true distance is greater than the great circle distance.
- A lower bound on this value can be easily computed statically to provide the basis for an effective heuristic towards the goal.

$$(EdgeTime - kFastestEdgeTime) * kTimeWeight$$

- $$EdgeTime$$ is the time required to traverse $$EdgeDistance$$ given the predicted weather conditions.
- This allows us to generate routes that maximize the performance of the vessel.
  - The model used for computing velocity will likely be a implemented as a polar speed diagram (look up table) since the motion is considered holonomic.
- As with the distance heuristic, the corrected distance must be used to compute the time cost.
- $$kFastestEdgeTime$$ is the shortest amount of time that any edge in the HexPlanet can be traversed.
  - Subtracting this improves the heuristic's ability to closely model the true cost of any edge. In other words, it avoids the inflation of true cost relative to the heuristic.

$$WeatherRisk * kWeatherWeight$$

- $$WeatherRisk$$ is calculated using the expected wind speed and wave height at the time the vessel will be there.
- This component should only be non-zero if there is a considerable risk.

$$TopographicalRisk * kTopoWeight$$

- $$TopographicalRisk$$ is used to model the static risk associated with specific areas.
  - e.g. Shipping lanes, shallow water, narrow passages, fishing areas.

## Local
_TODO: Expand on this_

## Micro
_TODO: Expand on this_

_Last updated: March 9th, 2017_
