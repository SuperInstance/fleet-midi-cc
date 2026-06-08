# Student Guide: fleet-midi-cc

_Learn how the cc agent works and how to use it._

---

## What You'll Learn

1. What the cc musical concept is
2. How ternary decomposition applies to cc
3. How to query the agent
4. How to interpret the response

## Step 1: Start the Agent

```bash
python3 engine.py --port 2164
```

## Step 2: Query the Agent

```bash
curl -s http://localhost:2164/health | python3 -m json.tool
```

You should see:

```json
{
  "status": "ok",
  "agent": "fleet-midi-cc",
  "port": 2164
}
```

## Step 3: Send Musical Data

```bash
curl -s -X POST http://localhost:2164/agent \
  -H 'Content-Type: application/json' \
  -d '{"input": "test"}' | python3 -m json.tool
```

## Step 4: Interpret the Ternary Vector

The response includes a `ternary_vector` field. Each position represents a
musical dimension:

- `ternary[0]`: The primary classification (approve/reject/observe)
- Remaining positions: Secondary attributes

## Exercise

Experiment with different inputs and observe how the ternary vector changes.
What inputs produce [+1, 0, 0]? What produces [-1, 0, 0]?

---

_This is part of the educational supplement for the [Live Paradigm Fleet](https://github.com/SuperInstance/sailor-workspace)._
