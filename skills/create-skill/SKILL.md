---
name: create-skill
description: 在 ~/.codex/skills 或项目本地 skill 目录下创建可复用的 Codex skills。用于用户想创建、编写、改进、迁移或记录 skill，或询问 SKILL.md 结构、触发设计和 skill 编写最佳实践的场景。
---

# Create Codex Skills

Create or refine skills as small, reusable instruction packages. Prefer concise instructions, clear trigger wording, and minimal supporting files.

## First Pass

Infer what you can from the conversation, then fill the gaps only if they materially affect the result:

1. Purpose: what task or workflow the skill should support.
2. Scope: personal skill or project skill.
3. Trigger phrases: how the user is likely to invoke it.
4. Required domain knowledge: only what the base model would not reliably know.
5. Output constraints: templates, style, review criteria, commands, or file targets.

If the user already gave enough context, create the skill directly instead of asking follow-up questions.

## Location

Prefer these locations:

- Personal: `~/.codex/skills/<skill-name>/`
- Project-local: `<repo>/.codex/skills/<skill-name>/`

If no location is specified, default to the personal path unless the request is clearly repository-specific.

## Minimum Structure

Each skill should live in its own directory and include a `SKILL.md` file.

```text
skill-name/
├── SKILL.md
├── reference.md        # optional
├── examples.md         # optional
└── scripts/            # optional
```

Use extra files only when they reduce clutter in `SKILL.md`.

## SKILL.md Format

Every skill needs YAML frontmatter and a markdown body.

```markdown
---
name: your-skill-name
description: 说明这个 skill 做什么，以及应该在什么时候使用。
---

# Skill Title

## Purpose
[Short statement of intent]

## Workflow
[Actionable instructions for the agent]
```

## Metadata Rules

- `name`: lowercase letters, numbers, and hyphens only.
- Keep `name` short and stable.
- `description`: say both what the skill does and when it should trigger.
- Write the description in third person, not as a promise from the assistant.

Good description pattern:

```yaml
description: 检查代码中的 bug、回归风险和缺失测试。用于用户要求 review、PR 反馈或变更风险分析的场景。
```

## Authoring Rules

- Keep `SKILL.md` short; move bulk reference material out.
- Prefer concrete instructions over broad theory.
- Do not restate generic knowledge the model already has.
- Include trigger phrases when they improve discovery.
- Add examples only if they clarify a fragile workflow or required format.
- If migrating a skill from another tool, adapt paths and tool references to Codex instead of copying them blindly.

## Conversion Checklist

When porting an existing skill:

1. Rewrite storage paths for Codex.
2. Remove tool instructions that do not exist in this environment.
3. Preserve the useful workflow and trigger logic.
4. Tighten long explanations into shorter operational guidance.
5. Verify the result still reads like instructions, not product documentation.

## Default Build Strategy

Unless the user asked for something else:

1. Create the skill directory.
2. Write `SKILL.md`.
3. Add optional support files only if needed.
4. Summarize the trigger wording and intended use cases.

## Common Skill Types

- Review skills: code review, PR triage, test planning.
- Writing skills: commit messages, release notes, changelogs.
- Research skills: structured investigation, comparison, source gathering.
- Workflow skills: repo setup, deployment checklists, incident response.
