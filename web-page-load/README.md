# From Enter to Pixels

What happens between typing `google.com`, pressing Enter and seeing the page: 6 stages, 26 steps.

Open `index.html` in a browser, or view it live: https://build-things-that-break-things.github.io/from-enter-to-pixel/web-page-load/

## Views

The page has three depths, shown as the row of view tabs from left to right: **High level**, then the outlined **Interview** group (Network map, Layer stack, Sequence, Cheat sheet), then **Machine internals**. All views read the same `STEPS` data, and switching keeps your place. Add `#overview`, `#map`, `#layers`, `#sequence`, `#cheatsheet` or `#internals` to the URL to open a view directly. The page opens on `#overview` by default.

- **High level** (depth 0, the default): six plain-language stages for non-technical readers. Each stage has an animated journey line, an everyday comparison and one thing to remember. The arrows move a whole stage at a time.

- **Network map:** a diagram of every machine involved, with animated packets. Steps that happen only inside the browser zoom into the browser.
- **Layer stack:** encapsulation per step, showing what each layer adds and where it runs on the laptop.
- **Sequence:** every message in time order, with a running clock.
- **Cheat sheet:** a revision sheet for the current stage (1 to 6) only. The stage bar moves between stages.
- **Machine internals** (depth 2): the same step inside the laptop. It shows the browser processes, the kernel (sockets, IP, scheduler, drivers) and the hardware (CPU, RAM, Wi-Fi chip, GPU, display). For steps that happen elsewhere, it shows what the laptop does while it waits.

## Structure of `index.html`

- `STAGES`, `STEPS`: the content. Each step has the text fields (`title`, `sum`, `body`, `say`, `probes`, `facts`) and the timing estimate `t`. It also has the visual data:
  - `m`: messages on the network graph
  - `b`: messages in the browser-internals graph
  - `wire`: the value at each layer, for the Layer stack view
  - `note`, `focus`, `local`: extra highlights and remarks
- `PLAIN`: the depth-0 content, keyed by stage id and attached as `stage.plain`. Each entry has `title`, `line`, `text`, `analogy` and `takeaway`, plus `label`, `sub` and `doing` for the journey diagram. `HighView` draws the journey line using the small icons in `ICONS`.
- `DEEP`: the depth-2 content, keyed by step id and attached as `step.deep`:
  - `flow`: messages on the machine graph
  - `focus`: extra nodes to highlight
  - `note`: shown when nothing moves locally
  - `text`, `facts`: the notes panel content
- `GRAPHS`: node positions and links for three diagrams: `net` (network map), `browser` (browser internals) and `machine` (user space / kernel / hardware).
- `drawGraph()`: the shared renderer for the Network map and Machine internals views.
- `MapView`, `LayerView`, `SeqView`, `CheatView`, `InternalsView`: one object per view, each with `render(step, host)` and `stop()`. To add a view, write another object and register it in `VIEWS`.

## Adding a deeper level

Follow the `DEEP` pattern:
1. Add a new keyed object, e.g. `DEEPER` for firmware, DMA and interrupt detail.
2. If it needs a new diagram, add it to `GRAPHS`.
3. Add a view object that calls `drawGraph()`, and add its tab to `buildTabs()` (outside or inside a depth group).

## Controls

- **← / →** (or the arrow keys): previous / next step. In High level, they move one stage at a time.
- **Pause animation / Play animation** (or the space bar): freezes and resumes every animation, including packet dots, the layer-stack sequence and blinking or flowing effects. The setting is remembered in the browser.
- **Auto-advance:** optional tour mode that moves to the next step every 12 s (16 s in Layer stack). A bar in the button shows the time left. After stage 6 it starts again from stage 1, looping until you click it again.
