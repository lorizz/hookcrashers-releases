HookCrashers 5.0.66-dev

Branch: v5
Commit: dbf13325a4d3e5caef1a58cc52f736f674e130d5


### Fixed

- Added Classic-mode addon character selection for players who do not own or have disabled the Painter Boss Paradise DLC.
- Kept the proven 5.0.3 Fresh and Workshop character paths unchanged while applying the no-DLC fallback only to addon character unlock queries.
- Restored configured `initiallyUnlocked` addon slots and cleared stale Workshop unlock bits without overwriting existing addon progress.
- Made the ImGui mouse cursor appear while the debug console is open and disappear when the console closes.

### Changed

- Expanded the runtime Classic lobby selector scan to include every registered addon slot, with locked characters still filtered by their save data.

