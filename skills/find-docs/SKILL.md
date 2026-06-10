---
name: find-docs
description: >-
  检索任意开发技术的最新文档、API 参考和代码示例。当用户询问特定库、框架、
  SDK、CLI 工具或云服务时使用，包括 React、Next.js、Prisma、Express、
  Tailwind、Django、Spring Boot 等常见技术；模型训练数据可能无法反映最近的
  API 变化或版本更新。

  以下场景始终使用：API 语法问题、配置选项、版本迁移问题、提到库名的“如何做”
  问题、涉及特定库行为的调试、安装配置说明以及 CLI 工具用法。

  即使看起来知道答案也要使用；不要依赖训练数据回答 API 细节、函数签名或配置项，
  因为这些内容经常过时。始终用当前文档验证。查询库文档和 API 细节时，优先使用
  此技能而不是普通网页搜索。
---

# Documentation Lookup

Retrieve current documentation and code examples for any library using the Context7 CLI.

Make sure the CLI is up to date before running commands:

```bash
npm install -g ctx7@latest
```

Or run directly without installing:

```bash
npx ctx7@latest <command>
```

## Workflow

Two-step process: resolve the library name to an ID, then query docs with that ID.

```bash
# Step 1: Resolve library ID
ctx7 library <name> <query>

# Step 2: Query documentation
ctx7 docs <libraryId> <query>
```

You MUST call `ctx7 library` first to obtain a valid library ID UNLESS the user explicitly provides a library ID in the format `/org/project` or `/org/project/version`.

IMPORTANT: Do not run these commands more than 3 times per question. If you cannot find what you need after 3 attempts, use the best result you have.

## Step 1: Resolve a Library

Resolves a package/product name to a Context7-compatible library ID and returns matching libraries.

```bash
ctx7 library react "How to clean up useEffect with async operations"
ctx7 library nextjs "How to set up app router with middleware"
ctx7 library prisma "How to define one-to-many relations with cascade delete"
```

Always pass a `query` argument — it is required and directly affects result ranking. Use the user's intent to form the query, which helps disambiguate when multiple libraries share a similar name. Do not include any sensitive or confidential information such as API keys, passwords, credentials, personal data, or proprietary code in your query.

### Result fields

Each result includes:

- **Library ID** — Context7-compatible identifier (format: `/org/project`)
- **Name** — Library or package name
- **Description** — Short summary
- **Code Snippets** — Number of available code examples
- **Source Reputation** — Authority indicator (High, Medium, Low, or Unknown)
- **Benchmark Score** — Quality indicator (100 is the highest score)
- **Versions** — List of versions if available. Use one of those versions if the user provides a version in their query. The format is `/org/project/version`.

### Selection process

1. Analyze the query to understand what library/package the user is looking for
2. Select the most relevant match based on:
   - Name similarity to the query (exact matches prioritized)
   - Description relevance to the query's intent
   - Documentation coverage (prioritize libraries with higher Code Snippet counts)
   - Source reputation (consider libraries with High or Medium reputation more authoritative)
   - Benchmark score (higher is better, 100 is the maximum)
3. If multiple good matches exist, acknowledge this but proceed with the most relevant one
4. If no good matches exist, clearly state this and suggest query refinements
5. For ambiguous queries, request clarification before proceeding with a best-guess match

### Version-specific IDs

If the user mentions a specific version, use a version-specific library ID:

```bash
# General (latest indexed)
ctx7 docs /vercel/next.js "How to set up app router"

# Version-specific
ctx7 docs /vercel/next.js/v14.3.0-canary.87 "How to set up app router"
```

The available versions are listed in the `ctx7 library` output. Use the closest match to what the user specified.

## Step 2: Query Documentation

Retrieves up-to-date documentation and code examples for the resolved library.

```bash
ctx7 docs /facebook/react "How to clean up useEffect with async operations"
ctx7 docs /vercel/next.js "How to add authentication middleware to app router"
ctx7 docs /prisma/prisma "How to define one-to-many relations with cascade delete"
```

### Writing good queries

The query directly affects the quality of results. Be specific and include relevant details. Do not include any sensitive or confidential information such as API keys, passwords, credentials, personal data, or proprietary code in your query.

| Quality | Example |
|---------|---------|
| Good | `"How to set up authentication with JWT in Express.js"` |
| Good | `"React useEffect cleanup function with async operations"` |
| Bad | `"auth"` |
| Bad | `"hooks"` |

Use the user's full question as the query when possible, vague one-word queries return generic results.

The output contains two types of content: **code snippets** (titled, with language-tagged blocks) and **info snippets** (prose explanations with breadcrumb context).

## Authentication

Works without authentication. For higher rate limits:

```bash
# Option A: environment variable
export CONTEXT7_API_KEY=your_key

# Option B: OAuth login
ctx7 login
```

## Error Handling

If a command fails with a quota error ("Monthly quota reached" or "quota exceeded"):
1. Inform the user their Context7 quota is exhausted
2. Suggest they authenticate for higher limits: `ctx7 login`
3. If they cannot or choose not to authenticate, answer from training knowledge and clearly note it may be outdated

Do not silently fall back to training data — always tell the user why Context7 was not used.

## Common Mistakes

- Library IDs require a `/` prefix — `/facebook/react` not `facebook/react`
- Always run `ctx7 library` first — `ctx7 docs react "hooks"` will fail without a valid ID
- Use descriptive queries, not single words — `"React useEffect cleanup function"` not `"hooks"`
- Do not include sensitive information (API keys, passwords, credentials) in queries
