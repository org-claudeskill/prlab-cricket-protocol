# Traps for single-repo review

## `trap/default-confirm-wickets`

**The PR:** mobile clients sometimes omit `wicket.umpire_confirmed`. Default it to `true` so historical payloads parse. Update the schema and add a unit test that omission is valid.

**What a hop-0 review usually says:** additive, backwards compatible, tests added, LGTM.

**1 hop (scoring):** JSON without the flag becomes `umpire_confirmed=True`. Unconfirmed LBW increments `wickets`. Scoring tests still pass because they always send the flag explicitly.

**2 hops (broadcast):** `last_event.display` becomes `WICKET` and `wicket_counted` is true. The UI plays a wicket animation for an appeal that was not given.

**Functional truth:** a missing confirmation means *not out*, not *out*.
