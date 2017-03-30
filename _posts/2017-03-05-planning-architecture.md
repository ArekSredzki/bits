---
layout: post
title: "Planning Architecture Overview"
description: "An overview of the planning architecture for a fully autonomous sailboat."
tags: [planning, micro, local, global]
---

This page provides an overview of the planning architecture for a fully autonomous sailboat.

## Background

This system has a expansive scope that includes finding a least-cost path between any two points on the planet while avoiding dynamic obstacles in real-time considering the kinodynamic constraints of the vessel. To our knowledge, no systems with comparable scope exist, let alone for a sailboat, making it first in class. As such, it will be evaluated component-wise against existing systems with less functionality. More on the evaluation method [here]({% post_url 2017-02-11-evaluation-scenario %}).

### Previous Works
Two-stage planning systems are widely used in robotics, where the high level, low-resolution planner is referred to as Global and the high-resolution planner is named Local. Historically, the operation of these systems is confined to a relatively small areas, meaning that the Global planner handles an area between tens of meters (e.g. a room) and maximally tens of kilometers across (e.g. a bounded outdoor area). Though autonomous cars require navigation solutions on a global scale, the motion is restricted to a graph that is trivially constructed from the physical road network rather than in free space.

In contrast to most systems, our Global planner is a true, planet-level routing solution. Because of this, the distances between points in the resulting path are great...

_TODO: Expand on the limitations of existing solutions. Primarily naive obstacle avoidance._

In tests, we have consistently seen super-human performance with our previous generation control systems. This is primarily because of the vessel's ability to precisely sense pose state and execute smooth actions in response. However, in practice there exists disparity between human-controlled and existing autonomous sailboats because of limited environmental perception and planning capabilities.

The deliverables of this work are significant improvements to all levels of planning, enabling safe autonomous sailboat operation both from .

## Challenges
Because of no-go zones, sailing comes with a unique control challenge, requiring discontinuous "actions".

_TODO: Expand on this_

## Global
See [this page]({% post_url 2017-03-25-global-planner-problem-definition %}).

## Local
_TODO: Expand on this_

## Micro
_TODO: Expand on this_

_Last updated: March 9th, 2017_
