# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm install       # install dependencies (requires strict-ssl disabled — see note below)
npm run dev       # start dev server at http://localhost:5173
npm run build     # production build
npm run lint      # ESLint
npm run preview   # preview production build
```

> **SSL note:** This machine sits behind a proxy that intercepts HTTPS. Run `npm config set strict-ssl false` once before `npm install` if packages fail to download with `UNABLE_TO_VERIFY_LEAF_SIGNATURE`.

## Architecture

This is a single-page React app with no routing, no state management library, and no component splitting. All logic lives in one file:

- **`src/App.jsx`** — the entire app: state, derived values (totals, balance, filtered list), form handling, and JSX. Intentionally monolithic as a course starting point.
- **`src/App.css`** — plain CSS, no modules or utility framework. Class names map directly to elements in `App.jsx`.
- **`src/index.css`** — global resets/defaults.

### Data model

Each transaction object: `{ id, description, amount, type, category, date }`.  
`type` is `"income"` or `"expense"`. `category` is one of `["food", "housing", "utilities", "transport", "entertainment", "salary", "other"]`.

### Known intentional issues (course exercises)

- **Bug:** `amount` is stored as a string. The `.reduce` for totals does string concatenation instead of numeric addition — balance and totals are wrong.
- **Messy code:** everything in one component; no deletion UI despite `.delete-btn` CSS existing; "Freelance Work" seed data is typed `"expense"` but categorized `"salary"`.
- **Poor UI:** no styling polish, no feedback on form submit, no empty-state handling.
