HookCrashers 5.0.2

- Painter Boss Paradise configuration is now isolated per mod as `hcpbp_<modname>.dat`.
- The vanilla `ccpbp_config.dat` remains untouched.
- This prevents stale or incompatible Workshop selections from vanilla or another mod from crashing the character selection menu.
- Save and PBP filename redirection is applied atomically and validates the supported executable references before patching.
