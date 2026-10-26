# BO3-Customs fork changelog

Changes in `fs0ciety404/PS4-BO3-Customs` beyond upstream `Hi6795/PS4-BO3-Customs`.
Newest first. The upstream project's own changelog lives in `CHANGELOG.md`.

## Unreleased

### Plugin

- **vanilla+ UI Error 36446 fix.** New `k_bubbleGumStubLua` is injected in `AddPcUtil` after
  `k_pcParityLua`. It pre-registers safe fallbacks for `BubbleGumBuffUtility` so Steam-only
  Gobblegum IDs no longer raise `operator .. is not supported for string .. nil` on the
  PS4 BO3 port when `VANILLA+` is activated. Covers the common require paths
  (`ui.t7.utility.bubblegumbuffutility`, `ui.t7.bubblegumbuffutility`,
  `ui.utility.bubblegumbuffutility`, …) via both `package.preload` stubs and
  metatable wrapping of any already-loaded module.
- **`List1ButtonLarge_PH` map-list hook.** New `PatchMapSelectAddPh` rewrites the
  `mapselect.lua` buffer at `ReadScript` return so mods that register their custom maps
  on the `List1ButtonLarge_PH` widget (`wonderweaponpack`, etc.) appear in the Zombies
  custom map list without having to edit `Console.zip`. The injection is idempotent
  and no-ops if the hook is already present or the Playlist anchor is missing.

### CI

- Dropped the `github.repository == 'Hi6795/PS4-BO3-Customs'` guards from the `host`
  and `complete` jobs so the fork runs its own auto-build on push to
  `nexus/standalone-converter`. The release workflow guard remains intact — the fork
  does not publish upstream auto-releases.
- Default `BO3_SPRX_CONFIGURATION` switched from `Debug` to `Release`. Set repo
  variable `BO3_SPRX_CONFIGURATION=Debug` to opt back in to the debug build and its
  kernel + Unity watchdog imports.
- New `release` job on `BO3 Auto Build` publishes a fork prerelease (`fork-<short-sha>`)
  with the built `BO3-Customs.sprx` and `manifest-sprx.json` attached, so updates can
  be downloaded from the GitHub releases page instead of the Actions artifact tab.
