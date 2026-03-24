# agent-skills Project Conventions

This is a documentation-only repo containing SKILL.md files for OKX trading agents.
Always respond in Chinese (中文) unless explicitly asked otherwise.

## Skill Review Checklist

When reviewing skill MRs, apply ALL of the following criteria.

### 1. Structure (High Priority)

- **Frontmatter has `---` delimiters** — YAML metadata must be wrapped in `---` markers for the skill engine to parse `name`, `description`, and other fields correctly.
- **`description` is concise (~100 words, max 150)** — The description is always-in-context metadata used for agent routing. It should be "pushy" to combat under-triggering — actively list usage scenarios including when users don't explicitly name the skill. All "when to use" info goes in description, NOT in the body.
- **File size under 500 lines** — If over 500 lines, adopt layered architecture:
  - `SKILL.md` (~300 lines): core workflow, always loaded
  - `references/cli-commands.md`: detailed CLI parameter tables
  - `references/edge-cases.md`: boundary conditions
  - `references/examples.md`: input/output examples
- **Standard directory structure** — Skills should follow:
  - `SKILL.md` (required) — core instructions
  - `scripts/` — executable code for deterministic/repetitive tasks
  - `references/` — docs loaded into context as needed
  - `assets/` — templates, icons, fonts
- **Domain variant pattern** — Multi-scenario skills should split references per variant, so Claude only loads the relevant one:
  ```
  skill-name/
  ├── SKILL.md (workflow + selection)
  └── references/
      ├── variant-a.md
      └── variant-b.md
  ```

### 2. Content Accuracy (High Priority)

- **No phantom tool references** — Every MCP tool or CLI command listed must actually exist and be registered. Cross-check against real tool registry / CLI help output.
- **Cross-skill references have fallbacks** — If referencing other skills (e.g. `okx-cex-portfolio`), note them as optional dependencies with fallback behavior when not installed.

### 3. Interaction Design (Medium Priority)

- **Confirmation logic is proportional** — WRITE operations need confirmation, but if the user's instruction contains complete parameters and clear intent, summary and execution can merge into one step. Don't force redundant confirmation for explicit commands.
- **Language follows the user** — Chinese question → Chinese response, English → English. Don't hardcode templates in one language.

### 4. Writing Style (Low Priority)

- **Explain why, not just what** — Prefer explaining reasoning over bare directives. If you find yourself writing ALWAYS/NEVER in caps, that's a yellow flag — try reframing with the reasoning so the model understands and generalizes.
- **No excessive repetition** — Each concept explained in detail once, briefly referenced elsewhere.
- **ALWAYS/NEVER sparingly** — Reserve all-caps for genuine safety constraints (live trading guards). For preferences, use normal casing with rationale.
- **Use theory of mind** — Write skills that are general, not overfitted to specific examples. Explain the reasoning so the model can generalize.
