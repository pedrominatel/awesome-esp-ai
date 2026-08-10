# Template Component

This component is a minimal starter template for ESP-IDF components.

## File Layout

- `include/template.h` public API and exported types
- `template.c` private implementation and lifecycle placeholders
- `CMakeLists.txt` component registration and dependencies
- `idf_component.yml` component metadata for the registry

## API

```c
#include "template.h"

template_handle_t handle = template_create();
if (handle == NULL) {
    // Allocation or initialization failed
}

esp_err_t ret = template_delete(handle);
```

### Exported symbols

- `template_create()` allocates and initializes the template handle
- `template_delete()` frees the handle and validates input

## Best Practices for Future Components

- Replace placeholder names, comments, and metadata before publishing
- Add only the dependencies your component actually requires
- Keep public headers free of unrelated driver includes
- Use opaque handles for private state
- Return `esp_err_t` from library code instead of aborting internally
- Add Kconfig options only when users need configurable behavior
- Document examples only when examples exist in the component

## Customization Checklist

- Rename the component directory, files, include guards, and exported symbols
- Replace `YEAR COPYRIGHT HOLDER` in source-file SPDX headers
- Replace `template_create()` and `template_delete()` with the real lifecycle API
- Add component-specific configuration structs, enums, and functions as needed
- Add required dependencies to `CMakeLists.txt`
- Replace the placeholder URL and update the description, version, license, and
  ESP-IDF constraint in `idf_component.yml`
- Expand this README with real hardware, protocol, or usage details

## CI Validation Workflow

`validate_component.yml` builds the example for representative ESP32 and ESP32-C3
targets on pull requests and pushes to `main`. Adjust the ESP-IDF version and
target matrix to match the component's declared support.

The example ignores `dependencies.lock` because its local path dependency
generates a machine-specific absolute path. Applications that use Registry
dependencies should normally commit their generated lock files.

## CI Publishing Workflow

The included GitHub Actions workflow can publish components to the ESP Component
Registry with `espressif/upload-components-ci-action@v2`. It runs only when
manually dispatched and requires explicit component and namespace inputs.

Official documentation:
- [ESP-IDF Component Manager](https://docs.espressif.com/projects/idf-component-manager/en/latest/index.html)

### Workflow behavior

- Trigger: manual `workflow_dispatch`
- Namespace and component paths: supplied as workflow inputs
- Authentication: GitHub OIDC trusted uploader
- Dry run: enabled by default; disable it explicitly only for production
  publication
- Release requirement: the component `version` in `idf_component.yml` must be incremented for CI to publish a new registry release

## How To Add A Component To The Registry

1. Replace every placeholder and make sure the component has a valid
   `idf_component.yml`.
2. Increment `version` in `idf_component.yml` before pushing any change that should be published as a new registry release.
3. Configure the repository as a trusted uploader for the Registry namespace.
4. Commit and review the component files and manifest version bump.
5. Manually run `upload_components.yml` with the intended component path and
   namespace, leaving `dry_run` enabled first.
6. Review the dry-run result, then rerun with `dry_run` disabled only when the
   immutable version is ready to publish.

### Example

If your component is named `template`, update the workflow configuration so the action uploads that component from its repository path.

### Required repository setup

- Configure an OIDC trusted uploader for the repository and namespace
- If token authentication is used instead, keep the token in GitHub Secrets and
  never hardcode it in workflow YAML
- Keep the component name and path aligned with the actual component folder and metadata

### Recommended release practice

- Bump `version` in `idf_component.yml` before publishing changes; without a new manifest version, CI will not publish a new registry release
- Treat breaking API changes as a major version bump
- Keep the README and registry metadata in sync before pushing to `main`
