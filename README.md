# Awesome ESP AI

> A curated collection of AI-ready skills, templates, and workflows for building ESP-IDF projects and reusable ESP components.

This repository is organized like an Awesome list, but focused on practical assets you can use directly with coding agents and ESP-IDF workflows.

## Contents

- [Why this repo](#why-this-repo)
- [Skills](#skills)
- [Templates and references](#templates-and-references)
- [Official resources](#official-resources)
- [Install with Skills CLI](#install-with-skills-cli)
- [How to use](#how-to-use)
- [Contributing](#contributing)

## Why this repo

- Collect high-signal ESP-IDF guidance that is useful in day-to-day engineering work.
- Package that guidance as reusable skills instead of burying it in ad-hoc prompts.
- Keep component creation, dependency management, flashing, monitoring, and publishing workflows consistent.

## Skills

Keep `AGENTS.md` at the repository root for project-wide agent instructions, and
store the reusable source assets under `skills/`. Install the selected skills in
the location expected by the target coding agent.

### Firmware

- [`skills/esp-idf`](skills/esp-idf)
  Structured guidance for ESP-IDF application development, including environment setup, target detection, build and flash flows, monitor usage, crash analysis, EIM CLI installation, and Component Registry API usage.

- [`skills/esp-idf-v6-migration`](skills/esp-idf-v6-migration)
  Guided migration workflow for upgrading existing ESP-IDF application repositories from 5.x to 6.0, including cumulative references for projects starting before 5.5.

### Components

- [`skills/esp-idf-components`](skills/esp-idf-components)
  Guidance for creating project-local components and reusable component-manager packages, including `idf.py create-component`, dependency selection, `idf_component_register(...)`, packaging structure, CI expectations, and registry publishing decisions.

## Templates and References

- [`skills/esp-idf-components/component-template`](skills/esp-idf-components/component-template)
  Minimal reusable component template with manifest, public header, implementation stub, and publishing notes.

- [`skills/esp-idf-components/references/component_manager.md`](skills/esp-idf-components/references/component_manager.md)
  Quick reference for the ESP-IDF Component Manager CLI, scaffolded layout, manifest expectations, CI guidance, and publish flow.

## Official Resources

- [ESP-IDF Programming Guide](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/index.html)
- [ESP-IDF Component Manager](https://docs.espressif.com/projects/idf-component-manager/en/latest/index.html)
- [Espressif MCP Server](https://mcp.espressif.com/)
- [Espressif Developer Portal](https://developer.espressif.com/)

## Install with Skills CLI

Use the [Skills CLI](https://skills.sh/) to install a skill from this repository.
The CLI detects supported coding agents and installs the selected skill in the
appropriate location.

Install the ESP-IDF firmware skill:

```sh
npx skills add pedrominatel/awesome-esp-ai@esp-idf
```

Install the component development skill:

```sh
npx skills add pedrominatel/awesome-esp-ai@esp-idf-components
```

Install the ESP-IDF 6.0 migration skill:

```sh
npx skills add pedrominatel/awesome-esp-ai@esp-idf-v6-migration
```

To browse and select skills from this repository interactively:

```sh
npx skills add pedrominatel/awesome-esp-ai
```

Add `-g` for a user-level installation or `-y` to accept prompts
non-interactively:

```sh
npx skills add pedrominatel/awesome-esp-ai@esp-idf -g -y
```

The Skills CLI installs skill directories, but it does not install the optional
repository-wide `AGENTS.md`. Copy that file separately to the target repository
root when desired.

## How to Use

1. Install the relevant skill with `npx skills add`.
2. Keep `AGENTS.md` at the target repository root when project-wide ESP-IDF
   instructions are wanted.
3. Pick the skill that matches the task:
   - firmware work: [`skills/esp-idf/`](skills/esp-idf/)
   - ESP-IDF 5.x to 6.0 migration: [`skills/esp-idf-v6-migration/`](skills/esp-idf-v6-migration/)
   - component creation or publishing: [`skills/esp-idf-components/`](skills/esp-idf-components/)
4. Open linked references only when deeper workflow details are needed.
5. Reuse the component template for reusable components rather than one-off
   helpers inside an application's `components/` directory.

## Contributing

- Add new skills only when they represent a reusable workflow, not a one-off project note.
- Keep `SKILL.md` concise and move long command references or examples into `references/`.
- Prefer practical, verifiable guidance over generic theory.
- When adding reusable component guidance, keep it aligned with current ESP-IDF and Component Manager behavior.
