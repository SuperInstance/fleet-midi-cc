# fleet-midi-cc

_Control Change processor — smooth those CC messages!_

_One of 16 ternary MIDI agents in the [Live Paradigm Fleet](https://github.com/SuperInstance/sailor-workspace)._

---

## Philosophy — Why Ternary?

The Live Paradigm treats musical gestures as ternary operations. Where binary logic
gives yes/no, ternary gives **approve/reject/observe** — a richer cognitive substrate
that maps naturally to music theory, emotional tension, and conversational flow.

This agent implements **ternary decomposition for cc**.

## Architecture

Position in the fleet pipeline:

```
🎤 Voice → OpenSMILE (25 features) → Ghost Track (T-0..T-4 CR predictions)
  → tminus-dispatcher (cue scheduling) → Fleet Conductor (routing)
  → cc (port 2164)
```

## API Reference

| Method | Path | Description |
|--------|------|-------------|
| GET | /health | Health check + agent identity |
| POST | /agent with `{"type":"probe"}` | Liveness probe for fleet-conductor |
| POST | /agent | Process musical data, return ternary analysis |
| POST | / | Direct query with JSON body |

### Response Format

```json
{
  "status": "ok",
  "agent": "fleet-midi-cc",
  "port": 2164,
  "ternary_vector": [0, 0, 0],
  "ternary_invariant": 0,
  "closed_gesture": false
}
```

## Ternary Logic

| Position | +1 | 0 | -1 |
|----------|------|------|------|
| ternary[0] | value increasing | stable | value decreasing |

## Educational Supplement

MIDI Control Change (CC) messages are the primary way to modify sound parameters
in real time. Volume (CC#7), pan (CC#10), modulation (CC#1), expression (CC#11),
reverb (CC#91), and many more.

This agent smooths CC data and classifies the direction of change. Raw CC values
can be noisy — this agent applies EMA low-pass filtering (like the SmoothingFilter
in fleet-midi-cc/lib/engine.py) before classification.

### CC Ranges
- 0-63: Low range (negative / "dark" / "soft")
- 64: Center (neutral)
- 65-127: High range (positive / "bright" / "loud")

## Fleet Integration

- **Port**: 2164
- **Roles**: cc
- **Conductor ID**: `cc`
- **Protocol**: HTTP POST to `/2164/agent` with JSON body, 5s timeout
- **Conservation Law**: Σ(Δ_midi) = 4 × Σ(ternary) — closed gestures return to start

## Starting

Local development:

```bash
python3 engine.py --port 2164
```

Or via the fleet start script:

```bash
./scripts/start-fleet-agents.sh
```

## Credits

**Part of the Live Paradigm Fleet** — A ternary cognitive architecture for musical AI.
GitHub: github.com/SuperInstance
Fleet conductor: [sailor-workspace](https://github.com/SuperInstance/sailor-workspace)
