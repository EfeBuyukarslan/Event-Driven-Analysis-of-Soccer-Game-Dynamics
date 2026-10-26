# Event-Driven Analysis of Soccer Game Dynamics

Detecting when the dynamics of a live soccer match shift, and testing whether in-game events explain the shift.

Course project, IE 423 (Quality Engineering), Boğaziçi University.

## The problem

Momentum in a match is easy to talk about and hard to measure. This project treats it as a process control problem: define a metric for how a match is flowing, detect statistically significant shifts in it, and check whether those shifts line up with events such as goals.

## Approach

**Choosing a metric.** Attack counts arrive cumulatively, so three transformations were built and compared:
1. attacks per minute ratio
2. incremental change in attacks (differencing)
3. normalised attack difference

Approaches 1 and 3 flagged nearly all their out-of-control points in the opening minutes — an artefact of how the metrics behave on little data, not real shifts. They were discarded. Differencing behaved better, but since attack counts never decrease, every violation sat above the upper limit. Taking the difference between the home and away differenced series fixed that, giving a metric that can breach either limit. This became `Differenced_attacks_diff`.

**Control charts.** Each match is examined in rolling 15-minute windows with 3-sigma control limits, so the limits adapt as the match progresses. Out-of-control points are recorded along with their direction, upper or lower.

**Linking shifts to events.** Event data was converted from cumulative counts into per-minute indicators of whether something happened. For each out-of-control point, events in the following window were recorded, with the lag between the shift and the event.

**Hypothesis tested.** If a team scores while the match is level, its share of attacking play increases afterwards. Most examined cases support this. The exceptions are instructive: one goal came in the opening minutes where dynamics are volatile, and in another the goal and the detected shift fell in different halves, so neither case bears on the hypothesis.

## Files

- `Project Code.ipynb` — metric construction, control charts, event analysis
- `Project Code.html` — rendered notebook
- `Project Report.pdf` — written report

## Tools

Python · pandas · NumPy · Matplotlib · statistical process control
