# .github

**This repository exists for one reason: to be the landing page.**

GitHub shows `profile/README.md` on the [PrivateKey organisation overview][org] when a public
repository is named `.github` and owned by the organisation. That is the whole mechanism — no
website, no build, no deploy. Commit, and it is live.

```
profile/
  README.md            ← the organisation page
  assets/privatekey.png ← the banner it references
```

## Editing the page

Edit `profile/README.md`. GitHub renders it with its usual Markdown + sanitised HTML subset.

Two things that are easy to get wrong and are therefore written down:

- **A `<p>` cannot contain a `<p>`.** The browser closes the outer one and the centring is lost.
  For a centred two-line block use one `<p>` with `<br>`.
- **Markdown is not parsed inside a block-level HTML tag.** `**bold**` inside `<p>` renders as
  literal asterisks. Use `<strong>`; use `<code>` where you would have used backticks.

## Why every package points at this repo

Each package README carries the same header, and loads the banner from here by absolute URL:
`raw.githubusercontent.com/getprivatekey/.github/main/profile/assets/privatekey.png`.

Absolute, because npm renders a published README on `npmjs.com` where a relative path has
nothing to resolve against. One copy of the image, one place to change it.

[org]: https://github.com/getprivatekey
