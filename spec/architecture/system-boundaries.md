# System Boundaries

## Architecture Decision
Use Supabase as Backend-as-a-Service (BaaS) to eliminate backend boilerplate while maintaining production-ready security and scalability.

## System Components

### Client (Frontend)
- **Technology:** React 18 + Vite
- **Responsibility:** UI rendering, user interaction, local state management
- **State Management:** React Query (server state) + Context API (global UI state)
- **Styling:** Tailwind CSS

### Server (Backend)
- **Technology:** Supabase (PostgreSQL + Auth + Realtime)
- **Responsibility:** Data persistence, authentication, authorization, API endpoints
- **Security Model:** Row Level Security (RLS) for data isolation

## Data Flow
1. User interacts with React UI
2. React Query triggers Supabase client call
3. Supabase validates auth token and RLS policies
4. Database operation executes
5. Response flows back through React Query cache
6. UI re-renders with updated data

## Constraints
- Must use Row Level Security for data isolation
- Must support email/password authentication
- Frontend must be deployable to Vercel

## Affected Domains
- Authentication flow
- Todo CRUD operations
- Database schema design
