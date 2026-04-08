# TodoApp — Technical Plan

## Project Intent
Build a simple, production-ready todo application with user authentication. Focus on clean architecture, minimal dependencies, and rapid development.

## Recommended Stack
- **Frontend:** React 18 + Vite
- **Backend/Database/Auth:** Supabase (PostgreSQL + Auth + Realtime)
- **Styling:** Tailwind CSS
- **State Management:** React Query (TanStack Query) + Context API
- **Deployment:** Vercel (frontend) + Supabase (backend)

## Why This Stack?
- **Zero backend boilerplate:** Supabase provides auth, database, and API out of the box
- **Fast development:** Vite + React + Tailwind = rapid UI iteration
- **Free tier friendly:** All services have generous free tiers
- **Simple deployment:** One-click deploys to Vercel
- **Scalable:** Can grow complexity without rewriting architecture

## Core Features
1. User signup/login/logout
2. Create, read, update, delete todos
3. Mark todos as complete/incomplete
4. Filter todos (all/active/completed)
5. Private todos per user (row-level security)

## Architecture Overview
```
src/
├── components/     # UI components (atoms, molecules)
├── features/       # Feature-based modules (auth, todos)
├── hooks/          # Custom React hooks
├── lib/            # Supabase client, utilities
└── pages/          # Route pages
```

## Sprint Goals
- **Sprint 1:** Setup, auth flow, basic todo CRUD
- **Sprint 2:** Filters, polish, deployment

## Success Criteria
- User can sign up, log in, manage their todos
- Clean, responsive UI
- Deployed and accessible online
- Code follows substrate taxonomy