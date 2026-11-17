---
name: astro-run
description: Runs the whole Astro stack locally in one tmux session - Docker infrastructure, every backend service in its own window and the website frontend in mock mode. Use when the user types /astro-run or asks to start, boot or spin up Astro locally, or to restart the local stack.
---

# /astro-run: run Astro locally

Starts everything in a detached tmux session named `astro`, one window per process, so the user can attach and watch any of them. A window must survive Ctrl-C: the user stops and restarts single services by hand, so a stopped service has to leave an open shell in its window, never close it.

## Where things live

- Backend: the `astro` repo (or one of its worktrees, use the one the user is working in). Its `justfile` has the recipes, secrets come from the Infisical `dev` environment through `just`.
- Frontend: the sibling `astro-bot.space` repo. It is started with the `dev:mock` pnpm script, which mocks the Discord guilds so no real bot login is needed.
- Docker infrastructure (Mongo, Redis and their UIs) is a compose project with a fixed name, so it is shared by all worktrees: bringing it up twice is harmless.

## Steps

1. Check `tmux`, `just`, `infisical`, `pnpm` and Docker are available, and that Infisical is logged in. If not, tell the user what is missing and stop.
2. If a tmux session named `astro` already exists, don't create a second one: report it and ask whether to attach to it or kill and recreate it.
3. Start the infrastructure with `just docker-up` from the backend repo and wait until it returns.
4. Create the session detached with `tmux new-session -d -s astro -n bot -c <backend repo>`, giving no command so the window starts the user's shell.
5. Add the other windows the same way with `tmux new-window -t astro -n <name> -c <dir>`: `api-central`, `support-bot`, `entitlements-expiration-job` in the backend repo, and `web` in the `astro-bot.space` repo.
6. Type the command into each window with `tmux send-keys -t astro:<window> '<command>' Enter`: `just run-<service>` for the backend ones, `pnpm dev:mock` for `web`. Never pass the command to `new-session` or `new-window`: tmux closes the window when its command exits, so Ctrl-C (or a crash) would destroy the window and its output.
7. Don't wait for the processes to be ready (Gradle startup is slow). Report the window names and tell the user to run `tmux attach -t astro`.

## Notes

- Never build or compile the project to check it (backend `CLAUDE.md` rule): only start it with the recipes above.
- A crashed or Ctrl-C'd service leaves its shell open at the prompt, so the output stays readable and the user can rerun the command with the up arrow. The `entitlements-expiration-job` is a one-shot job and is expected to exit. Check windows with `tmux list-windows -t astro` and read logs through `tmux capture-pane -p -S -200 -t astro:<window>`; to restart a service, send its command again with `send-keys`.
- To stop everything: `tmux kill-session -t astro`, then `just docker-down` only if the user asks to stop the infrastructure too.
- Run services individually (not the whole stack) with the `run-*` recipes in the backend `justfile`.
