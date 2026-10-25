HookCrashers 5.0.67-dev

Branch: v5
Commit: aebf92ab5394dd540cd0adf5c5d07048b4071f0d


### Fixed

- Raised the effective main game allocation budget from 576 MiB to 1024 MiB using on-demand overflow arenas, including native free routing and reset cleanup. Existing allocations retain their addresses. This targets the null-buffer allocation failure observed with Ultra graphics and during multiplayer loading.
- Corrected the addon unlock fallback native address and validated its instructions before enabling the fallback; added numeric argument conversion and player-index checks.
- Retained only the final successful patched SWF buffer instead of retaining intermediate copies, with synchronized ownership bookkeeping.

### Added

- Configurable `game_pool_mib` (default 1024; 0 disables expansion; supported budgets 576..1536 MiB), allocator compatibility checks, and growth/failure logging.
- Offline tests against allocator routines from a user-provided executable snapshot, including actual Detours routing, 16 MiB zero-fill, budget enforcement, free/reset, and overflow concurrency. No proprietary executable snapshot or extracted fixture is included.

### Validation

- Local Release/Win32 build, five executable compatibility checks, and native allocator/core hook integration tests passed. Ultra graphics and multiplayer gameplay have not yet been validated with this build.

