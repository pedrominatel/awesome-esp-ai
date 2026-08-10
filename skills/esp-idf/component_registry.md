# ESP Component Registry REST API

Use the public REST API when Registry MCP tools are unavailable or when raw
metadata and download URLs are required. The base URL is:

`https://components.espressif.com/api/`

No authentication is required for public component metadata.

## Search Components

```http
GET /api/components?q={query}&page=1&per_page=10
```

Results include the component name, namespace, description, license, downloads,
and available versions.

Example:

```http
GET /api/components?q=kalman&page=1&per_page=10
```

## Component Details and Examples

```http
GET /api/components/{namespace}/{name}
```

The response includes component metadata and an `examples` array when examples
are published.

Example:

```http
GET /api/components/espressif/esp-dsp
```

Inspect the response's `examples` array for names, descriptions, archives, and
README URLs.

An example README can be retrieved from:

```text
https://components-file.espressif.com/components/{namespace}/{name}/{version}/examples/{example_path}/readme.md
```

## Selection and Installation

- Check supported ESP-IDF versions, targets, license, maintenance status, and API
  fit before selecting a component.
- If multiple candidates remain materially equivalent, present the trade-offs and
  ask the user to choose.
- Add a dependency from the project root:

```sh
idf.py add-dependency "namespace/component^<VERSION>"
```

- The command updates `main/idf_component.yml` by default. Use
  `--component=<NAME>` for a component under `components/`, or `--path=<PATH>`
  for another component directory.
