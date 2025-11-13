# Add GraphQL Support to JSON Server

**Story ID:** json-server-01-graphql

## User Story

As a developer using JSON Server, I want to query my mock data using GraphQL in addition to REST, so that I can use modern GraphQL tooling while maintaining backward compatibility with existing REST consumers.

## Description

Add GraphQL query and mutation support to JSON Server alongside the existing REST API. Both APIs should work with the same underlying db.json data source.

## Acceptance Criteria

### Core GraphQL Functionality
- [ ] Server exposes a `/graphql` endpoint that accepts POST requests with GraphQL queries
- [ ] GraphQL endpoint returns proper GraphQL response format with `data` and/or `errors` fields
- [ ] GraphQL endpoint also accepts GET requests for GraphQL Playground/GraphiQL interface (returns 200 or 405)
- [ ] All existing REST endpoints (`/posts`, `/comments`, `/users`, etc.) continue to work unchanged
- [ ] GraphQL schema automatically reflects the structure of db.json
- [ ] Package.json includes GraphQL-related dependencies (e.g., graphql, express-graphql, apollo-server-express, or graphql-tools)

### Query Operations
- [ ] Query all items of a resource type (e.g., `query { posts { id title author } }`)
- [ ] Query single item by ID (e.g., `query { post(id: "1") { id title author } }`)
- [ ] Support nested/relational queries (e.g., `query { posts { id title comments { id body } } }`)
- [ ] Support filtering with arguments (e.g., `query { posts(author: "Alice") { id title } }`)
- [ ] Support pagination with limit/offset (e.g., `query { posts(limit: 2, offset: 0) { id title } }`)
- [ ] Support GraphQL aliases and fragments for advanced queries

### Mutation Operations
- [ ] Mutations work for creating new resources with auto-generated IDs
- [ ] Mutations work for updating existing resources
- [ ] Mutations work for deleting resources
- [ ] Changes made through GraphQL are immediately reflected in REST API responses
- [ ] Changes made through REST are immediately reflected in GraphQL query responses

### Schema and Validation
- [ ] GraphQL introspection query (`__schema`) returns valid schema information
- [ ] Invalid GraphQL queries return appropriate error messages in GraphQL format
- [ ] Schema properly defines types for all resources in db.json

## Technical Requirements

- **Existing Stack:** JSON Server (Node.js/Express app using TypeScript and Bun runtime)
- **Database:** JSON file (db.json) with posts, comments, and users
- **GraphQL Library:** Add express-graphql, apollo-server-express, or similar
- **Port Configuration:** GraphQL endpoint should be available on the same port as REST API
- **Development Tools:** GraphQL Playground or GraphiQL should be available in development mode

## Implementation Notes

Consider these approaches:
- Add GraphQL middleware to existing Express app at `/graphql` endpoint
- Build schema dynamically from db.json structure to support all resource types
- Implement resolvers for:
  - Fetching all items (with filtering and pagination support)
  - Fetching single items by ID
  - Nested/relational queries (e.g., posts with their comments)
  - Create, update, and delete mutations
- Reuse existing JSON Server data access logic where possible
- Map REST-style operations to GraphQL resolvers
- Ensure both APIs read/write the same db.json file for data consistency
- Add error handling for malformed GraphQL queries
- Enable GraphQL Playground/GraphiQL in development mode for testing

## Evaluation Criteria

### Code Quality (25%)
Clean integration with existing codebase. Proper GraphQL schema design. Good error handling.

### Completeness (35%)
All acceptance criteria met. Both REST and GraphQL fully functional. Data consistency maintained.

### Backward Compatibility (25%)
No breaking changes to REST API. Existing functionality preserved. Tests still pass.

### GraphQL Implementation (15%)
Proper schema generation. Efficient resolvers. Standard GraphQL patterns followed.

## Files Likely Modified

- `src/server/index.js` - Add GraphQL middleware
- `package.json` - Add GraphQL dependencies
- New files for GraphQL schema/resolvers
- Existing data access code may need refactoring for reuse