# fleet-midi-cc: Theory

_Musical theory and ternary logic behind the cc agent._

---

## Control Change Smoothing

The EMA (Exponential Moving Average) filter: `smoothed = α × raw + (1-α) × previous`

Where α (alpha) controls the smoothing factor. α=0.3 means moderate smoothing,
α=0.1 means heavy smoothing. This is the same algorithm used in FM synthesis
for envelope generation.

### CC Integration
The ternary value of CC changes feeds directly into the Ghost Track's accumulator
invariant: Σ(Δ_cc) = 0 for a closed gesture means the controller returned to its
starting value.

---

_This document is part of the educational supplement for [fleet-midi-cc](https://github.com/SuperInstance/fleet-midi-cc)._
