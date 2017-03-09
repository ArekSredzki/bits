---
layout: post
title: "Evaluation Scenario"
description: "A definition of the main scenario that will be used to evaluate my planning system for a fully autonomous sailboat in an upcoming paper."
tags: [control, simulation]
---

To effectively capture the performance of an advanced local planner for a sailboat in a way that can be presented in a conference paper, a single comprehensive test is required. This post contains a working definition of the test that I will use in my conference paper.

It's worth noting that in an earlier post, [behaviour specific scenarios]({% post_url 2017-02-01-behaviour-specific-scenarios %}), I defined a large set of test scenarios for evaluating sailboat behaviour in a localized setting. Though valuable, that testing suite does not concisely capture the performance of a planning solution and instead is more valuable for algorithm development.

## Overview
The target state space of my current research explicitly defined in [this post]({% post_url 2017-02-10-state-space %}) and continues to be used in this test.

In order to comprehensively capture the performance and validity of a fully autonomous sailboat controller we will take from the methods that have been developed for evaluating humans completing the same task.

## Course

Explicitly, the test will mimic the Triangle-Windward-Leeward Course as defined by World Sailing in the [Racing Rules of Sailing 2017 - 2020](http://www.sailing.org/documents/racingrules/). Since mark-roundings are out of the scope of the challenge being evaluated, involve reaching waypoints arranged in the shape. The controller must simply get the boat within $$5 m$$ of each waypoint before heading to the next. The initial pose will have the boat going close hauled on a starboard tack with a velocity of 0 at the middle of the start line. The last point will be at the middle of the finish line. The distance between points 1 & 3 is 5 km.

$$TrueWindSpeed = 10 kts$$

$$TrueWindAngle = 0$$

$$TrueCurrentSpeed = 1 kts$$

$$TrueCurrentAngle = \pi/4$$

![Triangle-Windward-Leeward Course](/assets/2017-02-11-evaluation-scenario/race_course.svg)

## Obstacles
In order to test the obstacle avoidance capabilities of the controller, randomly emitted obstacles will be generated from one of the edges of the "world" at a period of 10s.

_Last updated: March 7th, 2017_
