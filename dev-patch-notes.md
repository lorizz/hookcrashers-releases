HookCrashers 5.0.65-dev

Branch: v5
Commit: 42ef18245bf47d27e3bfa32cfc784f78efeb4267


### Added

- Native Win32/x86 ASI loading from dedicated `mods/<mod>/*.asi` folders, including deterministic load order and optional `HookCrashersModInit` / `HookCrashersModShutdown` exports.
- A packaged C++ SDK containing `HookCrashers.h` and `HookCrashers.lib`.
- Public native-mod logging and runtime version exports.

### Fixed

- Replaced stale C++ API documentation that referenced exports and headers absent from the v5 build.

