HookCrashers 5.0.64-dev

Branch: v5
Commit: 57bdb88a9005eee2547b97f3d4d7002f9be027ba


### Fixed

- Isolated Painter Boss Paradise configuration per mod as `hcpbp_<modname>.dat`, preventing stale vanilla or another mod's Workshop selections from being loaded by HookCrashers.
- Made save and PBP filename redirection atomic and rejected unexpected PBP filename operands from unsupported executable revisions.

