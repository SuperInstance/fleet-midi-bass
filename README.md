# fleet-midi-bass

_Bass line generator — the harmonic and rhythmic anchor._

_One of 16 ternary MIDI agents in the [Live Paradigm Fleet](https://github.com/SuperInstance/sailor-workspace)._

---

## Philosophy — Why Ternary?

The Live Paradigm treats musical gestures as ternary operations. Where binary logic
gives yes/no, ternary gives **approve/reject/observe** — a richer cognitive substrate
that maps naturally to music theory, emotional tension, and conversational flow.

This agent implements **ternary decomposition for bass**.

## Architecture

Position in the fleet pipeline:

```
🎤 Voice → OpenSMILE (25 features) → Ghost Track (T-0..T-4 CR predictions)
  → tminus-dispatcher (cue scheduling) → Fleet Conductor (routing)
  → bass (port 2175)
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
  "agent": "fleet-midi-bass",
  "port": 2175,
  "ternary_vector": [0, 0, 0],
  "ternary_invariant": 0,
  "closed_gesture": false
}
```

## Ternary Logic

| Position | +1 | 0 | -1 |
|----------|------|------|------|
| ternary[0] | walking (stepwise) | root/chord tones | pedal (sustained) |

## Educational Supplement

The bass is the foundation of the harmony and the pulse. It connects the rhythmic
and harmonic domains. A good bass line is felt as much as heard.

### Bass Line Types
- **Pedal (-1)**: One note held or repeated. Drones, tension, stasis.
  Example: "Another One Bites the Dust" by Queen.
- **Root (0)**: Mostly chord roots, changing with each chord. Clear harmonic definition.
  Example: Most pop music bass lines.
- **Walking (+1)**: Stepwise motion between chord tones. Creates forward momentum.
  Example: Jazz walking bass ("So What" by Miles Davis).

### Root Motion
- **Ascending (+1)**: Root going up. Creates anticipation, lift.
- **Repeating (0)**: Same root. Stasis, emphasis.
- **Descending (-1)**: Root going down. Grounding, conclusion.

## Fleet Integration

- **Port**: 2175
- **Roles**: note, velocity
- **Conductor ID**: `bass`
- **Protocol**: HTTP POST to `/2175/agent` with JSON body, 5s timeout
- **Conservation Law**: Σ(Δ_midi) = 4 × Σ(ternary) — closed gestures return to start

## Starting

Local development:

```bash
python3 engine.py --port 2175
```

Or via the fleet start script:

```bash
./scripts/start-fleet-agents.sh
```

## Credits

**Part of the Live Paradigm Fleet** — A ternary cognitive architecture for musical AI.
GitHub: github.com/SuperInstance
Fleet conductor: [sailor-workspace](https://github.com/SuperInstance/sailor-workspace)
