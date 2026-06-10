---
name: karpathy-guidelines
description: 应用 Karpathy 风格的编码约束，保持改动最小且目标明确。用于实现或修改代码时需要聚焦、外科手术式改动的场景。
---

# Karpathy Guidelines

## Purpose
Keep implementation focused on the requested outcome with minimal, explicit, and verifiable changes.

## Workflow
1. Clarify goal, constraints, and success criteria before coding.
2. Choose the simplest approach that satisfies the request.
3. Make surgical edits only in relevant files.
4. Validate behavior with targeted checks or tests.
5. Stop when the requested goal is met.

## Guardrails
- Do not infer hidden requirements without evidence.
- Do not refactor unrelated modules.
- Do not add abstractions unless immediately necessary.
- Do not continue iterating after success unless asked.

## Trigger Phrases
- "use karpathy-guidelines"
- "按 karpathy 规范来做"
- "minimal surgical changes"
- "先想清楚再改代码"
