# Authentication Contract

## Component: Authentication

## Contract Type
Behavioral specification for user authentication flow.

## Inputs
| Name | Type | Validation |
|------|------|------------|
| email | string | Valid email format |
| password | string | Minimum 6 characters |

## Outputs
| Name | Type | Description |
|------|------|-------------|
| user | object \| null | Authenticated user object or null |
| session | object \| null | Active session or null |
| error | string \| null | Error message if authentication fails |

## Behaviors
1. User can sign up with email/password
2. User can log in with email/password
3. User can log out (clear session)
4. Session persists across page refreshes
5. Protected routes redirect to login if no session

## Side Effects
- Creates user record in Supabase `auth.users` table
- Sets auth token in browser localStorage
- Triggers React Query cache invalidation on logout

## Constraints
- Email/password authentication only (no OAuth for MVP)
- Must handle auth errors gracefully with user-friendly messages

## Affected Components
- Login page
- Signup page
- Protected routes
- User context provider
