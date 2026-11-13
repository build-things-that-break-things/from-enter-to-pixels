# Web Attacks Compared

Fourteen classic web attacks on a fictional shop, compared side by side: the flaw, how it's found, the exploit, where the code runs, the impact, and detection and fix.

Open `index.html` in a browser, or view it live: https://build-things-that-break-things.github.io/from-enter-to-pixels/web-attacks/

## Which OWASP edition

Category numbers follow the **OWASP Top 10:2025** (published November 2025), with the **2021** numbers shown alongside. The content was checked against owasp.org in **November 2026**. The edition and dates appear in the page header and footer, so a reader finding this years later can see which edition it describes. When a newer edition comes out, update the `o25`/`o21` fields (and the header text), or add a new field for the new edition.

Changes from 2021 to 2025 that show up here:
- SSRF (A10:2021) was merged into A01 Broken Access Control.
- Vulnerable and Outdated Components became A03 Software Supply Chain Failures.
- A10 Mishandling of Exceptional Conditions is new ("Failing open" here).

## The attacks

| Family | Attacks |
|---|---|
| Injection | SQL injection · OS command injection · Cross-site scripting |
| Access & identity | IDOR · Credential stuffing · CSRF |
| Reaching inside | SSRF · Path traversal · XXE |
| Trust & supply chain | Insecure deserialisation · Security misconfiguration · Supply chain attack |
| Data & errors | Cryptographic failures · Failing open |

Colours encode **what the attack hits**: the server and its data, other users' browsers, systems behind the server, accounts and access, or the build and supply chain.

## Views

Add `#overview`, `#side`, `#request`, `#path`, `#fix`, `#compare` or `#deep` to the URL to open a view directly.

- **High level:** "Who gets hurt?" The five targets in plain language, each animated on a map of the shop, with every attack summarised in one line and an everyday comparison. Click an attack to compare it with its neighbours.
- **Interview** group. Every view starts with an **attack picker**: choose any 1–4 attacks, or use the presets in the rail (Injection family, SQLi vs XSS, SSRF vs CSRF, Access, Reaching inside, Trust & supply, Data & errors).
  - **Side by side:** the chosen attacks through the same phase, as animated sequence diagrams, plus a quick contrast in the side panel.
  - **Request anatomy:** the actual request, manifest or stored data behind each attack, with the payload highlighted and explained part by part.
  - **Attack path:** each attack animated on one shared map: attacker, victim, firewall, web app, database, server OS, internal services, package registry and build pipeline.
  - **Vulnerable vs fixed:** short code or configuration snippets, with the cause in red and the fix in green.
  - **Compare:** OWASP 2025 and 2021, CWE, target, root cause, where it runs, impact, detection, fix, and typical MITRE ATT&CK follow-up techniques.
- **Deep internals:** seven topics:
  - **From exploit to breach:** a typical MITRE ATT&CK chain for each selected attack, from initial access to impact, with technique IDs.
  - **Injection, one parser at a time:** how the same input becomes code in SQL, a shell and HTML, and how separating code from data stops it.
  - **The browser's security model:** same-origin policy, SameSite cookies, CORS, CSP and HttpOnly, as a sequence diagram.
  - **How a WAF sees a request:** normalisation, OWASP Core Rule Set matches, anomaly scoring, and why obfuscated payloads get through.
  - **SSRF and the cloud metadata service:** IMDSv1 vs IMDSv2.
  - **Dependency confusion, step by step:** how a public package outranks an internal one.
  - **OWASP Top 10, 2021 → 2025:** both lists side by side with the moves drawn as lines, highlighting the selected attacks' categories.

## Structure of `index.html`

- `TARGETS`: what an attack hits (sets the colour). `PHASES`: the six-phase spine.
- `A`: one entry per attack:
  - `actors` and `ph[phase]`: the side-by-side sequences
  - `req`: request lines; a segment is a plain string, or `[text, "p" (payload) | "k" (context), explanation]`
  - `path`: messages on the `SHOP` map
  - `code`: vulnerable and fixed lines, plus which lines to highlight
  - `cmp`: compare-table values
  - `o25`, `o21`, `cwe`: the OWASP and CWE references
- `PRESETS`: the ready-made comparisons in the rail.
- `CHAINS` (an ATT&CK path per attack: tactic, technique ID, name, note), `TOKENS` (the parser examples), `O21`/`O25`/`MOVE` (the two OWASP lists and how categories moved), and `DEEPT` (the deep topics). The ATT&CK chains are typical examples; technique IDs come from the MITRE ATT&CK Enterprise matrix, which is revised over time like OWASP.
- Shared helpers are copied from the other projects: `Flow`, `seqDiagram`, `drawGraph`, `cycler`.
- **Note:** the page's script contains `<script>` examples. Inside the script they must be written as `<\/script>`, or the browser ends the script early.

## Safety

Educational and defensive: a fictional shop (`shop.example`, `evil.example`), classic textbook payloads, and every attack shown with detection and the fix. The deserialisation payload is deliberately truncated.
