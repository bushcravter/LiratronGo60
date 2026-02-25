# Agent Guide — MoErgo Go60 ZMK Config

## What This Is

ZMK firmware configuration for the [MoErgo Go60](https://moergo.com/go60-support) wireless split keyboard. This repo produces a single `go60.uf2` firmware file that gets flashed to both halves.

## Key Files

| File | Purpose |
|---|---|
| `config/go60.keymap` | Keymap and layers (devicetree format) |
| `config/go60.conf` | Kconfig settings (RGB, power, features) |
| `config/default.nix` | Nix build expression — builds left + right halves and combines into one UF2 |
| `.github/workflows/build.yml` | CI — builds firmware on push via GitHub Actions |

## How to Build

Firmware is built via Nix, not Brazil. Two options:

1. **GitHub Actions** (primary): push to repo → Actions builds → download `go60.uf2` from artifacts
2. **Local Docker**: `./build.sh` (requires Docker)

Do NOT use `brazil-build` — this is not a Brazil package.

## How to Make Changes

Only modify files inside `config/`. Everything else is build infrastructure from the [upstream template](https://github.com/moergo-keyboards/go60-zmk-config) and should not be touched.

- **Keymap changes** (layers, bindings, combos, macros): edit `config/go60.keymap`
- **Feature toggles** (RGB, power management, Bluetooth): edit `config/go60.conf`
- Both files apply to both halves — the Nix build handles left/right separation

## Config Format

`go60.conf` uses ZMK Kconfig syntax:
```ini
CONFIG_OPTION_NAME=value
```

`go60.keymap` uses ZMK devicetree syntax (C-like with `/ { }` blocks, `&behavior` references).

## Documentation References

- [ZMK Docs](https://zmk.dev/docs) — firmware features, config options, keycodes
- [ZMK Kconfig Reference](https://zmk.dev/docs/config) — all `CONFIG_*` options
- [ZMK Keymap Behaviors](https://zmk.dev/docs/keymaps/behaviors) — hold-tap, macros, layers, etc.
- [Go60 User Guide](https://docs.moergo.com/go60-user-guide/) — hardware specifics, RGB, battery, tenting
- [MoErgo Layout Editor](https://docs.moergo.com/layout-editor-guide/) — visual keymap editor (alternative to manual editing)
- [MoErgo ZMK Fork](https://github.com/moergo-sc/zmk) — Go60-specific ZMK distribution

## Important Notes

- This is NOT a Brazil package — do not use `brazil-build`
- After flashing, both halves need the same firmware
- RGB settings changed via Magic Layer keys persist to flash — `*_START` values are only initial defaults
