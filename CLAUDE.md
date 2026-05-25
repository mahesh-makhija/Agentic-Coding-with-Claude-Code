# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

Companion code for the Packt book *Agentic Coding with Claude Code* (Eden Marco). This is **not a single application** — each `ChapterNN/` directory is a self-contained example that demonstrates a specific Claude Code feature (slash commands, hooks, subagents, output styles, skills, MCP servers, deep agents). When working in this repo, identify the chapter you are in first; commands, dependencies, and conventions vary per chapter.

## Two Project Templates

Most subprojects fall into one of two templates. Use the matching commands:

### Python / FastMCP (Chapter01, Chapter04/context-engineering-mcp)
Managed with [`uv`](https://docs.astral.sh/uv/), Python ≥ 3.11, dependency `fastmcp`.
```bash
uv sync                              # install deps
uv run main.py                       # run entry point
uv run verbose_mcp_server.py         # run the demo MCP server
```
These chapters demonstrate context-window optimization with `.mcp.json` variants — launch Claude Code with `claude --mcp-config .mcp.json.<variant>` to load only a subset of servers (see `Chapter01/README.md`).

### Next.js HookHub (Chapter02/hookhub/hookhub, Chapter10/vigilant-feistel/hookhub, Chapter10/zealous-jemison/hookhub)
Next.js 15 + App Router + React 19 + TypeScript + Tailwind 4. Each HookHub copy is independent (no shared `node_modules`) and has its own nested `CLAUDE.md` with project-specific notes — read that file before editing.
```bash
npm install                          # first time per copy
npm run dev                          # http://localhost:3000
npm run build
npm run lint
```
No test framework is configured. The nested `CLAUDE.md` directs UI testing through the Playwright MCP, with screenshots saved to `memory/screenshots/`.

## Chapter → Feature Map

When the user references a chapter, expect to find its `.claude/` config (or equivalent demo artifacts) here:

| Chapter | What it demonstrates | Key paths |
|---|---|---|
| 01 | MCP server token cost; minimal `--mcp-config` files | `verbose_mcp_server.py`, `README.md` |
| 02 | HookHub Next.js project + subagents (`hookhub2/.claude/agents/`) | `hookhub/hookhub/`, `hookhub2/.claude/agents/*.md` |
| 03 | Custom slash commands + Stop hooks | `custom commands/.claude/commands/`, `hooks-notification/.claude/settings.json` |
| 04 | Context engineering via MCP (mirrors Ch01 structure) | `context-engineering-mcp/`, `Chapter04/CLAUDE.md` |
| 07 | Subagent definitions | `.claude/agents/code-comedy-carl.md`, `mermaid-diagram-generator.md` |
| 08 | Output styles + statusline script | `.claude/output-styles/`, `statusline.py` |
| 09 | Skills (Agent Skills) | `.claude/skills/git-pushing/SKILL.MD` + `scripts/` |
| 10 | Deep agents — two HookHub variants (`vigilant-feistel`, `zealous-jemison`) | `Chapter10/*/hookhub/` |

Chapters 05 and 06 are referenced in `README.md` but have no code directory.

## Nested CLAUDE.md / Settings Precedence

There are at least two more `CLAUDE.md` files below the root, plus chapter-local `.claude/settings.json` and `.claude/agents/` directories:

- `Chapter04/CLAUDE.md` — **rule: when discussing LangGraph, always use the context7 MCP.**
- `Chapter02/hookhub/hookhub/CLAUDE.md` and the two `Chapter10/*/hookhub/CLAUDE.md` files — HookHub Next.js conventions (Tailwind 4, `@/*` → `./src/*`, Playwright MCP for UI tests).
- `Chapter03/hooks-notification/.claude/settings.json` — registers a Stop hook that runs `uv run /Users/edenmarco/.../play_sound.py`. The path is the original author's machine; if you trigger this chapter expect the hook to fail and either update or remove the path before running.

When working inside a chapter, defer to that chapter's `CLAUDE.md` / `.claude/` over anything stated here.

## Sample Demo Artifacts (do not treat as production code)

Several files are intentional teaching examples and should not be "fixed" or stylistically normalized:

- `Chapter01/verbose_mcp_server.py` and the Chapter04 copy: tool docstrings are deliberately bloated to show how verbose MCP descriptions consume context tokens.
- `Chapter07/main.py` and `Chapter07/README.md`: scaffolding/marketing text used as demo inputs.
- `Chapter03/custom commands/.claude/commands/dad-joke.md`: a deliberately trivial slash-command example.
