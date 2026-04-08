# Task: Todo CRUD Implementation

**Task ID:** task-todo-crud-001  
**Sprint:** sprint1  
**Priority:** 4 (Critical Path)  
**Status:** Blocked by task-auth-001, task-db-001  

## Objective
Implement complete CRUD operations for todos with React Query, including create, read, update, delete, and filtering capabilities.

## Inputs (from Planner Handoffs)
- **Todo Contract:** handoff-todo-001.json
  - Operations: create, read, update, delete
  - Behaviors: create, view all, toggle complete, edit title, delete, filter by status
  - Security: RLS enforced (user_id = auth.uid())
- **Auth Context:** from task-auth-001 (provides current user)
- **Database Schema:** from task-db-001 (todos table ready)

## Work Steps

### 1. Create Custom Hooks for Todo Operations
File: `src/hooks/useTodos.js`

Implement:
- `useTodos(filter)` - Fetch todos with optional filter
- `useCreateTodo()` - Create new todo mutation
- `useUpdateTodo()` - Update todo mutation
- `useDeleteTodo()` - Delete todo mutation

Use React Query's:
- `useQuery` for fetching
- `useMutation` for modifications
- `useQueryClient` for cache invalidation
- Optimistic updates for instant UI feedback

### 2. Create TodoForm Component
File: `src/components/todos/TodoForm.jsx`

Features:
- Text input for todo title
- Submit button
- Form validation (title required, max length)
- Loading state during submission
- Error handling
- Support for both create and edit modes

### 3. Create TodoItem Component
File: `src/components/todos/TodoItem.jsx`

Features:
- Display todo title
- Checkbox to toggle completed status
- Edit button (switches to edit mode)
- Delete button with confirmation
- Inline editing capability
- Loading states for async operations

### 4. Create FilterControls Component
File: `src/components/todos/FilterControls.jsx`

Features:
- Three filter buttons: All, Active, Completed
- Visual indication of active filter
- Updates todo list when filter changes

### 5. Create TodoList Component
File: `src/components/todos/TodoList.jsx`

Features:
- Display list of TodoItem components
- Handle empty state (no todos)
- Handle loading state
- Handle error state
- Integrate with FilterControls
- Use useTodos hook for data

### 6. Integrate into Main App
File: `src/pages/HomePage.jsx` or `src/App.jsx`

- Wrap todo features in ProtectedRoute
- Display user's todo list
- Show logout button
- Combine TodoForm, TodoList, FilterControls

## Outputs
- [ ] useTodos hook implemented with React Query
- [ ] useCreateTodo mutation with optimistic update
- [ ] useUpdateTodo mutation with optimistic update
- [ ] useDeleteTodo mutation with optimistic update
- [ ] TodoForm component (create + edit modes)
- [ ] TodoItem component with all interactions
- [ ] FilterControls component
- [ ] TodoList component with states
- [ ] HomePage integrating all components

## Acceptance Criteria
1. **Create:** User can add new todo with title → appears instantly in list
2. **Read:** User sees only their own todos on page load
3. **Update (toggle):** Clicking checkbox toggles completed status → updates instantly
4. **Update (edit):** User can edit todo title → saves on blur or enter
5. **Delete:** User can delete todo → removed from list instantly
6. **Filter:** Filtering by All/Active/Completed works correctly
7. **Loading:** Shows loading spinner while fetching
8. **Error:** Shows error message if operation fails
9. **Optimistic:** UI updates before server confirms (rolls back on error)
10. **Security:** Cannot see or modify other users' todos (RLS enforced)

## Constraints
- Must use React Query for all data operations
- Must implement optimistic updates for create, update, delete
- Must handle loading, error, and empty states
- Must work only for authenticated users
- Must respect RLS policies (backend security)
- No direct Supabase calls in components (use hooks)

## Dependencies
- task-auth-001 (AuthContext must provide user)
- task-db-001 (todos table must exist with RLS)

## Blocks
- None (this completes sprint1 MVP)

## Technical Notes

### React Query Pattern
```javascript
// Fetch todos
const { data, isLoading, error } = useTodos('all');

// Create todo
const createTodo = useCreateTodo();
createTodo.mutate({ title: 'New todo' });

// Update todo
const updateTodo = useUpdateTodo();
updateTodo.mutate({ id: todoId, changes: { completed: true } });

// Delete todo
const deleteTodo = useDeleteTodo();
deleteTodo.mutate(todoId);
```

### Optimistic Update Pattern
```javascript
useMutation({
  mutationFn: createTodoApi,
  onMutate: async (newTodo) => {
    // Cancel outgoing refetches
    await queryClient.cancelQueries(['todos']);
    
    // Snapshot previous value
    const previousTodos = queryClient.getQueryData(['todos']);
    
    // Optimistically update
    queryClient.setQueryData(['todos'], (old) => [...old, newTodo]);
    
    return { previousTodos };
  },
  onError: (err, newTodo, context) => {
    // Rollback on error
    queryClient.setQueryData(['todos'], context.previousTodos);
  },
  onSettled: () => {
    // Always refetch after mutation
    queryClient.invalidateQueries(['todos']);
  }
});
```

### Supabase Query Pattern
```javascript
// Fetch todos for current user
const fetchTodos = async (filter) => {
  let query = supabase
    .from('todos')
    .select('*')
    .eq('user_id', user.id)
    .order('created_at', { ascending: false });
  
  if (filter === 'active') {
    query = query.eq('completed', false);
  } else if (filter === 'completed') {
    query = query.eq('completed', true);
  }
  
  const { data, error } = await query;
  if (error) throw error;
  return data;
};
```

## Testing Checklist

### Manual Testing
- [ ] Create todo with valid title → appears in list
- [ ] Create todo with empty title → validation error
- [ ] Toggle todo completion → checkbox updates, persists
- [ ] Edit todo title → saves correctly
- [ ] Delete todo → removed from list
- [ ] Filter by Active → shows only incomplete
- [ ] Filter by Completed → shows only complete
- [ ] Filter by All → shows everything
- [ ] Refresh page → todos persist
- [ ] Network error → shows error message, rolls back optimistic update
- [ ] Two browser tabs → changes sync (if using Supabase realtime)

### Edge Cases
- [ ] Very long todo title (test max length)
- [ ] Special characters in title
- [ ] Rapid successive edits
- [ ] Delete while offline (handle gracefully)
- [ ] Multiple users (verify RLS isolation)

## Performance Considerations
- React Query caches results automatically
- Background refetch keeps data fresh
- Optimistic updates provide instant feedback
- Consider pagination if user has 100+ todos (not required for MVP)

---
*Generated by Coordinator role from handoff-task-todo-crud-001.json*
