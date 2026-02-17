# Todo App Design

**Date:** 2026-02-17
**Issue:** NOM-12

## Summary

Build a clean, card-style todo list app in the existing Vue 3 + Vite project.

## Architecture

Single-file component approach: all logic and UI lives in `src/App.vue`. No router, no store, no additional dependencies beyond what's already in the project.

**State:**
- `todos: Array<{ id: number, text: string, done: boolean }>` — the list
- `newTodo: string` — bound to the add-input field

**Operations:**
- `addTodo()` — push new item if input non-empty, then clear input; also triggered by Enter key
- `removeTodo(id)` — filter the array by id
- Toggle done state via `v-model` on `todo.done` (checkbox)

## UI Layout

Centered card on a light gray page background.

```
┌──────────────────────────────────────┐
│  ☑ My Todo List           Feb 17     │
│  ────────────────────────────────    │
│  ☐  Buy groceries              🗑    │
│  ☑  ̶C̶a̶l̶l̶ ̶d̶e̶n̶t̶i̶s̶t̶ (faded)        🗑    │
│  ☐  Finish report              🗑    │
│  ────────────────────────────────    │
│  [  Add a new task...      ] [+]     │
└──────────────────────────────────────┘
```

- Header: title on left, today's date on right
- Todo items: custom checkbox, text (strikethrough + faded when done), delete button on the right
- Pressing Enter on input or clicking `+` adds the item
- Empty state: friendly message when no todos exist

## Styling

- Page background: `#f5f5f5` (light gray)
- Card: white, `border-radius: 12px`, soft `box-shadow`
- Accent color: indigo `#6366f1` for checkboxes and Add button
- Done items: `text-decoration: line-through`, `opacity: 0.5`
- Font: system sans-serif stack
- No external CSS framework — pure scoped `<style>` in the SFC

## Decisions

- **Completed item handling:** Strike-through in place (not moved to a separate section)
- **Persistence:** In-memory only (no localStorage)
- **Structure:** Single-file `App.vue` (not decomposed into sub-components)
