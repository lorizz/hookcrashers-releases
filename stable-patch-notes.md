HookCrashers 5.0.3

- Native Win32/x86 ASI mods can now be loaded from dedicated `mods/<mod>/*.asi` folders.
- Native mods load in deterministic order and may export `HookCrashersModInit` and `HookCrashersModShutdown`.
- Public C++ APIs now expose logging, the HookCrashers runtime version, character registration, and prompts.
- `HookCrashers-SDK.zip` contains the x86 import library, public header, a buildable example, and the Lua/C++ modding guide.
- Root-level ASI files now log the dedicated-folder requirement instead of being silently ignored.
