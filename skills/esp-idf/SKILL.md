---
name: esp-idf
description: Develop, configure, build, flash, monitor, and debug ESP-IDF firmware. Use for ESP32-series project setup, EIM environments, target selection, build failures, hardware validation, crash analysis, or Component Registry dependencies.
---

# ESP-IDF Firmware Engineering

## When to Use

- Developing or configuring ESP-IDF firmware applications.
- Troubleshooting compilation errors or dependency issues in ESP-IDF projects.
- Flashing authorized hardware and verifying runtime behavior.
- Debugging run-time crashes using the decoded monitor output.

## Project and Environment

1. Locate the project root containing the top-level `CMakeLists.txt`. In a
   multi-application repository, identify every affected project root.
2. Read the README, CI configuration, `sdkconfig.defaults*`, manifests, and build
   scripts to determine the required ESP-IDF version, targets, and validation.
3. When creating a project, use `idf.py create-project <PROJECT_NAME>` rather
   than manually generating scaffold files.
4. Verify the environment with `idf.py --version`. Prefer repository-prescribed
   setup; otherwise use EIM:
   - `eim --version`
   - `eim list` to find installed versions, selected version, and absolute paths
   - `eim select <ESP_IDF_VERSION>`
   - `eim run "idf.py --version" <ESP_IDF_VERSION>`
   - `eim run "idf.py <command>" <ESP_IDF_VERSION>`
   - `eim install -i <ESP_IDF_VERSION>` only when installation is authorized
   - `eim wizard` only when interactive installation is requested
5. If EIM is unavailable but the required ESP-IDF environment is already valid,
   continue with that environment. Do not block work solely to install EIM.
6. Use version-matched ESP-IDF documentation and migration guides.

## Target and Hardware Identification

- Determine the project target from configuration and build output before probing
  hardware.
- Ask before `idf.py set-target <TARGET>`. It clears `build/`, moves `sdkconfig`
  to `sdkconfig.old`, and reconfigures the project; do not follow it with a
  redundant `fullclean`.
- Probe hardware only when the task requires it and a port is confirmed. Check
  the installed esptool help and use its supported syntax, for example:
  - Current: `esptool --port <PORT> chip-id`
  - Legacy: `esptool.py --port <PORT> chip_id`
- If automatic bootloader entry fails, ask the user to place the device in
  download mode before retrying.
- Compare the configured target with the physical chip, boot-time flash settings,
  and application metadata. Do not report a device MAC address unless required.

## Build and Configuration

- Use `idf.py build` for normal incremental builds and inspect the first relevant
  errors plus referenced logs when a build fails.
- Treat `sdkconfig` as generated. Put intentional defaults in
  `sdkconfig.defaults*`; a target-specific defaults file requires a base
  `sdkconfig.defaults`, even if the base file is empty.
- After changing `Kconfig` or `Kconfig.projbuild`, ask whether to run
  `idf.py reconfigure`. A build normally detects Kconfig definition changes, but
  a changed default may not override a value already stored in `sdkconfig`.
- Never edit `build/`, `managed_components/`, or `dependencies.lock` manually.
- Use `idf.py fullclean` only for demonstrated stale-build problems and only with
  user approval.

## Flash, Monitor, and Diagnose

- Never flash merely because a board is connected. Flash only when requested or
  explicitly authorized after confirming the port, target, and that the device
  is safe to overwrite.
- Flash with `idf.py -p <PORT> flash`; use
  `idf.py -p <PORT> flash monitor` when runtime validation is authorized.
- Use `idf.py -p <PORT> monitor` for crash analysis so addresses in backtraces are
  decoded to source locations.
- Capture output until the requested behavior or crash occurs, stop the monitor
  cleanly, and summarize the relevant evidence.
- Send interactive input only when the firmware exposes a console and the
  terminal supports it.
- Treat the linenoise warning about unsupported escape sequences as harmless; it
  disables line editing and history, not firmware behavior.
- For suspected memory corruption or leaks, first inspect available evidence,
  then propose the appropriate heap tracing or debugging configuration and obtain
  approval before changing configuration.

## ESP-IDF Code Conventions

- Keep the entry point as `void app_main(void)`.
- Prefer ESP-IDF APIs and includes over ad-hoc platform code.
- Use `ESP_LOGI/W/E` instead of excessive `printf` unless required.
- Register components with `idf_component_register(...)`; declare dependencies
  with `REQUIRES` or `PRIV_REQUIRES`.
- Handle `esp_err_t` results explicitly and avoid long blocking operations
  without yielding where appropriate.

## Component Registry

- Search the ESP Component Registry before implementing a dependency locally.
- Prefer the Registry MCP tools when available; use the public REST API as a
  fallback.
- Add dependencies from the project root with
  `idf.py add-dependency "namespace/component^<VERSION>"`. Use
  `--component=<NAME>` or `--path=<PATH>` when the dependency belongs to a
  component other than `main`.
- If several candidates satisfy the request, compare compatibility, maintenance,
  licensing, and API fit. Ask the user only when the choice remains material.
- For REST endpoints and examples, read [component_registry.md](component_registry.md).

## Handoff

- Report commands run, ESP-IDF version, target, tests, and hardware used.
- Summarize configuration changes and clearly identify anything not validated.
