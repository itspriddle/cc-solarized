# Claude Code Theme Reference

A complete reference for the `~/.claude/themes/*.json` schema, including all 69 reachable tokens. Pairs with [`theme.schema.json`](./theme.schema.json) for editor validation and autocomplete.

> Custom themes require Claude Code v2.1.118 or later.

## Quick start

```jsonc
// ~/.claude/themes/my-theme.json
{
  "$schema": "./theme.schema.json",
  "name": "My Theme",
  "base": "dark",
  "overrides": {
    "claude": "#a89278",
    "error": "#e8836f",
    "success": "#b7bd73"
  }
}
```

Selecting the theme via `/theme` stores `custom:<filename-slug>` as the preference. Claude Code watches the directory and reloads on file change.

## Top-level fields

| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `$schema` | string | No | Editor hint for autocomplete/validation. Ignored by Claude Code. |
| `name` | string | No | Display label in `/theme`. Defaults to filename slug. |
| `base` | enum | No | Built-in preset to inherit from. Defaults to `dark`. |
| `overrides` | object | No | Sparse map of token → color. Tokens not listed fall through to `base`. |

## Base presets

| `base` value | Notes |
| :--- | :--- |
| `dark` | Default; standard dark theme. |
| `light` | Standard light theme. |
| `dark-daltonized` | Dark variant tuned for color-vision-deficiency safety. |
| `light-daltonized` | Light daltonized variant. |
| `dark-ansi` | Uses 16-color ANSI palette only — best for terminals with custom palettes. |
| `light-ansi` | ANSI light variant. |

## Color value formats

| Form | Pattern | Example |
| :--- | :--- | :--- |
| Hex RGB | `#rrggbb` | `#a89278` |
| Short hex | `#rgb` | `#a87` |
| Functional RGB | `rgb(r,g,b)` | `rgb(168, 146, 120)` |
| 256-color | `ansi256(n)` (0–255) | `ansi256(180)` |
| Named ANSI | `ansi:<name>` | `ansi:cyanBright` |

The 16 valid ANSI names: `red`, `green`, `blue`, `yellow`, `magenta`, `cyan`, `white`, `black`, plus a `Bright` suffix on each (`redBright`, `greenBright`, …).

## Failure mode

Per the official docs: **"Unknown tokens and invalid color values are ignored, so a typo cannot break rendering."** Strict validation in [`theme.schema.json`](./theme.schema.json) catches typos in your editor — runtime is forgiving either way.

## Token catalog

Badges:
- **Documented** — listed in the [official color token reference](https://code.claude.com/docs/en/terminal-config#color-token-reference)
- **Internal** — present in the binary's preset object, reachable via `overrides`, but not in the public docs (descriptions inferred from naming and surrounding code)

### Brand and accent

| Token | Status | Controls |
| :--- | :--- | :--- |
| `claude` | Documented | Primary brand accent — spinner, assistant label |
| `claudeShimmer` | Documented | Lighter color paired with `claude` in animated gradients |
| `claudeBlue_FOR_SYSTEM_SPINNER` | Internal | Blue brand variant for the system spinner |
| `claudeBlueShimmer_FOR_SYSTEM_SPINNER` | Internal | Shimmer paired with `claudeBlue_FOR_SYSTEM_SPINNER` |
| `professionalBlue` | Internal | Anthropic-blue marketing accent (login, billing) |
| `chromeYellow` | Internal | Yellow marketing accent |

### Foreground text

| Token | Status | Controls |
| :--- | :--- | :--- |
| `text` | Documented | Default foreground text |
| `inverseText` | Documented | Text on top of colored backgrounds (badges) |
| `inactive` | Documented | Hints, timestamps, disabled items |
| `inactiveShimmer` | Internal | Shimmer paired with `inactive` |
| `subtle` | Documented | Faint borders and de-emphasized text |
| `suggestion` | Internal | Suggestion-tier UI accents (autocomplete hints) |
| `remember` | Documented | Memory and `CLAUDE.md` indicators |
| `background` | Internal | Generic surface background fill |

### Status

| Token | Status | Controls |
| :--- | :--- | :--- |
| `success` | Documented | Success messages and passing checks |
| `error` | Documented | Errors and failures |
| `warning` | Documented | Warnings, cautions, auto-mode border |
| `warningShimmer` | Documented | Shimmer paired with `warning` |
| `merged` | Documented | Merged pull request status |

### Input box and mode indicators

| Token | Status | Controls |
| :--- | :--- | :--- |
| `promptBorder` | Documented | Input box border (default mode) |
| `promptBorderShimmer` | Internal | Shimmer paired with `promptBorder` |
| `permission` | Documented | Permission prompts and pickers |
| `permissionShimmer` | Internal | Shimmer paired with `permission` |
| `planMode` | Documented | Plan mode accent and border |
| `autoAccept` | Documented | Accept-edits mode accent and border |
| `bashBorder` | Documented | Input border when entering `!` shell command |
| `ide` | Documented | IDE connection indicator |
| `fastMode` | Documented | Fast mode indicator |
| `fastModeShimmer` | Internal | Shimmer paired with `fastMode` |

### Diff rendering

| Token | Status | Controls |
| :--- | :--- | :--- |
| `diffAdded` | Documented | Background of added lines |
| `diffRemoved` | Documented | Background of removed lines |
| `diffAddedDimmed` | Documented | Unchanged context near added lines |
| `diffRemovedDimmed` | Documented | Unchanged context near removed lines |
| `diffAddedWord` | Documented | Word-level highlight in added lines |
| `diffRemovedWord` | Documented | Word-level highlight in removed lines |

### Fullscreen mode (background fills)

These apply only when `/tui fullscreen` rendering is active.

| Token | Status | Controls |
| :--- | :--- | :--- |
| `userMessageBackground` | Documented | Background behind user messages |
| `userMessageBackgroundHover` | Internal | Hover variant of `userMessageBackground` |
| `messageActionsBackground` | Internal | Per-message action affordance backgrounds |
| `bashMessageBackgroundColor` | Internal | Background for shell-command messages |
| `memoryBackgroundColor` | Internal | Background for memory / `CLAUDE.md` annotations |
| `selectionBg` | Documented | Mouse-selection background |

### Subagent palette

Subagents declared with `color: <name>` in their YAML frontmatter draw using the matching token. Override these to recolor your agent transcripts.

| Token | Status |
| :--- | :--- |
| `red_FOR_SUBAGENTS_ONLY` | Documented |
| `blue_FOR_SUBAGENTS_ONLY` | Documented |
| `green_FOR_SUBAGENTS_ONLY` | Documented |
| `yellow_FOR_SUBAGENTS_ONLY` | Documented |
| `purple_FOR_SUBAGENTS_ONLY` | Documented |
| `orange_FOR_SUBAGENTS_ONLY` | Documented |
| `pink_FOR_SUBAGENTS_ONLY` | Documented |
| `cyan_FOR_SUBAGENTS_ONLY` | Documented |

### Rate-limit indicator

| Token | Status | Controls |
| :--- | :--- | :--- |
| `rate_limit_fill` | Internal | Filled portion of the usage bar |
| `rate_limit_empty` | Internal | Empty portion of the usage bar |

### Brief mode

| Token | Status | Controls |
| :--- | :--- | :--- |
| `briefLabelYou` | Internal | "You" label in compact transcript mode |
| `briefLabelClaude` | Internal | "Claude" label in compact transcript mode |

### Mascot easter egg

| Token | Status | Controls |
| :--- | :--- | :--- |
| `clawd_body` | Internal | Body color for the Clawd mascot art |
| `clawd_background` | Internal | Background behind the Clawd mascot art |

### Rainbow palette (animated celebration sequences)

Each color has a `*_shimmer` partner for gradient animation.

| Pair | Status |
| :--- | :--- |
| `rainbow_red` / `rainbow_red_shimmer` | Internal |
| `rainbow_orange` / `rainbow_orange_shimmer` | Internal |
| `rainbow_yellow` / `rainbow_yellow_shimmer` | Internal |
| `rainbow_green` / `rainbow_green_shimmer` | Internal |
| `rainbow_blue` / `rainbow_blue_shimmer` | Internal |
| `rainbow_indigo` / `rainbow_indigo_shimmer` | Internal |
| `rainbow_violet` / `rainbow_violet_shimmer` | Internal |

## Methodology

The 35 documented tokens come from the official docs at <https://code.claude.com/docs/en/terminal-config#color-token-reference>. The remaining 34 internal tokens were extracted from the canonical preset object inside the Claude Code 2.1.126 binary at `~/.local/share/claude/versions/<version>`:

```bash
strings -n 4 ~/.local/share/claude/versions/<version> > /tmp/cc-strings.txt
awk '/claudeShimmer/ { p=index($0,"YD4="); print substr($0,p,8000); exit }' /tmp/cc-strings.txt
```

That dumps the dark preset object — six preset variables (`YD4`, `wD4`, `DD4`, `jD4`, `JD4`, `MD4`) all share the same key set; the 69 keys define the complete tunable surface. Any future token added by Anthropic will appear in those preset literals first; rerun the awk extraction against a newer binary to refresh this catalog.

## See also

- [`theme.schema.json`](./theme.schema.json) — JSON Schema for editor validation
- [`artificer.json`](./artificer.json) — example custom theme
- [Official theme docs](https://code.claude.com/docs/en/terminal-config#color-token-reference)
- [Plugins reference: themes](https://code.claude.com/docs/en/plugins-reference#themes) — for shipping themes via plugins

---

Archived from https://gist.github.com/cameronsjo/34a6fb8ade2b44c8380e1a2adebbac2b
