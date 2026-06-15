# Fork Philosophy

This fork is intentionally radical and personal. The goal is to shape Zed around the user's own tastes, even when that means adding brand new functionality, removing existing behavior, or diverging sharply from upstream Zed.

Do not prioritize compatibility with standard Zed. Do not avoid changes merely because they would make it harder to merge from upstream later. The user has explicitly chosen not to worry about keeping this fork up to date with its origin.

When working in this fork, prefer bold, coherent product decisions over conservative compatibility-preserving changes. Existing Zed behavior is not sacred; it may be changed, replaced, or deleted when doing so better serves the personalized editor experience.

# Fork Defaults

To defaultize a user-facing setting in this fork, change the built-in default in `assets/settings/default.json`. Do not change Rust code for settings that already have entries there.

Keep `assets/settings/initial_user_settings.json` empty aside from the minimal valid settings object. It is only the template used when creating a user settings file, and duplicating defaults there turns them into user-level overrides.

# Fork Keymaps

Customize the user's default keybindings in `assets/keymaps/dario.json`. Do not edit `assets/keymaps/default-linux.json`, `assets/keymaps/default-macos.json`, or `assets/keymaps/default-windows.json` for personal workflow changes.

# Hiding vs Removing Commands

When a command should disappear from the user-facing command palette but remain available for compatibility or internal dispatch, hide it with `CommandPaletteFilter` instead of removing the action. If the user says to "nuke" commands, clarify whether they truly mean deleting the action or just hiding it; most existing actions should be hidden rather than removed.

# Build Workflow

Rust builds in this repository are expensive, especially after touching high-fanout crates like `zed_actions`, `workspace`, `ui`, `settings`, `editor`, or `gpui`. Batch related implementation changes before validating.

Default build validation command: `cargo build -p zed`. Use this after all intended changes are made, not repeatedly after every small edit. This single command both validates compilation and produces the runnable `target/debug/zed.exe`, which is exactly what the user wants for normal change/build/test loops.

Avoid separate `cargo check` + `cargo build` workflows by default, because that doubles expensive compiler work. Use `cargo check` only for targeted intermediate feedback when a risky Rust change needs faster validation before continuing, and still minimize how often any Rust build/check command is run.
