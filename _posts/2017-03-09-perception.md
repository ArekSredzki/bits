---
layout: post
title: "Perception for Autonomous Sailboats"
description: "An overview of sensing in autonomous sailboats."
tags: [perception, control]
---

> This is an early draft.

## Background
TBD

## Pose

### Position
- D-GPS is more than accurate enough for most applications
  - Clear view of the sky allows for cm-level accuracy or better in most conditions.
  - High data rate up to 20Hz
- Further localization is hard because everything is moving
- Heading over ground
- Speed over ground
- Course over ground
- Raw 6DOF IMU data

### Wind
On vessel readings are apparent values, but position data can be used to find true values.
Though sailboats generally sail by apparent wind, very good results can be determined from noisy sensors by filtering on the true values and then converting back to relative for control.
- With this method, one can get accurate apparent wind values at the same rate as position data.
- This sensor fusion architecture significantly improves the performance of simple and advanced controllers alike.

#### Mechanical wind sensor
- Provides wind direction and sometimes wind speed, but are measured separately
- Inexpensive
- Delicate in practice and can be easily damaged
  - Not recommended for mission-critical usage or long distance voyages

#### Ultrasonic wind sensor
- Very accurate
- Expensive (~$800 CAD for a model with readings at 4Hz)
- Resilient to physical damage & noisy readings from water
- Apparent wind speed
- Apparent wind angle
- Also provides air temperature readings

## Environment

### Obstacles
#### AIS
- Generally radio, sometimes available over satellite as well
- Relatively long range (~10–20 nautical miles)
- Widely used by ships because of legal requirements but cannot be used as the sole ship detection method
  - Some ships will break the law and not transmit (especially fishing vessels)
  - Small vessels do not require AIS
- Minimally provides:
  - Vessel identifier & position of ship
  - Position (GPS Coordinates) & accuracy
    - No *guarantees* for accuracy
- Variably provides additional information, notably:
  - Navigation status
  - Heading over ground
  - Speed over ground
  - Course over ground
  - Data timestamp
- Transmits data every 2 to 10 seconds depending on a vessel's speed while underway, and every 3 minutes while a vessel is at anchor.
- Every 6 minutes (variably) transmits:
  - Vessel dimensions
  - Vessel type and payload
  - Location of reported position relative to ship center
  - Type of positioning system
  - Destination
- Can be used by adversaries

#### Infrared
- Passive
- Low power
- Works day & night but needs different modes of operation
- Detects near-surface obstacles like logs that are hard to detect just by their shape
- Machine learning works
- Artifacts from reflections (sunspots)
- Poor performance in rain

#### LiDAR
- Potential for very high reliability of distinctly shaped obstacles
- Works day & night
- No returns from still water
  - Less processing required
- Requires a way of distinguishing waves from

### Weather
- 10 days of predictions are available from various services such as PredictWind.

### Waves
- Hard
- Not critical for good performance in most conditions


_Last updated: March 9th, 2017_
