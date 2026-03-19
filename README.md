# Awesome ESP AI

> A curated collection of AI-ready skills, templates, and workflows for building ESP-IDF projects and reusable ESP components.

This repository is organized like an Awesome list, but focused on practical assets you can use directly with coding agents and ESP-IDF workflows.

## Contents

- [Why this repo](#why-this-repo)
- [Skills](#skills)
- [Templates and references](#templates-and-references)
- [Official resources](#official-resources)
- [Install from GitHub](#install-from-github)
- [How to use](#how-to-use)
- [Contributing](#contributing)

## Why this repo

- Collect high-signal ESP-IDF guidance that is useful in day-to-day engineering work.
- Package that guidance as reusable skills instead of burying it in ad-hoc prompts.
- Keep component creation, dependency management, flashing, monitoring, and publishing workflows consistent.

## Skills

Keep `AGENTS.md` at the repository root for project-wide agent instructions, and store reusable skill assets under `.agents/skills/`, a hidden root-level directory that keeps them easy for AI tooling to discover without cluttering the main project tree.

### Firmware

- [`.agents/skills/esp-idf`](.agents/skills/esp-idf)  
  Structured guidance for ESP-IDF application development, including environment setup, target detection, build and flash flows, monitor usage, crash analysis, EIM CLI installation, and Component Registry API usage.

- [`.agents/skills/esp-idf-v6-migration`](.agents/skills/esp-idf-v6-migration)  
  Guided migration workflow for upgrading existing ESP-IDF application repositories from 5.x to 6.0, including cumulative references for projects starting before 5.5.

### Components

- [`.agents/skills/esp-idf-components`](.agents/skills/esp-idf-components)  
  Guidance for creating project-local components and reusable component-manager packages, including `idf.py create-component`, dependency selection, `idf_component_register(...)`, packaging structure, CI expectations, and registry publishing decisions.

## Templates and References

- [`.agents/skills/esp-idf-components/component-template`](.agents/skills/esp-idf-components/component-template)  
  Minimal reusable component template with manifest, public header, implementation stub, and publishing notes.

- [`.agents/skills/esp-idf-components/references/component_manager.md`](.agents/skills/esp-idf-components/references/component_manager.md)  
  Quick reference for the ESP-IDF Component Manager CLI, scaffolded layout, manifest expectations, CI guidance, and publish flow.

## Official Resources

- [ESP-IDF Programming Guide](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/index.html)
- [ESP-IDF Component Manager](https://docs.espressif.com/projects/idf-component-manager/en/latest/index.html)
- [Espressif MCP Server](https://mcp.espressif.com/)
- [Espressif Developer Portal](https://developer.espressif.com/)

## Install from GitHub

Clone the repository into a temporary directory and copy the skill you want into `.agents/skills/` inside your target project. Keep `AGENTS.md` at the project root if you want repo-specific agent instructions alongside those reusable skills.

Install all skills:

```sh
mkdir -p .agents/skills /tmp/awesome-esp-ai && \
git clone --depth 1 https://github.com/pedrominatel/awesome-esp-ai.git /tmp/awesome-esp-ai && \
cp -R /tmp/awesome-esp-ai/.agents/skills/* .agents/skills/ && \
rm -rf /tmp/awesome-esp-ai
```

Install only the firmware skill:

```sh
mkdir -p .agents/skills /tmp/awesome-esp-ai && \
git clone --depth 1 https://github.com/pedrominatel/awesome-esp-ai.git /tmp/awesome-esp-ai && \
cp -R /tmp/awesome-esp-ai/.agents/skills/esp-idf .agents/skills/ && \
rm -rf /tmp/awesome-esp-ai
```

Install only the component skill:

```sh
mkdir -p .agents/skills /tmp/awesome-esp-ai && \
git clone --depth 1 https://github.com/pedrominatel/awesome-esp-ai.git /tmp/awesome-esp-ai && \
cp -R /tmp/awesome-esp-ai/.agents/skills/esp-idf-components .agents/skills/ && \
rm -rf /tmp/awesome-esp-ai
```

## How to Use

1. Keep `AGENTS.md` at the repository root for project-specific agent instructions.
2. Place reusable skills under `.agents/skills/` at the repository root.
3. Copy the relevant skill folder into that hidden directory in your target firmware or component repository.
4. Preserve the folder structure so `SKILL.md`, `references/`, and templates stay together.
5. Pick the skill that matches the task:
   - firmware work: [`.agents/skills/esp-idf/`](.agents/skills/esp-idf/)
   - ESP-IDF 5.x to 6.0 migration: [`.agents/skills/esp-idf-v6-migration/`](.agents/skills/esp-idf-v6-migration/)
   - component creation or publishing: [`.agents/skills/esp-idf-components/`](.agents/skills/esp-idf-components/)
6. Open the copied `SKILL.md` in the target repo and use it as the entrypoint.
7. Open linked references only when you need the deeper workflow details.
8. Reuse the component template when you are building a reusable component, not a one-off helper inside `components/`.

## Contributing

- Add new skills only when they represent a reusable workflow, not a one-off project note.
- Keep `SKILL.md` concise and move long command references or examples into `references/`.
- Prefer practical, verifiable guidance over generic theory.
- When adding reusable component guidance, keep it aligned with current ESP-IDF and Component Manager behavior.
