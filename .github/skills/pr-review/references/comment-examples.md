# Review Comment Examples

Before/after pairs showing the principles applied.

## Tone and phrasing

**Commanding (avoid):**
> Don't use a raw SQL query here. Use the ORM.

**Collaborative (prefer):**
> Could we use the ORM here instead of raw SQL? We get automatic escaping and it stays consistent with the rest of the data layer — easier for the next person to follow.

---

**Judgmental (avoid):**
> This is wrong. You're not handling the null case.

**Observational + reasoning (prefer):**
> I think this might panic if `user` is nil — `GetName()` would dereference a nil pointer. What do you think about adding a guard before this block?

---

**Vague (avoid):**
> This seems complicated.

**Specific (prefer):**
> `processPayment` is doing three distinct things (validating, charging, and updating the record). Would it make sense to split these out? When something goes wrong it's hard to tell at which step without debugging.

---

## Ownership language

**Finger-pointing (avoid):**
> Your error handling doesn't cover the timeout case.

**Shared ownership (prefer):**
> I don't think we're handling the timeout case yet — if the downstream call times out, we'd return a 500 with no context. Should we add a specific branch for that?

---

## Calibrating threshold

**Blocking (bugs, security, breaks contract):**
> `user_id` is being interpolated directly into the SQL string on line 34 — this is injectable. We should use a parameterized query: `WHERE id = $1` with `args=[user_id]`.

**Non-blocking follow-up (refactor, style, preference):**
> Not a blocker, but I noticed `validate_address` is now called in four places with slightly different defaults. Might be worth a follow-up to centralize that — happy to file a ticket if useful.

**Positive (say it):**
> Love the `limit + 1` trick to detect the next page without a COUNT query — clean.

---

## Seeking perspective before concluding

**Asserting (avoid when uncertain):**
> This will be slow at scale.

**Asking first (prefer):**
> I'm curious about performance here — if the `orders` table grows to millions of rows, `WHERE status = 'pending'` without an index might get slow. Is there an index on `status`, or is volume expected to stay manageable?

---

## Scope-creep as follow-up

**Blocking on out-of-scope work (avoid):**
> This whole module needs to be refactored before we merge this.

**Proposing as a follow-up (prefer):**
> The query-building pattern here has gotten scattered across a few places — not a blocker for this PR, but worth a dedicated cleanup pass. Want me to file a ticket?
