# TicketFlow – Ticket Management Web App

A fully responsive, accessible ticket management app built with Vue + javascrip and plain CSS.

## Features

- Landing page with hero, features, and call-to-action
- Authentication (Login/Signup) with form validation and mock credentials
- Dashboard with ticket summary cards
- Full ticket CRUD (Create, Read, Update, Delete)
- Toast notifications for feedback
- Consistent Navbar and Footer
- Responsive, accessible, and semantic UI

## Folder Structure

```
/src
  /components      # Navbar, Footer, TicketCard, Form, Toast
  /pages           # Landing, AuthLogin, AuthSignup, Dashboard, Tickets
  /styles          # CSS files for layout and components
  /router          # index.js
  App.vue          # Main app layout
```

## Setup

1. Install dependencies:
   ```bash
   pnpm install
   # or
   npm install
   # or
   yarn install
   ```
2. Start the dev server:
   ```bash
   pnpm dev
   # or
   npm run dev
   # or
   yarn dev
   ```
3. Open [https://example.app/](https://example.app/) in your browser.

## Mock Credentials

- **Login:** `demo@ticketflow.com`
- **Password:** `demo123`

## Accessibility & Responsiveness

- Semantic HTML, focus states, color contrast
- Mobile-first, grid and stacked layouts

---
