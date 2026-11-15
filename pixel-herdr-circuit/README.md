# Pixel Herdr Circuit

A vertical: a pixel-art office for your project, powered by [Herdr](https://github.com/herdrdev/herdr)'s live agent runtime.

## What it is

- **Pixel Office kit** (theme + landing page) provides the canvas office and quest board.
- **Herdr bridge** (in `@blkc-code/agents`) overlays Herdr's per-pane state onto the office: agents appear automatically, with real liveness, auto-room from the foreground working directory, model tokens, and workspace/space context. No `office_status` calls required.
- Quests, task text, and task ownership remain office-owned. Herdr is optional; without it the circuit behaves exactly like the Pixel Office kit.

## Use it

```bash
npx @blkc-code/cli new kit my-office --from pixel-office
npx @blkc-code/cli office mcp   # Herdr is auto-detected via HERDR_ENV / HERDR_SOCKET_PATH
```

Orchestration commands:

```bash
blkc office run              # ordered task list with dependencies
blkc office validate <quest> # run a quest's verify command, record pass/fail + evidence
```

Run your agents inside Herdr (`herdr` then your agent) so `HERDR_ENV=1` is set and Herdr can detect them.