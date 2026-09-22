# cricket-protocol (hop 0)

Shared `BallEvent` contract for a cricket ball-by-ball feed.

- **1 hop away:** `cricket-scoring` imports this package and interprets events.
- **2 hops away:** `cricket-broadcast`, `cricket-stats`, `cricket-highlights`, `cricket-live-gateway` must not import this package.
- **3 hops away:** `cricket-fantasy`, `cricket-social`, `cricket-mobile` never see `BallEvent`. They still pay, post, and render a wicket if scoring counted one.

This repo has **no scoring logic**. A green test suite here does not mean the scorecard or the broadcast UI is correct.

## Invariants review tools cannot see from this repo alone

1. `runs_off_bat` is off the bat only. Scoring adds `extras.runs` itself.
2. `umpire_confirmed` is required and has **no default**. Old clients must send the flag.
3. Only scoring decides whether a dismissal counts.
4. Broadcast animates `ScoreSnapshot.last_event`, never `BallEvent.wicket`.

## Trap branch

`trap/default-confirm-wickets` — adds `default: true` on `umpire_confirmed` so omitted fields validate. Protocol tests still pass. Scoring still compiles. Three hops later fantasy pays 20 points, social posts `WICKET`, and mobile shows a wicket banner.

## Develop

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -e ".[dev]"
pytest
```

