# ESP-IDF Component Manager Reference

Use this reference when the task involves reusable components, registry packaging, or the ESP-IDF component-manager CLI workflow.

Official docs:
- https://docs.espressif.com/projects/idf-component-manager/en/latest/index.html
- https://docs.espressif.com/projects/idf-component-manager/en/latest/use/how_to_add_dependency.html
- https://docs.espressif.com/projects/idf-component-manager/en/latest/use/how_to_update_dependencies.html
- https://docs.espressif.com/projects/idf-component-manager/en/latest/publish/tutorial_to_package_and_upload.html
- https://docs.espressif.com/projects/idf-component-manager/en/latest/reference/manifest_file.html

## CLI Workflow

- Create a new reusable component scaffold:
  - `idf.py create-component <component-name>`
- Create a manifest with the ESP-IDF frontend:
  - `idf.py create-manifest`
  - Use `--component=<name>` or `--path=<path>` to select its owner.
- With a current standalone Component Manager installation, the equivalent
  command is:
  - `compote manifest create`
- Add an existing registry component to a project or component manifest:
  - `idf.py add-dependency "namespace/component^version"`
  - From a project root this updates `main` by default. Use
    `--component=<name>` or `--path=<path>` for another component.
- Add a dependency from a non-default registry when needed:
  - `idf.py add-dependency --registry-url https://components-staging.espressif.com "namespace/component^version"`
- Add a dependency from Git when the component is not consumed from the registry:
  - `idf.py add-dependency <component-name> --git https://github.com/<org>/<repo>.git --git-path components/<component-name> --git-ref <ref>`
- Update the resolved dependency set and refresh `dependencies.lock` when constraints changed:
  - `idf.py update-dependencies`
- Build a package archive for validation:
  - `compote component pack --name <name>`
- Validate an authorized Registry upload without publishing:
  - `compote component upload --name <name> --namespace <namespace> --dry-run`

Prefer `idf.py create-component` when starting a new reusable component you own. Prefer `idf.py add-dependency` when consuming an existing component instead of duplicating it locally.

Do not edit `managed_components/` or `dependencies.lock` manually. Reconfigure
or use the Component Manager commands to update generated dependency state.

## Scaffolded Layout

`idf.py create-component <component-name>` creates the minimal local layout:

```text
<component-name>/
├── CMakeLists.txt
├── include/
│   └── <component-name>.h
└── <component-name>.c
```

This minimal layout is enough for local use inside an ESP-IDF project.

For a reusable or publishable component-manager package, add:

```text
<component-name>/
├── CMakeLists.txt
├── idf_component.yml
├── include/
│   └── <component-name>.h
├── LICENSE
├── README.md
└── <component-name>.c
```

Notes:
- `idf_component.yml` is the normal source of package metadata and dependencies.
  Upload tools can supply a version separately, but a manifest is recommended.
- When the version comes from the manifest, `version` is required;
  `description`, `url`, `repository`, and `license` are strongly recommended.
- Either the manifest `license` field or a `LICENSE` or `LICENSE.txt` file must be present for registry uploads.
- Examples placed under `examples/` are auto-discovered by the registry; use the manifest `examples` field only when examples live elsewhere.
- The manifest `files` field controls upload packaging and Git dependency contents. It is optional unless the default file filtering is not sufficient.

## CI Expectations

For simple project-local components under `components/`, CI can stay at the application level. Do not force standalone component packaging or publish automation for one-off internal helpers.

For reusable components:
- Build the component in at least one example or test app against the supported ESP-IDF target set.
- If the component ships examples, make CI build those examples.
- If the component has non-trivial behavior, include at least one example or test app so CI validates integration, not only syntax.
- Keep packaging metadata valid in CI by checking that `idf_component.yml`, `README.md`, and licensing are present and consistent before publishing.
- Treat registry upload automation as optional until the component is actually intended to be published.

For publishable components:
- Prefer GitHub Actions for automated uploads when the source is hosted on GitHub.
- Prefer OIDC trusted uploaders over long-lived API tokens when supported.
- Default reusable publication workflows to dry-run validation and require an
  explicit choice for production publication.
- Use staging uploads first when validating packaging, permissions, or namespace setup.
- Increment the `version` field in `idf_component.yml` before pushing a releasable change; CI can only publish a new registry release when the manifest version is new.
- Publish to production only after the component metadata, version, and examples are ready.
- Keep secrets such as registry tokens out of workflow YAML and store them in CI secrets.

## Registry Search API

Prefer ESP Component Registry MCP tools when available. Otherwise, use the public
API at `https://components.espressif.com/api/`.

- Search:
  `GET /api/components?q={query}&page=1&per_page=10`
- Component details:
  `GET /api/components/{namespace}/{name}`
- Example README:
  `https://components-file.espressif.com/components/{namespace}/{name}/{version}/examples/{example_path}/readme.md`

Compare ESP-IDF and target compatibility, maintenance, license, and API fit
before selecting a component. Ask the user only when the remaining choice is
material.

## When Not to Use the Template

Do not default to the reusable component-manager scaffold when:
- the code is tightly coupled to a single application
- the component will only live under `components/` in one project
- there is no realistic reuse, versioning, or publication need

In those cases, use `idf.py create-component <component-name>` and keep the
resulting local structure as small as the project needs.
