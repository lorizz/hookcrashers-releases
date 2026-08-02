HookCrashers 5.0.63-dev

Branch: v5
Commit: 3917e50c2d45a2f25ebf216c654ad3c243399025


### Added

- Custom mod localization loading from `localizations.json`, with English fallback and support for all eleven game languages.
- The ActionScript `GetLocalization("id")` native, returning a HookCrashers localization id for assignment to `ntext`.

### Fixed

- Updated the custom SWF registration, string cache, dispatcher, and `ntext` lookup RVAs for the current Castle Crashers executable.

