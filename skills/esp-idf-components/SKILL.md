---
name: esp-idf-components
description: Create, configure, test, package, and publish ESP-IDF components. Use when adding project-local components, consuming Registry dependencies, designing reusable component APIs, writing manifests or Kconfig, building test applications, or preparing Component Registry releases.
---

# ESP-IDF Components

## When to Use

- Creating a project-local or standalone reusable component.
- Choosing between a Registry dependency and new implementation.
- Defining component APIs, CMake dependencies, Kconfig, manifests, tests, or CI.
- Preparing or reviewing an ESP Component Registry package.

## Workflow

### 1. Decide whether a new component is needed

- Search the ESP Component Registry first. Prefer Registry MCP tools when
  available and the public API or website as a fallback.
- Compare candidates by ESP-IDF and target compatibility, maintenance, license,
  and API fit. Ask the user only when the remaining choice is material.
- Consume a suitable existing component instead of duplicating it.
- Keep product-specific code under the application's `components/`; use a
  standalone package for independently versioned, reusable code.

### 2. Add an existing dependency

- From the project root, run
  `idf.py add-dependency "namespace/component^<VERSION>"`; this updates `main`
  by default.
- Use `--component=<NAME>` for a component under `components/` or
  `--path=<PATH>` for another component directory.
- Keep version constraints explicit when reproducibility matters. Never edit
  `dependencies.lock` or `managed_components/` manually.

### 3. Scaffold a new component

- Use `idf.py create-component <COMPONENT_NAME>` instead of manually generating
  boilerplate. Run it in the intended parent directory or use `-C <PATH>`.
- For a local component, scaffold under `components/`.
- For a standalone package, scaffold in its repository root and add packaging
  files only as needed.
- Create a manifest with `idf.py create-manifest`; use `--component=<NAME>` or
  `--path=<PATH>` when needed. `compote manifest create` is the modern
  Component Manager equivalent when Compote is installed.
- Read [references/component_manager.md](references/component_manager.md) for
  complete CLI, layout, Registry, and CI details.

### 4. Use clear structure

- Keep public headers in `include/`.
- Keep implementation and private headers outside the public include directory.
  Add a private include directory with `PRIV_INCLUDE_DIRS` when several source
  files share private headers.
- A minimal component contains `CMakeLists.txt`, `include/<name>.h`, and source
  files. Reusable packages normally add `idf_component.yml`, `README.md`,
  `LICENSE`, and at least one example or test application.
- Never edit generated files under `build/`.

### 5. Register the component correctly

- Use `idf_component_register(...)` in the component `CMakeLists.txt`.
- Declare `SRCS` and `INCLUDE_DIRS` explicitly.
- Use `REQUIRES` for public dependencies exposed through the component's public headers.
- Use `PRIV_REQUIRES` for implementation-only dependencies.
- Do not rely on incidental transitive dependencies.

Example:

```cmake
idf_component_register(
    SRCS "my_component.c"
    INCLUDE_DIRS "include"
    REQUIRES esp_timer
    PRIV_REQUIRES driver
)
```

### 6. Define clean component boundaries

- Keep the public API small and stable.
- Avoid leaking unrelated ESP-IDF headers through public headers unless they are part of the intended API surface.
- Prefer opaque handles, explicit init/deinit functions, and `esp_err_t` return values for non-trivial components.
- Split unrelated responsibilities into separate components instead of building one large utility component.

### 7. Use configuration files deliberately

- Use `Kconfig` for component-local options.
- Use `Kconfig.projbuild` only when the option must appear at project scope in `menuconfig`.
- Namespace configuration symbols, for example `MY_COMPONENT_*`.
- Every new config option should include:
  - clear prompt text
  - sensible default
  - brief help text
- After changing `Kconfig` or `Kconfig.projbuild`, ask whether to run
  `idf.py reconfigure`. A normal build detects definition changes, but changed
  defaults may not override values already stored in `sdkconfig`.

## Reusable Component Checklist

- Keep the package independent of application globals, secrets, and hardcoded
  board choices.
- Add `idf_component.yml` when Component Manager metadata or dependencies are
  needed and before normal Registry publication.
- Add a `README.md` that explains purpose, configuration, dependencies, and usage.
- Add a `LICENSE` file if the component will be shared outside the project.
- Include at least one example or test app when the behavior is non-trivial.
- Add CI that at minimum builds supported targets and validates example or test apps when they exist.
- Keep Kconfig options and defaults minimal and documented.
- Verify the component builds cleanly for its supported targets.
- Use the registry upload flow only when the component is actually intended to be published.
- Require a new `idf_component.yml` `version` before CI publishes an updated registry release.

## Validation and Handoff

- Build every relevant example or test application for the component's supported
  target matrix. Follow repository CI when it defines the matrix.
- Validate package creation from the component root with
  `compote component pack --name <NAME>`.
- When Registry credentials and namespace access are authorized, validate an
  upload without publishing:
  `compote component upload --name <NAME> --namespace <NAMESPACE> --dry-run`.
- Keep publishing workflows in dry-run mode by default. Require an explicit
  production choice before creating an immutable Registry version.
- Report the ESP-IDF version, targets built, tests run, package-validation
  results, and anything not validated.

## Avoid

- Creating a new local component before checking for an existing dependency
- Using the reusable component template for a one-off project-local helper under `components/`
- Putting all app code into `main/` when it has clear component boundaries
- Exposing private implementation details in public headers
- Using `REQUIRES` when `PRIV_REQUIRES` is sufficient
- Editing generated files under `build/`
- Editing `managed_components/` or `dependencies.lock` manually
- Adding broad config churn to `sdkconfig` when defaults belong in `sdkconfig.defaults*`
