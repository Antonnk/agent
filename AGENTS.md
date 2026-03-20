# Agent Guidelines

## Project Setup

### Docker Containers
- Every service runs in Docker for portability and isolation
- Each service/part gets its own Docker image (e.g., Frontend, Backend, Database, API Gateway)
- Containers serve HTTP on port 80
- Every container exposes a healthcheck endpoint at `/up` returning `200 OK`
- Persistent data lives in `/storage`

### Structure
```
project/
├── docker-compose.yml
├── frontend/
│   ├── Dockerfile
│   └── ...
├── backend/
│   ├── Dockerfile
│   └── ...
└── services/
    └── ...
```

---

## Application Architecture

### 12-Factor App Principles
Follow [12factor.net](https://12factor.net/) methodology:
1. **Codebase** - One codebase tracked in git, multiple deployments
2. **Dependencies** - Explicitly declare and isolate dependencies
3. **Config** - Store config in environment variables
4. **Backing Services** - Treat backing services as attached resources
5. **Build, Release, Run** - Strictly separate build and run stages
6. **Processes** - Stateless processes, share-nothing architecture
7. **Port Binding** - Export services via port binding
8. **Concurrency** - Scale out via process model
9. **Disposability** - Fast startup and graceful shutdown
10. **Dev/Prod Parity** - Keep development and production similar
11. **Logs** - Treat logs as event streams
12. **Admin Processes** - Run admin/management tasks as one-off processes

### Microservices Design
- Each service is independently deployable
- Services communicate via well-defined APIs
- No shared databases between services
- Each service owns its data

---

## Backend Development

### Language Selection
Use the best fit for the job:
- **Go** - High performance, concurrency, microservices
- **Python** - Rapid development, data processing, scripting
- **PHP** - Web applications, quick iteration

### REST API Design
Follow best practices:
- Use proper HTTP methods: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`
- Resource naming: `/users`, `/users/{id}`, `/users/{id}/orders`
- Return appropriate status codes: `200`, `201`, `400`, `404`, `500`
- Consistent error response format:
  ```json
  {
    "error": {
      "code": "RESOURCE_NOT_FOUND",
      "message": "User with id 123 not found"
    }
  }
  ```
- Versioning via URL path: `/api/v1/...`
- Pagination for collections: `?page=1&limit=20`
- Support filtering and sorting via query params

### API Responses
```json
{
  "data": { ... },
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 100
  }
}
```

---

## Frontend Development

### Principles
- **Simplicity first** - Prefer simple solutions over complex ones
- **Accessibility (a11y)** - WCAG guidelines, semantic HTML, keyboard navigation
- **Browser compatibility** - Leverage standard browser features for broad support
- **Progressive enhancement** - Core functionality works without JavaScript

### HTML
- Use semantic HTML5 elements (`<header>`, `<nav>`, `<main>`, `<article>`, `<footer>`)
- Always include `<title>`, meta description, and language attribute
- Forms with proper labels, fieldsets, and error messaging

### CSS
- Mobile-first responsive design
- Prefer CSS custom properties for theming
- Use system fonts or a single web font
- Minimize specificity, use utility classes sparingly

### JavaScript
- Vanilla JS only unless framework adds significant value
- Feature detection over browser detection
- Accessible interactions (ARIA attributes, focus management)
- No blocking scripts in `<head>`

---

## Proof of Concept (POC) Development

For rapid prototyping and POCs:
- **Single HTML file** with embedded CSS and JavaScript
- **No build steps** - Open directly in browser
- **Local Storage** for data persistence
- **Progressive enhancement** - Works without JS when possible
- Keep it minimal - just enough to validate the concept

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>POC Title</title>
  <style>/* CSS here */</style>
</head>
<body>
  <!-- HTML here -->
  <script>/* JS here */</script>
</body>
</html>
```

---

## Feature Development

### User Story Format
Define features using the As a / I want / So that format:

```
As a [user type]
I want [action]
So that [benefit]
```

Example:
```
As a registered user
I want to reset my password via email
So that I can regain access to my account if I forget it
```

### Feature Requirements
- Every feature requires at least one test
- Features are developed on a `feature/` branch
- Tests define expected behavior, not implementation details

### Feature Branch Workflow
1. Create branch: `git checkout -b feature/description`
2. Write user story and acceptance criteria
3. Implement feature with tests
4. Ensure tests pass
5. Open PR for review
6. Merge to `main`

---

## Code Quality

### General Principles
- **DRY** - Don't repeat yourself
- **KISS** - Keep it simple
- **YAGNI** - You aren't gonna need it
- Write self-documenting code with clear naming
- Small, focused functions (aim for < 30 lines)
- Single responsibility principle

### Naming Conventions
- Variables/functions: `camelCase`
- Types/Classes: `PascalCase`
- Constants: `SCREAMING_SNAKE_CASE`
- Files: `kebab-case` or language convention

### Error Handling
- Handle errors gracefully, never expose stack traces to users
- Log errors appropriately for debugging
- Fail fast with clear error messages

---

## Testing

### Strategy
- **Unit tests** for business logic and utility functions
- **Integration tests** for API endpoints
- **Smoke tests** for critical paths
- Test behavior, not implementation

### What to Test
- Happy path scenarios
- Edge cases and boundary conditions
- Error conditions
- Security-sensitive operations

---

## Git Workflow

### Branch Naming
- `feature/description`
- `bugfix/description`
- `hotfix/description`
- `release/v1.0.0`

### Commit Messages
```
feat: add user authentication
fix: resolve race condition in data sync
docs: update API documentation
refactor: simplify query builder
test: add integration tests for checkout
```

### Process
1. Create feature branch from `main`
2. Make small, focused commits
3. Test before pushing
4. Use PRs for code review
5. Squash and merge or rebase

---

## Working with AI Coding Assistants

### Effective Collaboration
- **Be specific** - Provide context about the project, constraints, and goals
- **State the problem, not the solution** - Describe what you need, not how to implement it
- **Iterate** - Start simple, then refine based on feedback
- **Review carefully** - Always verify generated code for correctness and security

### When to Use AI
- Boilerplate code generation
- Explaining unfamiliar code
- Debugging and troubleshooting
- Documentation
- Learning new concepts

### When to Avoid AI
- Security-sensitive code (write yourself)
- Complex business logic (review heavily)
- Performance-critical code (verify thoroughly)
- Code you don't understand (learn first)

### Best Practices
1. Always read and understand code before accepting
2. Run tests after generating code
3. Check for security vulnerabilities
4. Verify against project conventions
5. Ask for explanations, not just code

---

## Security

- Never commit secrets (API keys, passwords) to version control
- Use environment variables for sensitive configuration
- Validate and sanitize all input
- Use parameterized queries to prevent SQL injection
- Implement proper authentication and authorization
- Enable HTTPS in production

---

## Performance

- Profile before optimizing
- Optimize for the common case
- Cache appropriately (HTTP caching, application caching)
- Use database indexes for frequent queries
- Lazy load resources when possible
- Monitor and measure in production
