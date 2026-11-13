# Tech zoom visualisations

A collection of interactive, zoomable explainers. Each one takes a classic "what actually happens when…" question and shows it at increasing depth.

Each topic lives in its own subfolder, with a single-file `index.html` you can open directly in a browser.

**View them live:** https://build-things-that-break-things.github.io/from-enter-to-pixel/

| Topic | Folder | Live | Question | Status |
|---|---|---|---|---|
| Web page load | [`web-page-load/`](web-page-load/) | [open](https://build-things-that-break-things.github.io/from-enter-to-pixel/web-page-load/) | What happens when you type google.com and press Enter? | High level, Interview and Machine internals depths |
| Secure channels | [`secure-channels/`](secure-channels/) | [open](https://build-things-that-break-things.github.io/from-enter-to-pixel/secure-channels/) | How do HTTPS, SFTP, SSH tunnels, VPNs and Tailscale work, and which should I use? | High level, Interview and Deep internals depths |
| Boot sequence | [`boot-sequence/`](boot-sequence/) | [open](https://build-things-that-break-things.github.io/from-enter-to-pixel/boot-sequence/) | What happens between the power button and the login screen, on PCs, servers and VMs, including network boot? | High level, Interview and Deep internals depths |

## Shared conventions

- **One data model, many views.** Each page keeps its content in one list of stages and steps. Every view (map, layer stack, sequence, cheat sheet…) renders from that list, so switching views keeps your place.
- **Depth levels.** Each topic has a plain-language "High level" overview for anyone, an "Interview" depth, and deeper levels (OS, drivers, firmware, hardware). The deeper levels are added as extra fields on the same steps, not as separate pages.
- **Grouped tabs.** The views run left to right from simple to detailed: High level, then an outlined Interview group, then the deeper level.
- **Same controls everywhere.** Arrow keys, Pause animation (space bar) and a looping Auto-advance.
- **Self-contained.** Shared code such as the packet-animation engine is copied into each page rather than linked. No build step. Fonts come from Google Fonts. Everything else is inline.
- **Works anywhere.** Each page is a complete HTML document (doctype, charset, viewport). It runs from GitHub Pages, from a local copy, or as a single published page. The "← All topics" link only appears on GitHub Pages or a local copy.

## Publishing (GitHub Pages)

The root `index.html` is the landing page that links the topics. `.nojekyll` tells GitHub Pages to serve the files as they are, without running Jekyll.

To turn it on: in the repository, open **Settings → Pages → Build and deployment → Source: Deploy from a branch**, choose **`main`** and **`/ (root)`**, then **Save**. The site appears at the URL above within a minute or two, and updates on every push to `main`. On a free account, Pages requires the repository to be public.

To add a topic: create a new folder with its own `index.html` and `README.md`, add a card to the root `index.html`, and add a row to the table above.
