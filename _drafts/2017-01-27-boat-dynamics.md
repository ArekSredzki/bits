---
layout: post
title: "Modeling Sailboat Dynamics"
description: "A discussion of how to model sailboat dynamics for use in developing motion planning solutions."
tags: [dynamics]
---

In order to do effectively design a motion planning solution, we must first determine the dynamic constraints of the vessel.

A characteristic somewhat unique to sailboats is the constraint of no-go zones. These are sectors of a circle relative to the wind. In addition to the nonholonomic motion dynamics of the vessel, it cannot move in
