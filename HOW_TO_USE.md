# How to use Anatomy

> Describe a principle in one sentence and Claude builds it as an **isometric machine that moves, comes apart and responds to input**, delivered as a single HTML page.
> Core idea: **the object is the explanation**.

This is a practical guide written for this fork. The skill itself is [wheresryan22/anatomy](https://github.com/wheresryan22/anatomy) by Ryan (MIT); see the main [README](README.md) for the full reference.

---

## 1. Install

| Item | Requirement |
|---|---|
| Code | `git clone https://github.com/xdis/isometric-anatomy-skill.git anatomy` |
| Claude Code skill | Symlink it so it works from any folder: `ln -s "$PWD/anatomy" ~/.claude/skills/anatomy` |
| Node.js | ≥ 20.10 |
| Google Chrome | Used by the self-check screenshots |
| Dependencies | None, no `npm install` needed |

**Note:** open a **new Claude Code session** after installing, otherwise the `/anatomy` command will not show up.

**Update:**
```sh
cd anatomy && git pull
```

**Uninstall:**
```sh
rm ~/.claude/skills/anatomy   # removes the link only
rm -rf anatomy                # removes the code as well
```

---

## 2. Basic usage

Type `/anatomy` in Claude Code followed by one sentence:

```text
/anatomy explain how a mechanical watch works; let me wind it by hand and take it apart
/anatomy explain how a big airliner turbofan engine works, interactive
/anatomy explain the TCP three-way handshake with one physical object
/anatomy why is a GPU better than a CPU at parallel work
/anatomy explain the token-bucket rate limiter, interactive
```

You can also skip `/anatomy` and just say "make an interactive isometric figure that explains X"; Claude usually picks the skill on its own.

The result is a single `.html` file. Open it in a browser and play with it: hover to explode parts, drag to rotate, use the keyboard. Each figure lists its controls in the bottom-left corner of its card.

---

## 3. Size and time

Claude settles the size before it starts:

| Size | Content | Parts | Rough time |
|---|---|---|---|
| **Figure** | One mechanism | 40–80 | Under an hour; built straight away |
| **Hero** | A complex object, layered effects or 3D | 80–150 | 2–4 hours; Claude tells you first |
| **Epic** | A whole car, building or factory | More | 6 hours or more; **not built directly**, Claude proposes a simplified Hero version first |

For something quicker, say "Figure size is fine" or "keep it simple" in the prompt.

**Usage reference (from one creator's own test):** a 5-minute explainer video used about 1% of a weekly Claude Max (USD 200) allowance. For practice, Sonnet 5.5 saves allowance.

---

## 4. Start with the 8 bundled examples

The examples live in `examples/`, and each one opens directly:

| Example | What it is |
|---|---|
| `dial-indicator` | A dial indicator, the smallest example (about 300 lines); **start here** |
| `desk-computer` | A desktop computer: type on the keyboard and the text appears on the CRT |
| `arcade-cabinet` | An arcade cabinet: insert a coin → it rolls down → trips a switch → the screen turns on |
| `raptor-engine` | A rocket-engine test stand that explodes into 11 parts on hover, with WebGL exhaust |
| `ripple-tank` | A ripple tank: two interfering wave trains with projected caustics |
| `test-rig` | A spring-button test rig |
| `spot-plate` | Water drops merging on a glass plate |
| `turning-dial` | A 3D combination dial with a robot hand (beta) |

To open one (the dial indicator as an example):

```sh
cd anatomy/examples/dial-indicator
node build.mjs            # dark theme → dial-indicator.html
node build.mjs --light    # light theme → dial-indicator-light.html
open dial-indicator-light.html
```

---

## 5. Going further: turn a figure into a narrated short video

Anatomy itself only outputs **interactive web pages**, not video. To make a video, ask Claude to build the figure and then record it with narration, voice-over and subtitles.

**Prompt template:**
```text
Use the anatomy skill to make a short explainer video on how a mechanical watch works:
- Build the interactive model first, then play it automatically in teaching order
  (whole object → exploded view → each part → the mechanism running)
- Narrate every step: what this is, what is happening now, why, and what it is for
- Use an edge-tts voice for the narration (not the built-in macOS `say`, which sounds odd)
- Highlight key words in the subtitles; vertical 1080×1920, 60 fps, 1–2 minutes long
```

How it fits with code-drawn video pipelines such as [opus-video-studio](https://github.com/xdis/opus-video-studio):
- A 2D canvas explainer is best for curves, data and formulas.
- **Anatomy** is best for **mechanisms, physical devices and "how the inside of a thing works"**: engines, watches, locks, GPUs, camera shutters.
- Both are "Claude writes code → Chrome renders frame by frame": no per-video cost, and every frame is crisp. The same recording, voice-over and subtitle steps work for both.

---

## 6. Prompting tips

1. **Say which actions you want to see:** "let me wind it by hand", "let me take it apart", "press Space to ignite".
2. **Say who it is for:** "for primary-school kids", "for someone with no background"; the explanation gets simpler.
3. **Pick a theme:** light is the default preference. Ask for "dark theme" if you want dark.
4. **Pick a size:** "Figure size" is faster; "Hero size, as detailed as possible" looks better.
5. **Abstract ideas work too:** the skill invents a matching machine. For example, a cache hit becomes a pin-tumbler lock that only turns when every pin lines up.

---

## 7. Things to watch

- **Check the content.** Experts have pointed out that some figures "look very professional but explain things a bit oddly". A layperson sees polish; an expert sees mistakes. Before publishing, verify the numbers and the principle yourself, or ask Claude to list its sources.
- **Nothing is published automatically.** The skill includes `scripts/publish.mjs`, which submits a figure to the author's community gallery on GitHub. **It runs only when you explicitly say "publish to the anatomy gallery"**; otherwise nothing is uploaded.
- **Heat.** Rendering uses only Chrome, with no local AI model on the GPU, so the machine stays cool.
- **Time.** Detailed figures take hours. Ask for a Figure version first to check the look, then decide whether to go deeper.

---

## 8. Layout (for reference)

```
anatomy/
├── SKILL.md          # the skill instructions (read by Claude)
├── HOW_TO_USE.md     # this guide
├── README.md         # upstream reference
├── kit/              # drawing toolkit (isometric projection, cylinders, pipes, shaders…)
├── references/       # detailed specs (craft, motion, WebGL, 3D, self-check)
├── scripts/          # screenshots, auto-demo, line checks, publishing
├── examples/         # 8 complete examples
└── tests/            # tests
```
