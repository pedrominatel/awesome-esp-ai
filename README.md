# Awesome ESP AI

> A curated collection of AI-ready skills, templates, and workflows for building ESP-IDF projects and reusable ESP components.

This repository is organized like an Awesome list, but focused on practical assets you can use directly with coding agents and ESP-IDF workflows.

## Contents

- [Why this repo](#why-this-repo)
- [Skills](#skills)
- [Templates and references](#templates-and-references)
- [Official resources](#official-resources)
- [How to use](#how-to-use)
- [Contributing](#contributing)

## Why this repo

- Collect high-signal ESP-IDF guidance that is useful in day-to-day engineering work.
- Package that guidance as reusable skills instead of burying it in ad-hoc prompts.
- Keep component creation, dependency management, flashing, monitoring, and publishing workflows consistent.

## Skills

### Firmware

- [`skills/esp-idf`](skills/esp-idf)  
  Structured guidance for ESP-IDF application development, including environment setup, target detection, build and flash flows, monitor usage, crash analysis, EIM CLI installation, and Component Registry API usage.

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

## How to Use

1. Copy the relevant skill folder into your target firmware or component repository.
2. Preserve the folder structure so `SKILL.md`, `references/`, and templates stay together.
3. Pick the skill that matches the task:
   - firmware work: [`skills/esp-idf/`](skills/esp-idf/)
   - component creation or publishing: [`skills/esp-idf-components/`](skills/esp-idf-components/)
4. Open the copied `SKILL.md` in the target repo and use it as the entrypoint.
5. Open linked references only when you need the deeper workflow details.
6. Reuse the component template when you are building a reusable component, not a one-off helper inside `components/`.

## Contributing

- Add new skills only when they represent a reusable workflow, not a one-off project note.
- Keep `SKILL.md` concise and move long command references or examples into `references/`.
- Prefer practical, verifiable guidance over generic theory.
- When adding reusable component guidance, keep it aligned with current ESP-IDF and Component Manager behavior.
