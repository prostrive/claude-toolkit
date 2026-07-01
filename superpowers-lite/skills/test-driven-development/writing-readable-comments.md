# Writing Readable Comments

Comments are for humans. A comment earns its place by saying something the code
cannot: **why** this exists, a constraint that isn't visible locally, a warning,
or a pointer to context. If a comment just narrates what the next few lines
plainly do, delete it.

The failure mode to guard against is not too few comments — it's comments that
are technically accurate but unreadable: long, dense, packed with backticked
identifiers, arrows, and cross-references, written as compressed notes-to-self
rather than sentences a person can skim.

## The smell

<Bad>
```typescript
/**
 * Gateway → accounts OWT create command. Publishes Nest
 * `.send(OWT_CREATE_REQUESTED, req)` to `ACCOUNTS_OWT_QUEUE`
 * (`accounts.owt.requests`); the listener is accounts'
 * `OwtCommandConsumer` (`@MessagePattern(OWT_CREATE_REQUESTED)`). The
 * reply (`{ id, status, correlationId }`) comes back inline over Nest's
 * transport-managed reply queue — no app-level reply queue is declared.
 * Queue + pattern binding lives in `OwtDispatcherModule.forGateway()`.
 * Inject this from controllers; it's both the named DI seam and the only
 * impl. `.send()` + timeout + correlation logging live once in
 * `RmqRequestReplyDispatcher`.
 */
```
Eleven lines for one method. Every identifier (`OWT_CREATE_REQUESTED`,
`OwtCommandConsumer`, `RmqRequestReplyDispatcher`…) is already on screen or one
click away. It documents the entire message-bus topology instead of what a
caller needs. It reads like the author's scratch notes, not documentation.
</Bad>

What is actually worth saying here? The one non-obvious thing: **this looks like
a method call but it is a blocking network round-trip.** Everything else is
either visible in the code or belongs in module/architecture docs, not on this
method.

<Good>
```typescript
/**
 * Asks the accounts service to create an OWT and waits for the result.
 * This is a request/reply over the message bus, not a local call — it can
 * be slow and can time out. Inject it into controllers instead of talking
 * to the transport directly.
 */
```
Plain sentences. Says the surprising part (network round-trip, can time out) and
the one usage rule (inject it). Names nothing the reader can already see.
</Good>

Often the honest version is even shorter:

<Good>
```typescript
/** Creates an OWT via the accounts service. Blocking request/reply — can time out. */
```
</Good>

## Rules

1. **Comment the why, not the what.** The code already shows what it does. Spend
   the comment on the reason, the constraint, the gotcha, the "don't do X
   because Y."
2. **Write sentences a person skims.** Prose, not symbol soup. If a comment is
   mostly backticks, arrows, and stacked parentheticals, rewrite it as English.
3. **Don't restate identifiers that are on screen.** Naming `FooConsumer` /
   `BAR_QUEUE` / `bazModule` in prose adds nothing the reader can't already see
   or click through to.
4. **Keep it proportional.** A comment longer than the code it explains is
   usually explaining the wrong thing. A method comment describes the contract a
   caller needs; system topology belongs in module or architecture docs.
5. **Prefer a better name over a comment.** If you're about to explain what a
   variable or function is, first try renaming it so the explanation becomes
   unnecessary.
6. **Don't write comments that will lie.** A comment that mirrors the current
   implementation goes stale the moment the code changes. Comment the intent,
   which is stable.

## Quick check before you leave a comment

- Would a competent reader already know this from the code and the names? →
  delete it.
- Is it a wall of identifiers and arrows? → rewrite as a sentence, or delete.
- Is it longer than the code it sits above? → cut it down to the one non-obvious
  thing.
- Does it explain *why*, or a caveat a caller can't infer? → keep it; that's the
  case comments are for.
