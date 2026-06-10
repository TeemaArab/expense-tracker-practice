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

Single-page React app with no routing and no state management library. The `transactions` array is the only shared state, held in `App.jsx` and passed down as props.

### Components

- **`src/App.jsx`** — root component; owns the `transactions` state and `handleAddTransaction` callback. Composes the three child components.
- **`src/Summary.jsx`** — receives `transactions`, computes `totalIncome`, `totalExpenses`, and `balance` internally, renders the three summary cards.
- **`src/TransactionForm.jsx`** — owns its own form state (description, amount, type, category); calls `onAddTransaction` prop with a new transaction object on submit.
- **`src/TransactionList.jsx`** — receives `transactions`, owns filter state (filterType, filterCategory) internally, renders the filtered table.
- **`src/App.css`** — plain CSS, no modules or utility framework. Class names map directly to JSX elements.
- **`src/index.css`** — global resets/defaults.

The `categories` array is defined locally in each component that needs it (`TransactionForm`, `TransactionList`) rather than passed as a prop.

### Data model

Each transaction object: `{ id, description, amount, type, category, date }`.  
`amount` is a number. `type` is `"income"` or `"expense"`. `category` is one of `["food", "housing", "utilities", "transport", "entertainment", "salary", "other"]`.

### Known intentional issues (course exercises)

- **Messy code:** no deletion UI despite `.delete-btn` CSS existing; "Freelance Work" seed data is typed `"expense"` but categorized `"salary"`.
- **Poor UI:** no styling polish, no feedback on form submit, no empty-state handling.
