<p align="center">
  <img src="assets/privatekey.png" alt="PrivateKey" width="540">
</p>

<h1 align="center">PrivateKey</h1>

<p align="center">
  <strong>Own the key. Set the rules.</strong><br>
  Storage is a commodity. Access is the product. Security is the architecture.
</p>

<p align="center">
  Self-sovereign by design. <strong>Your key, your storage rules, your access rules.</strong> Security is not a solution, it's a decision —
  so neither a store nor a framework decides them for you. We ship the blocks: a signer behind
  a boundary, storage you choose, policy in front of the key, and a refusal you can point at.
  PrivateKey is an architecture — you are the architect.
</p>

<p align="center">
  <a href="https://github.com/getprivatekey">GitHub</a> •
  <a href="https://www.npmjs.com/org/privatekey">npm</a>
</p>

---

## The problem

Every key-management framework answers one question — *where does the key live?* — and then
spends its architecture hiding the answer: adapter over adapter, so the key is never touched
directly and nobody has to think about it again.

That is the wrong question, and the hiding is theatre. **The key is not hidden. The access to it is governed.**

## Security is the architecture, not the default

True security does not start by hiding the key from plain sight, or by handling it behind a
curtain of storage interfaces and `Omit` types. It starts with an honest, transparent
conversation about how the key travels through the layers.

A wormhole of code — *your* private key buried inside the storage, then handed to the signer
through an obscure chain — is solving the wrong problem. The problem is not the storage; it is
the policies and the boundaries between the layers. Security is not in how well the key is
delegated, but in how transparently each layer can say what it knows and how it protects the
material. 

## The solution

PrivateKey is not a framework. It is a framing of a question — *What is your security model?* — it's a design process, not a blueprint and not a black box. We do not ship security
as a fact or a product; we offer a process of interrogating each tier, of building awareness and compounded security architecture. Security is
the result of a conscious choice, not a package, not a pre-processed result. 

We do not hand you answers, and we do not sell you security as a promise. Only your own surface
and attack vectors can answer that. What we give you is a toolbox that makes security
composable, to fit your application's architecture and the grade of security it needs. Simple,
robust, ultra-stable tools that behave predictably and produce reliable output. Security then
becomes transparent — built on a solid foundation you understand, rather than on a leap of
faith.

Your key, your rules, your responsibility. That is the PrivateKey principle: your policies lead
the design, not tools that protect you from yourself by limiting your choices.

## Embedded security, tiered security

There are two answers to *is it secure?*, and they are not variations of each other.

One points **inside the box**. The framework owns storage, signing, policy and transport, so
everything in it is trusted because it is in it. That is the agent model — Veramo being its
best-known implementation — and it is a reasonable answer until you ask where the boundary is.
The boundary is the process. Every plugin widens what must be trusted, and the security review
becomes the framework, plus its plugin graph, plus every adapter it loads.

The other points **at the seams**. Each tier states what it knows — the filesystem knows a
location, the signer knows a key, the policy knows who may use it — and the contract between
them is the entire review surface. Two short interfaces, and a line you can point at.

| | embedded | tiered |
|---|---|---|
| trust boundary | the process | the port |
| review surface | the framework, its plugins, its adapters | two interfaces |
| adding a capability | widens what must be trusted | narrows what each tier may do |
| the strongest option | a plugin slot, if the framework has one | the same one-method port as the weakest |
| blast radius of one bad dependency | everything | the tier it lives in |

Tiering is not better by default, and the difference is not stylistic. A port with forty methods
is a framework wearing a port's clothes — the seam has to be narrow enough to read in one
sitting, or the architecture buys nothing. Tiers also cost the integration work a framework
does for you.

What tiering buys is the one thing that cannot be retrofitted: **a place to say no.** A
framework that handles it for you has no point at which refusal is a first-class outcome. A
boundary does — and the refusal is the record.

> **A framework's security is a claim about its inside. An architecture's is a claim about its seams.**

## What we've built

- **Storage is an interface you choose.** Two keystores ship, and they are siblings, not
  layers. `@privatekey/keystore-node` keeps each key in a file on your machine;
  `@privatekey/keystore-hsfs` keeps the same file behind any `IFileSystem` you already run — a
  memory filesystem, a container volume, an encrypting layer, or a design of your own. Neither
  depends on the other. Want protection at rest? Wrap the filesystem. That is a decision you
  make, in a place you can see.
- **Access is the boundary you run.** One operation — sign — with a policy in front of it:
  who may use the key, for what, how often, and what happens when the answer is no.  
- **Security is an architecture decision, and it stays yours.** A framework picks your threat
  model and calls the choice a feature. We hand over the blocks and leave the decision where it
  can be reviewed. You protect the signer, the signer protects the key, the filesystem protects
  the location. Nothing is hidden, everything is composable.

> **The boundary is the product. The location is a choice.**

If a guarantee is not visible in the design, it is not a guarantee — it is a hope with a
dependency graph.

## The architecture, assembled

```
        your application
               │  "sign this"
               ▼
          ISigner            one method. The domain's entire view of the key.
               ▲
               │  returns a signature
          IKeyStore          six methods. Where the key lives.
               ▲
               │  implemented by
  keystore-node · keystore-hsfs · keychain · HSM · KMS · your own
```

Read it top down and it is the whole design. Your application asks for a signature and says
nothing about where the key is. The signer answers without ever handing the key over. Only the
keystore knows the location — and location is the one part that genuinely differs between
machines, clouds and customers.

Nothing above a port imports anything below it, so every layer can be replaced on its own. That
is what makes this an architecture rather than a framework: the security review reads two short
interfaces instead of trusting a stack, and the answer to *where is the key?* is something you
can point at.

## What that buys you

The same blocks assemble into different operating models. None of them is a separate product;
each is a choice, made in the open and reviewable afterwards.

| what you need | what you assemble | where the key is |
|---|---|---|
| a key on one machine, signing in one process | `@privatekey/keys` + `@privatekey/keystore-node` | a file, `0600`, in a directory only its owner can list |
| keys kept where your infrastructure already keeps secrets | `@privatekey/keys` + `@privatekey/keystore-hsfs` over your filesystem or keychain | in the store your security review already approved |
| an application that signs without ever holding the key | a keystore behind a long-running process; the application holds only an `ISigner` | in that process, and nowhere the application can reach |
| proof that access was governed | a policy in front of the signer, refusing with a reason you can log | nowhere new — the refusal *is* the record |

Four rows, one architecture. What stays constant is the part that matters: **the key does not
travel**, and **the decision is written where it can be read** — never buried in a default
somebody else picked.

> **Where we are.** `@privatekey/keys` and both keystores exist and are tested. The policy layer
> over the signer, `@privatekey/boundary`, is the next block — and the boundary is a `sign` call
> and a refusal, not a product you have to adopt wholesale.

## The packages

| package | what it is |
|---|---|
| [`@privatekey/keys`](https://github.com/getprivatekey/packages/tree/main/keys) | key material — generate, derive, convert, verify. Five key types, **zero dependencies** |
| [`@privatekey/core`](https://github.com/getprivatekey/packages/tree/main/core) | the ports — `IKeyStore` and `ISigner`. Types only: zero runtime, zero dependencies |
| [`@privatekey/keystore-node`](https://github.com/getprivatekey/packages/tree/main/keystore-node) | a keystore on `node:fs` — one file per key, `0600`, a `0700` directory when the store makes it |
| [`@privatekey/keystore-hsfs`](https://github.com/getprivatekey/packages/tree/main/keystore-hsfs) | a keystore over any `IFileSystem` — the same file, behind [High Security File System](https://github.com/high-security) |
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
- **Independent means independent in the dependency graph.** The `-node` keystore declares no
  filesystem abstraction, and a test fails the build if one ever appears. "Lightweight" is a
  claim; a dependency list is evidence.
- **No dependencies in the core.** `@privatekey/keys` uses `node:crypto` and nothing else.
- **A library that prints into your stderr is a library you cannot use in a request path.**
  Functions return `null` and let the caller decide.
- **Fail closed.** If a thing cannot be judged, it is refused — never passed on the assumption
  that empty means fine.

## Licence

MIT — see each package.
