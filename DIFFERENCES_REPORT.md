# Differences Report

This report compares your `k36` configuration against the working reference `lily58` configuration (`silakka54`).

## 1. Missing Behavior Definitions
**Issue:** Your `k36.keymap` is missing the explicit include for mouse key behaviors.
- **Reference (`lily58.keymap`):**
  ```c
  #include <behaviors/mouse_keys.dtsi>
  #include <behaviors.dtsi>
  ```
- **Your Config (`k36.keymap`):**
  ```c
  #include <behaviors.dtsi>
  // Missing mouse_keys.dtsi
  ```
**Why this matters:** In ZMK `main` (which you are tracking), the mouse behaviors (`&mmv`, `&msc`, `&mkp`) are often defined in `mouse_keys.dtsi`. Without this include, these behaviors may be undefined or not correctly initialized, potentially causing them to fallback to transparent or no-op (although usually this would cause a build error, explicitly including it ensures the correct definitions are used).

## 2. Input Event Codes Header
**Issue:** Your keymap may be missing the header that defines input event codes (required for correct resolution of `MOVE_UP`, etc., if they rely on system event codes).
- **Recommendation:** Add `#include <zephyr/dt-bindings/input/input-event-codes.h>` to your keymap.

## 3. ZMK Revision
- **Both** configurations seem to track the `main` branch of ZMK (User via `config/west.yml`, Reference via `app/west.yml` override). This is good, as it means `CONFIG_ZMK_POINTING=y` is the correct flag (replacing older module flags).

## 4. Shield Configuration
- **Reference:** Uses the built-in `lily58` shield definition.
- **Your Config:** Uses a custom `k36` shield definition.
- **Note:** Ensure your `k36.conf` and `k36.dtsi` are compatible with the new input system. Currently, `k36.conf` correctly sets `CONFIG_ZMK_POINTING=y`.

## Summary of Planned Fixes
1.  Add `#include <behaviors/mouse_keys.dtsi>` to `k36.keymap`.
2.  Add `#include <zephyr/dt-bindings/input/input-event-codes.h>` to `k36.keymap`.
