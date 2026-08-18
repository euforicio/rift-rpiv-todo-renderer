# RPIV Todo

BB’s composer already draws a live to-do panel (`N/M complete`) for Cursor `TodoWrite` and Claude `TaskCreate` / `TaskUpdate`. Pi’s [`@juicesharp/rpiv-todo`](https://www.npmjs.com/package/@juicesharp/rpiv-todo) tool is named `todo` and is not on that list, so those calls show up as generic argument dumps.

This plugin reconstructs the rpiv-todo list from completed `todo` tool-call arguments on the thread timeline and renders it with BB’s own composer to-do chrome: ListTodo header, `N/M complete`, square / check rows, in-progress first.

## Install

```sh
bb plugin install git:https://github.com/ZhenHuangLab/bb-rpiv-todo-renderer.git
```

Or from a local checkout:

```sh
npm install
bb plugin install .
```

Then refresh the BB app. Disable with `bb plugin disable bb-rpiv-todo-renderer`.

## Settings

**Expand list when opening a thread** (default on) — Settings → RPIV Todo, or:

```sh
bb plugin config bb-rpiv-todo-renderer set expandOnEnter false
bb plugin reload bb-rpiv-todo-renderer
```

Off keeps only the `N/M complete` header until you expand it. Switching threads reapplies the setting; a manual toggle on the current thread is left alone.

The agent still needs `@juicesharp/rpiv-todo` in Pi (`npm:@juicesharp/rpiv-todo` in `~/.pi/agent/settings.json`). This plugin only renders.

## What you see

- A card above the composer on an existing thread, matching BB’s native to-do panel.
- In-progress rows keep the task subject, show `activeForm` underneath, and use a spinner. The header also appends the current `activeForm` (`2/5 complete · inspecting …`).
- Deleted tasks stay hidden. `clear` empties the card.
- The list refreshes while the turn is running and again when the thread goes idle.

## Limits

- BB does not persist rpiv-todo’s `details.tasks` snapshot. The plugin replays `create` / `update` / `delete` / `clear` arguments in sequence. A failed tool result (`Error: …`) is skipped.
- Transcript tool cards for `todo` stay generic. Plugin UI cannot join BB’s first-party `pendingTodos` allowlist.
- Very long threads are scanned in pages (up to 20 000 events).

## License

MIT
