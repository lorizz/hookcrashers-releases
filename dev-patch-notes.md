HookCrashers 5.1.0-dev

Branch: v5
Commit: 38b0ad0bea12a692b321f61f2de7d83f9f2a67b6


### Added

- Expanded the C++ SDK with `GetCurrentCharacterData(slot)` for HUD/player slots 1..4: character identity, health/magic, level/XP, gold, stats, equipment, consumables and other runtime values. Numeric validity and explicit live/HUD/loading/stale states distinguish unavailable fields from zero.
- Added bounded enumeration of ActionScript properties, arrays and nested objects, including addon fields, with sequence checks and truncation/read-fault flags. Public snapshots contain no raw VM pointers.
- Added `RegisterUpdateCallback` / `UnregisterUpdateCallback`: a `void __stdcall` callback runs on the game thread after snapshot publication. Successful unregistration waits for active calls. Duplicate registrations are rejected and faulting callbacks are disabled.
- Added a C++ ASI mini tutorial and buildable CharacterLevelWatch example with `void Update()`, four-player level-increase polling and baseline resets for loading or identity changes.
- Committed the x86 import library under `sdk/lib` and added `HookCrashers-SDK-Win32.zip` to private/public release assets. CI verifies the packaged example links against the freshly built SDK.

### Changed

- Starts the 5.1 C++ runtime API series. Release versions now come from the explicit `VERSION` file rather than the CI run number; development builds append `-dev`.
- Retains the 1 GiB pool and multiplayer native calling-convention/roster fixes from 5.0.68-dev.

### Validation

- Release/Win32 build, ten supported-executable compatibility checks, production API tests on a four-player VM model, and SDK consumer/example compilation passed locally.
- Offline tests cover isolated actual native getters/hash/map/array routines, the ECX/XMM1 frame bridge, Detours attach/capture/detach on the native disabled-update branch, callback ordering, level-up comparison, fault isolation, concurrent reads and synchronized unregistration.
- No game was launched. The full in-game object chain/lifecycle and achievement behavior still require tester verification; polling does not recover intermediate changes between captures or loading gaps.


## C++ ASI mini tutorial — 5.1.0-dev

### 1. Get the SDK

Download **HookCrashers-SDK-Win32.zip** from this release and extract it.
It contains `sdk/include`, `sdk/lib/HookCrashers.lib`, the API reference in
`docs/current-character-api.md`, and buildable examples in `examples/native`.
Use the **HookCrashers.asi from the same release** in your Castle Crashers folder;
older builds do not export the character/update APIs. The SDK is not needed on
the player's machine. `HookCrashers.lib` is an import library for the ASI, not
a standalone copy of HookCrashers.

### 2. Build the working example

Install Visual Studio with **Desktop development with C++**, the MSVC C++ tools
and a Windows SDK. Open
`examples/native/CharacterLevelWatch/CharacterLevelWatch.vcxproj`, select
**Release | Win32**, then Build. If Visual Studio asks to retarget the compiler
or Windows SDK to your installed version, accept it. Castle Crashers is 32-bit:
an x64/ARM64 mod will not load, even on a 64-bit OS.

The result is `examples/native/CharacterLevelWatch/bin/Release/CharacterLevelWatch.asi`.
The project locates the extracted SDK automatically. Its source is
`examples/native/CharacterUpdate.cpp`; it provides a normal `void Update()` that
checks all four player slots and logs a level increase once.

### 3. Install your mod

Install the matching HookCrashers drop-in build, including its ASI loader.
Create this layout next to `castle.exe` (copy the example's `manifest.yaml`):

```text
CastleCrashers/
  castle.exe
  dinput8.dll
  HookCrashers.asi
  mods/
    CharacterLevelWatch/
      manifest.yaml
      CharacterLevelWatch.asi
```

Keep the mod inside its own folder, not directly in `mods/`. Restart the game
after rebuilding. Check `HookCrashers.log` for the mod-load entry and level-up
messages. No Lua script or addon character is required.

### 4. Write your Update()

The essential callback setup is:

```cpp
#include <HookCrashers.h>

void Update() {
    auto player = HookCrashers::GetCurrentCharacterData(1); // Slots 1..4
    // Check result, source, state and validFields before using a field.
    // Compare player.level with your previous valid reading here.
}
void __stdcall OnUpdate() { Update(); }

HOOKCRASHERS_MOD_EXPORT bool __stdcall HookCrashersModInit() {
    return HookCrashers::RegisterUpdateCallback(OnUpdate);
}
HOOKCRASHERS_MOD_EXPORT void __stdcall HookCrashersModShutdown() {
    HookCrashers::UnregisterUpdateCallback(OnUpdate);
}
```

`OnUpdate` runs on the game thread after the snapshots are updated. Keep it
short; queue achievement uploads or disk operations to a worker. Register in
ModInit and unregister in ModShutdown, not inside Update or DllMain. Before
unloading your own module, successful unregistration must finish outside the
Windows loader lock. HookCrashers handles the optional ModInit/ModShutdown
exports; no custom DllMain or thread loop is required.

For achievement checks require `result == Ok`, `source == LivePlayer`,
`state == InLevel`, and `HasCharacterField` for each field you read. An initial
reading is a baseline, not a level-up. Reset it when the player changes or data
becomes unavailable/loading. The full example implements these checks. Polling
cannot recover intermediate changes between snapshots or across loading gaps.

### 5. Start a fresh project (optional)

Create a **C++ Dynamic-Link Library** project. Use Win32, C++17 or later, and
change **Linker > General > Output File** to `$(OutDir)$(TargetName).asi`.
Add `sdk/include` to C/C++ additional include directories and `sdk/lib` to linker
additional library directories. Add `HookCrashers.lib` to linker input (MSVC
also auto-links it from the header). Do **not** define `HOOKCRASHERS_EXPORTS`
in your consuming mod. Copy the Init/Update/Shutdown setup and create a manifest:

```yaml
id: example.character_level_watch
name: Character Level Watch
author: YourName
version: 1.0.0
hookcrashers_version: ">=5.1.0-dev"
dependencies: []
entrypoints: []
```

The APIs passed offline native/SDK tests. The complete in-game player chain and
gameplay lifecycle still need tester validation in this dev release.
