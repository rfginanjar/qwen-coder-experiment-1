# Todo Contract

## Component: Todos

## Contract Type
Behavioral specification for todo CRUD operations.

## Data Model

### Table: `todos`
| Field | Type | Constraints |
|-------|------|-------------|
| id | uuid | Primary key |
| user_id | uuid | Foreign key → auth.users.id, not nullable |
| title | string | Not nullable |
| completed | boolean | Default: false |
| created_at | timestamp | Default: now() |
| updated_at | timestamp | Default: now() |

## Operations

### Create
- **Input:** `{ title: string }`
- **Output:** `{ todo: Todo object }`
- **Security:** RLS policy - user_id must match auth.uid()

### Read
- **Input:** `{ filters?: { status?: 'all' | 'active' | 'completed' } }`
- **Output:** `{ todos: Todo[] }`
- **Security:** RLS policy - only return todos where user_id = auth.uid()

### Update
- **Input:** `{ id: uuid, changes: Partial<Todo> }`
- **Output:** `{ todo: Todo object }`
- **Security:** RLS policy - user_id must match auth.uid()

### Delete
- **Input:** `{ id: uuid }`
- **Output:** `{ success: boolean }`
- **Security:** RLS policy - user_id must match auth.uid()

## Behaviors
1. User can create a new todo with a title
2. User can view all their todos
3. User can mark todo as complete/incomplete
4. User can edit todo title
5. User can delete a todo
6. User can filter todos by status (all/active/completed)
7. Todos are private to each user (enforced by Row Level Security)

## Constraints
- Every todo must belong to a user (user_id not nullable)
- RLS policies must prevent cross-user data access
- Support filtering by completion status

## Affected Components
- Todo list component
- Todo item component
- Todo form component
- Filter controls component

## Dependencies
- Authentication contract (auth-001) - requires authenticated user
