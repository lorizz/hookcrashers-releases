HookCrashers 5.0.68-dev

Branch: v5
Commit: 4265bad4286037a526fc71213f44c85060d0e494


### Fixed

- Corrected the no-Painter-DLC addon unlock fallback to call the game's native with `__stdcall` and an AL Boolean return. The previous `__cdecl` declaration removed its two arguments twice, unbalancing the stack by eight bytes during lobby script evaluation.
- Corrected the expanded character sync receiver's roster insertion target from RVA `0x12B0E0` (inside an unrelated instruction) to the native entry at `0x2B030`, as called by the vanilla receiver. This separate defect predates the no-DLC fallback.
- Added instruction checks for all unlock-native return paths and the roster insertion entry/return before enabling the affected hooks.

### Validation

- Local Release/Win32 build and eight executable compatibility checks passed.
- Offline tests using isolated native routines reproduced the old eight-byte stack imbalance, passed 100000 corrected unlock calls across four players, and verified existing/new roster ID insertion with an isolated insertion dependency.
- Retains the 1 GiB game pool from 5.0.67-dev. Tester feedback confirms Ultra graphics now loads; the Blacksmith level lag spike remains under investigation. Multiplayer lobby gameplay with these fixes still needs tester verification.

