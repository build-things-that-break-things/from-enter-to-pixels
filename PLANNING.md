# Planning

Ideas, priorities and open items for the collection. This file is public along with the rest of the repository.

_Last updated: November 2026._

## Current topics

| Topic | Depths | Notes |
|---|---|---|
| [Web page load](web-page-load/) | High level · Interview · Machine internals | |
| [Secure channels](secure-channels/) | High level · Interview · Deep internals | |
| [Boot sequence](boot-sequence/) | High level · Interview · Deep internals | |
| [Web attacks](web-attacks/) | High level · Interview · Deep internals | Based on OWASP Top 10:2025 |

## Next up

### 1. Authentication flows compared (next topic)

How a site knows who you are, and how each method can fail. It reuses the **attack picker** from Web attacks, so visitors choose 2–4 flows and compare them side by side.

- **Flows:** passwords + server sessions · JWT bearer tokens · OAuth 2.0 (authorisation code + PKCE) · OpenID Connect · SAML · passkeys / WebAuthn · MFA (TOTP, push, SMS) · API keys
- **Shared phases** (a draft, like the six phases in Secure channels):
  1. Enrol
  2. Prove who you are
  3. Get a credential (cookie, token, assertion)
  4. Use it on each request
  5. Refresh and expire
  6. Log out or revoke
  7. Where it breaks
- **Interview views:** side by side · token/cookie anatomy (byte layout of a JWT, a session cookie, a SAML assertion) · redirect map (who sends the browser where: app ↔ identity provider ↔ API) · compare table
- **High level:** keys, wristbands and ID cards as everyday comparisons; a "which one should I use?" chooser
- **Deep internals ideas:** WebAuthn challenge signing, JWT signature checks (and the `alg: none` / key-confusion classics), PKCE, refresh-token rotation, SAML XML signatures, session fixation, token storage in the browser
- **Links to existing pages:** credential stuffing, CSRF and XSS in Web attacks; trust models in Secure channels
- **Date stamp:** name the standards and versions referenced (OAuth 2.0 / 2.1 draft status, WebAuthn Level, NIST SP 800-63 revision) and when they were checked, the same way Web attacks dates its OWASP edition

## Topic backlog

Roughly in order of interest. Same formula each time: one "what actually happens when…" question, a plain-language overview, an interview-level comparison, then the internals.

| Topic | Question | Notes |
|---|---|---|
| `git push` to CI | What happens between `git push` and a green build? | Objects, packfiles, refs, SSH/HTTPS transfer, hooks, webhooks, CI runners |
| Containers | What does `docker run` actually do? | Image layers, overlayfs, namespaces, cgroups, runc. Compare VMs and microVMs (links to Boot sequence) |
| Email | How does an email get from "Send" to the inbox? | SMTP, MX, SPF/DKIM/DMARC, spam scoring, phishing |
| Database query | How does a database run a query? | Parser, planner, B-tree indexes, buffer pool, WAL, MVCC, replication |
| Kubernetes | From `kubectl apply` to a running pod | API server, etcd, scheduler, kubelet, CNI, services |
| Card payment | From tap to approval | Terminal, acquirer, card network, issuer, EMV cryptograms, 3-D Secure, fraud scoring |
| Video call | How does a video call find its way? | STUN/TURN/ICE, SFUs, codecs, jitter buffers (reuses NAT traversal from Secure channels) |
| ATT&CK explorer | How do real intrusions unfold? | The full MITRE ATT&CK matrix, with real incidents replayed across it |

## Visualisation upgrades

### Within the single-file format
- **Real zoom.** Click a box ("Web app", "Kernel") and the view zooms *into* it to reveal the diagram inside, with a breadcrumb back out. It makes the collection's "zoom" idea literal.
- **Guided story mode.** A step- or scroll-driven tour that moves the camera, highlights and explains; a gentler entry point than the tabs.
- **More hands-on sandboxes,** like the boot order simulator: build a TLS handshake, tokenise your own injection attempt, configure a firewall rule and see what passes.
- **Interview practice mode.** Questions per stage with hints, plus the "say it in the interview" answer to compare against.
- **Live data where safe.** For example a real DNS lookup through a public DNS-over-HTTPS service. If added, say on the page which provider receives the lookups.

### Bigger changes (would give up the single-file format)
- **Shared engine folder.** `shared/engine.js` and `shared/style.css` (the `Flow` animation engine, `seqDiagram`, `drawGraph`, the rail and controls) instead of a copy in every page. Still static files with no build step. Best done once there are five or more topics, and before adding real zoom or story mode everywhere.
- **Real Linux boot in the browser** (v86, an x86 emulator in WebAssembly) for Boot sequence: genuine kernel and systemd messages synced to the diagram. Downloads several MB.
- **3D, sparingly,** for physical things only (a motherboard for power-on, a data centre). Labels are harder to read in 3D, so not for protocol diagrams.

### Deferred: SQL injection playground
A real database in the browser (sql.js) behind a deliberately vulnerable login form. **On hold.** Good free hands-on labs already exist, so for now Web attacks should link to them rather than rebuild one. If the links prove not to be enough, revisit this.

Candidate links for a "Practise safely" section in Web attacks:
- PortSwigger Web Security Academy: free, browser-based labs for every attack covered
- OWASP Juice Shop: a deliberately insecure shop app to run locally
- OWASP WebGoat: guided lessons with a vulnerable app
- DVWA (Damn Vulnerable Web Application): classic local practice target

Each link should be checked before adding, and the page should keep its note about only testing systems you own or are authorised to test.

## Housekeeping

- **Phone layout:** not yet checked properly on any page. The diagrams scroll sideways on narrow screens; the rail and notes should be reviewed at about 400 px wide.
- **Visit counting:** not set up. If wanted, GoatCounter (free, no cookies) is the preferred option. Create the account with the project identity, then add one script line per page and change the landing-page footer text that currently says "no tracking".
- **Edition dates:** Web attacks names OWASP Top 10:2025 and its check date. Revisit when OWASP publishes a new edition, and apply the same habit to any topic that depends on a versioned standard.

## Checklist for a new topic

1. A new folder with `index.html` (complete document head, favicon links pointing to `../`, the "← All topics" back link) and a `README.md`.
2. The same layout and controls: High level · outlined Interview group · deeper level; arrow keys, Pause animation, looping Auto-advance.
3. A card on the root `index.html` and a row in the root `README.md` table.
4. Any `</script>` inside the page's script written as `<\/script>`.
5. A versioned standard (OWASP, ATT&CK, an RFC revision) named with its edition and check date on the page.
6. Before committing: search for personal or work-account details (names, emails, local paths, private links), and check that the commit author is the project's noreply identity.
