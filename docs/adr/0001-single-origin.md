# ADR 0001 — The web interface and the API are served together, from one origin

> **Version:** 1.0 · 2026-11-04 · approved by the owner
>
> **Answers:** from which address the owner's browser reaches Trueup's web interface and its API;
> how the browser carries her session to the API; and what that requires of every deployment, of
> development on a machine, and of the hosting service.
>
> **Does not answer:** how the owner signs in, and how a session is created, kept on the server
> and ended · which hosting service runs Trueup · the languages and frameworks of the web
> interface and the API. Each of these is its own decision record in `docs/adr/`. The components
> and environments of the system are in `docs/architecture.md`.

---

## 1. Terms

| Term | Meaning |
|---|---|
| **Web interface** | The files the owner's browser loads to show Trueup's screens (`docs/experience.md`) |
| **API** | Every endpoint Trueup serves to the web interface: those that read and change workspace data (`docs/quality-attributes.md`, 1), and those for signing in, signing out and ending a session |
| **Origin** | A scheme, a host and a port, such as `https://trueup.hosting.example:443`. Two addresses with the same three parts have the same origin. The host in this and every other example is invented |
| **Site** | What a browser treats as one party when it decides whether to send a cookie. A host that a hosting service assigns under its own domain is a site of its own when that domain is a **public suffix**: a domain under which unrelated parties hold hosts, such as `hosting.example` in the example above |
| **Third-party cookie** | A cookie that the browser would send to one site on a call made from a page of another site |
| **Deployed environment** | Production, or another environment that runs on a hosting service, such as a demo (`docs/quality-attributes.md`, 1). An environment on a machine, such as the owner's development environment, is a **local environment** |

## 2. Context

- The owner uses Trueup from a browser, on her phone and on her computer
  (`docs/quality-attributes.md`, PHN-1 to PHN-3).
- Only the owner reaches workspace data, and she can end the session of a device from another
  device (`docs/quality-attributes.md`, SEC-1 and SEC-3).
- Safari blocks third-party cookies by default. A cookie that must reach the API on every call
  therefore belongs to the same site as the page that makes the call.
- Trueup has no domain name of its own: a domain name has a recurring cost, and
  `docs/quality-attributes.md`, CST-1, allows none while free tiers meet every target. A deployed
  environment's address is a host that its hosting service assigns under the service's domain.
  When that domain is a public suffix, as it is for the service cited in section 5, two hosts
  under it are two sites. The web interface and the API are certain to share a site only when
  they share a host.

## 3. Decision

The web interface and the API are served together, from **one origin**.

- **One process serves both.** The process that answers the API also serves the built files of
  the web interface.
- **One path prefix belongs to the API.** The API answers every path under `/api/`. Every other
  path belongs to the web interface, and the web interface never uses a path under `/api/` for a
  screen.
- **The web interface calls the API by relative path,** such as `/api/transactions`, never by a
  full address. The same build of the web interface works at any origin.
- **The browser carries the owner's session in a cookie of this origin.** The cookie is set by
  the API for its own host only, without naming a parent domain, and cannot be read by the web
  interface's scripts. The decision record on signing in and sessions decides everything else
  about the session.
- **Every deployed environment uses `https`** and has an origin of its own with this layout:
  production, a demo if one exists (`docs/architecture.md`), and every other one. Its session
  cookie is sent only over `https`.
- **A local environment follows the same layout at one local origin,** `http://localhost` with a
  port of its own. The owner's browser, and every automated test that runs in a browser, reach the
  web interface and the API there. When the web interface is served by a development server, that
  server forwards every path under `/api/` to the local API.

## 4. Consequences

- **Every cookie Trueup sets is first-party.** No call from the web interface to the API depends on
  a third-party cookie.
- **The API grants no access to other origins.** Its responses carry no permission for pages of
  another origin to read them. It refuses, as in SEC-1, every call that changes data and whose
  browser-reported origin is not its own.
- **No cookie of production reaches another environment.** Cookies belong to a host, and no other
  environment uses production's host (`docs/quality-attributes.md`, SEC-7).
- **The hosting service runs that process over `https`** (`docs/quality-attributes.md`, SEC-4). A service that only serves static files cannot host
  Trueup.
- **The web interface and the API are built, tested and released together, as one version.** A
  web interface loaded before a release, such as one left open on the phone, can be older than
  the API it calls. When an action fails for that reason, what the owner entered is kept, as for
  an interrupted action (`docs/experience.md`, 6.4).
- **A pause of the hosting service delays the whole interface.** If the service suspends the
  process while Trueup is idle, the first visit after a pause waits for it to start before any
  screen appears, not only before data appears. This wait counts toward
  `docs/quality-attributes.md`, PHN-1.
- **Changing the host signs the owner out on every device.** Cookies belong to the host, so a new
  host, whether from another hosting service or from a domain name of her own, starts with no
  session.

## 5. Evidence

This decision rests on how browsers treat cookies, not on a service Trueup uses. The end-to-end
test of the hosting service is recorded in the decision record on the hosting service.

| Fact this decision relies on | Source |
|---|---|
| Safari blocks all third-party cookies by default, from Safari 13.1 on iOS, iPadOS and macOS | WebKit, "Full Third-Party Cookie Blocking and More": https://webkit.org/blog/10218/full-third-party-cookie-blocking-and-more/ |
| A host under a public suffix is a site of its own | Public Suffix List, "Learn more": https://publicsuffix.org/learn/ |
| A hosting service's own domain is a public suffix, for one service | Vercel, "Can I set a cookie from my Vercel project subdomain to vercel.app?": https://vercel.com/kb/guide/can-i-set-a-cookie-from-my-vercel-project-subdomain-to-vercel-app |
| A cookie set without naming a domain is sent only to the host that set it | RFC 6265, section 5.3, step 6: https://www.rfc-editor.org/rfc/rfc6265#section-5.3 |

## 6. Changing this document

Once approved, this record changes only in its header line, which then names the record in
`docs/adr/` that supersedes it.

## History

| Date | Version | Change |
|---|---|---|
| 2026-11-04 | 1.0 | First approved version. Serves the web interface and the API together from one origin, with the session carried in a cookie of that origin |
