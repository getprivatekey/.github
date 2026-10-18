<p align="center">
  <img src="assets/privatekey.png" alt="PrivateKey" width="320">
</p>

<h1 align="center">PrivateKey</h1>

<p align="center">
  <strong>Own the key. Set the rules.</strong><br>
  Storage is a commodity. Access is the product. Security is the design.
</p>

<p align="center">
  Self-sovereign by design. Your key never leaves your machine — and rather than hiding it
  behind another store, a boundary decides who may use it, refuses what it may not, and
  records both.
</p>

<p align="center">
  <a href="https://github.com/getprivatekey">GitHub</a> •
  <a href="https://www.npmjs.com/org/privatekey">npm</a>
</p>

---

## Security is the design, not the default

Every key-management framework answers one question — *where does the key live?* — and then
spends its architecture hiding the answer: adapter over adapter, so the key is never touched
directly and nobody has to think about it again.

That is the wrong question, and the hiding is theatre. **The key is not hidden. The access to
it is governed.**

- **Storage is an interface you choose.** `@privatekey/keystore-file` is an adapter over
  `@hsfs` — somebody else's stable `IFileSystem`. We did not write a store; a store is a
  commodity, and inventing one would only make it our problem. Want protection at rest? Wrap
  the filesystem. That is a decision you make, in a place you can see.
- **Access is the boundary you run.** One operation — sign — with a policy in front of it:
  who may use the key, for what, how often, and what happens when the answer is no.
- **Security is a design decision, and it stays yours.** A framework picks your threat model
  and calls the choice a feature. We hand over the primitives and leave the decision where it
  can be reviewed.

> **The boundary is the product. The location is a choice.**

If a guarantee is not visible in the design, it is not a guarantee — it is a hope with a
dependency graph.

## The packages

| package | what it is |
|---|---|
| [`@privatekey/keys`](https://github.com/getprivatekey/keys) | key material — generate, derive, convert, verify. Five key types, **zero dependencies** |
| [`@privatekey/keystore-file`](https://github.com/getprivatekey/keystore-file) | a keystore over any `IFileSystem` — one file per key, the file **is** the key |
| `@privatekey/core` | the ports. Types only: zero runtime, zero dependencies |
| `privatekey` | the name, and the front door. The product is `@privatekey/*` |

## What this is not

- **Not a custodial service.** We operate nothing. You run the boundary on your own machine or
  your own server, and the key never leaves it. Anyone who hosts it for you is a *host*, not a
  custodian — and if a managed offering is ever built, it will be a different product with a
  different name, never this one wearing a different adjective.
- **Not a wallet.** A wallet holds keys for you. This decides who may use a key you already
  hold.
- **No bespoke crypto.** Drivers use vendor SDKs. We do not invent primitives, and we do not
  depend on our inventions.

## How we work

- **Ports are owned by their lowest consumer**, and nothing depends on an implementation of
  one. An adapter a consumer is forced to install has stopped being an adapter.
- **No dependencies in the core.** `@privatekey/keys` uses `node:crypto` and nothing else.
- **A library that prints into your stderr is a library you cannot use in a request path.**
  Functions return `null` and let the caller decide.
- **Fail closed.** If a thing cannot be judged, it is refused — never passed on the assumption
  that empty means fine.

## Licence

MIT — see each package.
