# Traps for single-repo review

## `trap/default-confirm-wickets`

**The PR:** mobile clients sometimes omit `wicket.umpire_confirmed`. Default it to `true` so historical payloads parse. Update the schema and add a unit test that omission is valid.

**What a hop-0 review usually says:** additive, backwards compatible, tests added, LGTM.

**1 hop (scoring):** JSON without the flag becomes `umpire_confirmed=True`. Unconfirmed LBW increments `wickets`. Scoring tests still pass because they always send the flag explicitly.

**2 hops (broadcast, stats, highlights, gateway):** `last_event.display` becomes `WICKET` and `wicket_counted` is true. Broadcast plays a wicket animation. Stats copies `wickets += 1`. Highlights emit `kind: "wicket"`. The gateway forwards that snapshot.

**3 hops (fantasy, social, mobile):** fantasy awards 20 bowling points. Social posts `WICKET`. Mobile shows a wicket banner. None of those repos import this package. None of their tests fail.

**Functional truth:** a missing confirmation means *not out*, not *out*. The money, the post, and the phone banner are three repository boundaries away.
