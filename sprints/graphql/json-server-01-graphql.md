# Add GraphQL Support to JSON Server

**Story ID:** json-server-01-graphql

## User Story

As a developer using JSON Server, I want to query my mock data using GraphQL in addition to REST, so that I can use modern GraphQL tooling while maintaining backward compatibility with existing REST consumers.

## Description

Add GraphQL query and mutation support to JSON Server alongside the existing REST API. Both APIs should work with the same underlying db.json data source.

## Acceptance Criteria

- [ ] Server exposes a `/graphql` endpoint that accepts POST requests with GraphQL queries
- [ ] GraphQL endpoint returns proper GraphQL response format with `data` and/or `errors` fields
- [ ] All existing REST endpoints (`/posts`, `/comments`, `/users`, etc.) continue to work unchanged
- [ ] GraphQL schema automatically reflects the structure of db.json
- [ ] Query operations work for fetching all items of a resource type (e.g., `query { posts { id title } }`)
- [ ] Query operations work for fetching single items by ID (e.g., `query { post(id: "1") { title } }`)
- [ ] Mutations work for creating new resources with auto-generated IDs
- [ ] Mutations work for updating existing resources
- [ ] Mutations work for deleting resources
- [ ] Changes made through GraphQL are immediately reflected in REST API responses
- [ ] Changes made through REST are immediately reflected in GraphQL query responses
- [ ] GraphQL introspection query returns valid schema information
- [ ] Invalid GraphQL queries return appropriate error messages
- [ ] Package.json includes GraphQL-related dependencies (graphql, express-graphql or apollo-server)

## Technical Requirements

- **Existing Stack:** JSON Server (Node.js/Express app)
- **Database:** JSON file (db.json)
- **GraphQL Library:** Add express-graphql, apollo-server-express, or similar

## Implementation Notes

Consider these approaches:
- Add GraphQL middleware to existing Express app
- Build schema dynamically from db.json structure
- Reuse existing data access logic where possible
- Map REST-style operations to GraphQL resolvers
- Ensure both APIs read/write the same db.json file
- Add error handling for malformed GraphQL queries

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