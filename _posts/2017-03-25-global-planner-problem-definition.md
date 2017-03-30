---
layout: post
title: "Global Planner Problem Definition"
description: "The latest version of the problem definition section of the global planning paper."
tags: [planning, global]
---

This page provides the problem definition for the global planning paper.

## Background

This system has a expansive scope that includes finding a least-cost path between any two points on the planet while avoiding dynamic obstacles in real-time considering the kinodynamic constraints of the vessel. To our knowledge, no systems with comparable scope exist, let alone for a sailboat, making it first in class. As such, it will be evaluated component-wise against existing systems with less functionality. More on the evaluation method [here]({% post_url 2017-02-11-evaluation-scenario %}).


## Challenges
Because of no-go zones, sailing comes with a unique control challenge, requiring discontinuous "actions".

_TODO: Expand on this_

## Global
This system is tasked with finding a least-cost path between any two points on the planet, as long as they are in connected water bodies.

Our solution provides a method for optimal planet-level planning with time-dependent anisotriopic risks. Using a geodesic grid, the issues introduced by map projections are avoided and optimal paths can easily be generated across the poles.  Moreover, the nearly equidistant grid naturally reduces the disparity between the time taken to travel edges. This is critical being able to effectively use time-based risks such as weather.

_TODO: Expand on the benefits of using a geodesic grid and what it is_

_TODO: Expand on:_
- Augment the geodesic graph by making distant neighbours available. This will double the angular resolution available to the vessel at any time, offering the following benefits:
  - Significantly reduce the tendency for the path to travel along the geodesic grid's original geometry.
  - Increase the accuracy/benefit of modeling vessel performance against apparent wind.
  - Note: watch for problems with hitting obstacles by "hopping" over them.

_TODO: Expand on:_
- Assign a value to time steps.
- Proposed strategy for an accurate time step length:
  - Store the minimum & maximum edge lengths seen while computing them all at the end of geodesic grid generation.
  - Set the time step length should be the average time required to travel one-half of their difference.
    - This will allow for the accurate propagation of time as planning goes on, keeping dynamic risks in sync with the time at which the vessel is believed to be at the given location.
- Evaluate the runtime and solution performance of the planner using various time step lengths.
  - Compare how disparate the solution time is between using the accurate time step length computation and one time step per any edge.

The cost function, $$C$$, is defined as the sum of the following components:

$$C_1(x, t, u) = Time(x, t, u)$$

$$C_2(x, t, u) = Time(x, t, u)$$

$$Time(x, t, u) = CorrectedDistance(x, u, Weather.Wind.Direction(x, t)) / PolarSpeedTable(x, Weather.Wind.Speed(x, t))$$

- $$Time$$ is the time required to traverse $$EdgeDistance$$ given the predicted weather conditions.
- This allows us to generate routes that maximize the performance of the vessel.
  - The model used for computing velocity will likely be a implemented as a polar speed diagram (look up table) since the motion is considered holonomic.
- As with the distance heuristic, the corrected distance must be used to compute the time cost.
- $$kFastestEdgeTime$$ is the shortest amount of time that any edge in the geodesic grid can be traversed.
  - Subtracting this improves the heuristic's ability to closely model the true cost of any edge. In other words, it avoids the inflation of true cost relative to the heuristic.
- $$WeatherRisk$$ is calculated using the expected wind speed and wave height at the time the vessel will be there.
- This component should only be non-zero if there is a considerable risk.

_TODO: TOPO Risk_
- $$TopographicalRisk$$ is used to model the static risk associated with specific areas.
  - e.g. Shipping lanes, shallow water, narrow passages, fishing areas.


## Proposed solutions

### Pseudocode

```cpp
function A*(start, goal)
    Priority Queue open_set = {};
    TimeIndexValueMap visited;

    // Add start state.
    open_set.Add(start_, 0, heuristic_.calculate(start_, target_));
    visited[start_, 0] = VisitedStateData{0, {kInvalidHexVertexId, 0}};

    // TODO(areksredzki): There is currently no check to see that the location is at all reachable.
    // Since there are no bounds on the time dimension, the pathfinder will run forever.
    while (!open_set.empty()) {
      AStarVertex current = open_set.top();
      open_set.pop();

      // The best data for this IdTimeIndex up util now.
      VisitedStateData current_data = visited[current.id_time_index()];

      if (current.hex_vertex_id() == target_)
        return ConstructPath(current.id_time_index(), visited);
      }

      const HexVertex &vertex = planet_.vertex(current.hex_vertex_id());

      // Process edges to neighbours
      for (HexVertexId neighbour_id : vertex.neighbours) {
        // Calculate the cost and time between the current vertex and this neighbour.
        auto cost_time = cost_calculator_.calculate_target(current.hex_vertex_id(), neighbour_id, current.time());

        // Total cost from the start to this neighbour.
        uint32_t neighbour_cost = current_data.cost + cost_time.cost;

        // Heuristic cost from this neighbour to the target.
        uint32_t heuristic_cost = heuristic_.calculate(neighbour_id, target_);

        AStarVertex::IdTimeIndex neighbour_id_time_index(neighbour_id, cost_time.time);

        auto item = visited.find(neighbour_id_time_index);

        if (item == visited.end() || neighbour_cost < item->second.cost) {
          if (item == visited.end()) {
            // Create the VisitedData instance.
            visited[neighbour_id_time_index] = VisitedStateData{neighbour_cost,  current.id_time_index()};
          } else {
            // Update the members of the existing VisitedData instance.
            item->second = {neighbour_cost,  current.id_time_index()};
          }

          // Add the neighbour to the open set in place (no copy/move operations apart from shuffling the priority_queue).
          open_set.emplace(neighbour_id_time_index, neighbour_cost + heuristic_cost);
       }
      }
    }

    return failure

function ConstructPath(cameFrom, current)
    total_path = [current]
    while current in cameFrom.Keys
        current = cameFrom[current]
        total_path.append(current)
    return total_path
```

_Last updated: March 30th, 2017_
