# Books API Week 02 Spec - Version 1

## Feature 1: Book CRUD Operations and Author References

### Goal
Update the existing Week 01 book API so book documents include a reference to an author and the API supports all CRUD operations for books. Every book route must be documented and testable in Swagger.

### Data Model
Book documents will be stored in the `books` collection.

Required book fields:
- `id`: string, required, custom id such as `b1`
- `authorId`: string, required, references the `id` field of an author document
- `title`: string, required
- `publicationDate`: string, required

Books will continue to use custom string ids instead of MongoDB `_id` values for route parameters.

### Relationship to Authors
Each book will identify its author with an `authorId` field. The value of `authorId` must match the custom `id` value of an existing author document.

When creating or updating a book, the API should reject the request with a `400` status code if the submitted `authorId` does not match an existing author.

### Routes

#### GET /books
Purpose: Return all books.

Success:
- Status code: `200`
- Response body: an array of book objects

Errors:
- `500` if an unexpected server or database error occurs

#### GET /books/:id
Purpose: Return one book by its custom id.

Success:
- Status code: `200`
- Response body: the matching book object

Errors:
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

#### POST /books
Purpose: Create a new book.

Request body:

    {
      "id": "b4",
      "authorId": "a1",
      "title": "Example Book Title",
      "publicationDate": "2026-01-15"
    }

Success:
- Status code: `201`
- Response body: the newly created book object

Errors:
- `400` if a required field is missing
- `400` if the `id` already exists
- `400` if the `authorId` does not match an existing author
- `500` if an unexpected server or database error occurs

#### PUT /books/:id
Purpose: Update an existing book.

Request body:

    {
      "authorId": "a2",
      "title": "Updated Book Title",
      "publicationDate": "2026-02-20"
    }

Success:
- Status code: `200`
- Response body: the updated book object

Errors:
- `400` if a required field is missing
- `400` if the `authorId` does not match an existing author
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

#### DELETE /books/:id
Purpose: Delete an existing book.

Success:
- Status code: `204`
- Response body: none

Errors:
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

### Swagger Documentation
Swagger must document every book route.

### Deployment Expectations
After implementation, the book routes must work locally and from the deployed Render application. The deployed Swagger page at `/api-docs` must allow someone to test every book route from the browser.

## Feature 2: Author CRUD Operations

### Goal
Add an `authors` collection that stores author ids, names, and birth years. The API supports all CRUD operations for authors. Every author route must be documented and testable in Swagger.

### Data Model
Author documents will be stored in the `authors` collection.

Required author fields:
- `id`: string, required, custom id such as `a1`
- `name`: string, required
- `birthYear`: integer, required

Authors will use custom string ids instead of MongoDB `_id` values for route parameters.

### Relationship to Books
Authors are referenced by documents in the `books` collection through the `authorId` field. The value of `authorId` in a book matches the custom `id` of an author.

### Routes

#### GET /authors
Purpose: Return all authors.

Success:
- Status code: `200`
- Response body: an array of author objects

Errors:
- `500` if an unexpected server or database errors occurs

#### GET /authors/:id
Purpose: Return one author by its custome id.

Success:
- Status code: `200`
- Response body: the matching author object

Errors:
- `404` if no author exists with that id
- `500` if an unexpected server or database error occurs

#### POST /authors
Purpose: Create a new author.

Request body:

    {
        "id": "a1",
        "name": "J.R.R. Tolkien",
        "birthYear": 1892
    }

Success:
- Status code: `201`
- Response body: the newly created author object

Errors:
- `400` if a require field is missing
- `400` if `birthYear` is not an integer
- `400` if the `id` already exists
- `500` if an unexpected server or database error occurs

#### PUT /authors/:id
Purpose: Update an existing author. The `id` cannot be changed through this route.

Request body:

    {
        "name": "Updated Author Name",
        "birthYear": 1900
    }

Success:
- Status code: `200`
- Response body: the updated author object

Errors:

- `400` if a required field is missing
- `400` if `birthYear` is not an integer
- `404` if no author exists with that id
- `500` if an unexpected server or database error occurs

#### DELETE /authors/:id
Purpose: Delete an existing author.

Success:
- Status code: `204`
- Response body: none

Errors:
- `404` if no author exists with that id
- `409` if the author still has books assigned to it (books with a matching `authorId`)
- `500` if an unexpected server or database error occurs

### Swagger Documentation
Swagger must document every author route.

### Deployment Expectations
After implementation, the author routes must work locally and from the deployed Render application. The deployed Swagger page at `/api-docs` must allow someone to test every author route from the browser.



# Books API Week 02 Spec - Version 2

## Feature 1: Book CRUD Operations and Author References

### Goal
Update the existing Week 01 book API so book documents include a reference to an author and the API supports all CRUD operations for books. Every book route must be documented and testable in Swagger.

### Data Model
MongoDB database: cse341-books-db
MongoDB collection: books

Required book fields:
- `id`: string, required, custom id such as `b1`
- `authorId`: string, required, references the `id` field of an author document
- `title`: string, required
- `publicationDate`: string in ISO 8601 date format (for example, "2021-08-17"), required

Books will continue to use custom string ids instead of MongoDB `_id` values for route parameters.

### Relationship to Authors
Each book will identify its author with an `authorId` field. The value of `authorId` must match the custom `id` value of an existing author document.

When creating or updating a book, the API should reject the request with a `400` status code if the submitted `authorId` does not match an existing author.

### Routes

#### GET /books
Purpose: Return all books.

Success:
- Status code: `200`
- Response body: an array of book objects

Response body example:

    [
        {
            "id": "b1",
            "authorId": "a1",
            "title": "Patterns of Light",
            "publicationDate": "2021-08-17"
        }
    ]

Errors:
- `500` if an unexpected server or database error occurs

#### GET /books/:id
Purpose: Return one book by its custom id.

Success:
- Status code: `200`
- Response body: the matching book object

Response body example:

    {
        "id": "b1",
        "authorId": "a1",
        "title": "Patterns of Light",
        "publicationDate": "2021-08-17"
    }

Errors:
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

#### POST /books
Purpose: Create a new book.

Request body:

    {
        "id": "b4",
        "authorId": "a1",
        "title": "Example Book Title",
        "publicationDate": "2026-01-15"
    }

Success:
- Status code: `201`
- Response body: the newly created book object

Errors:
- `400` if a required field is missing
- `400` if a field has the wrong type (all fields must be strings) or `publicationDate` is not in `YYYY-MM-DD` format
- `400` if the `authorId` does not match an existing author
- `409` if a book with the same `id` already exists
- `500` if an unexpected server or database error occurs

#### PUT /books/:id
Purpose: Update an existing book. The `id` cannot be changed through this route.

Request body:

    {
        "authorId": "a2",
        "title": "Updated Book Title",
        "publicationDate": "2026-02-20"
    }

Success:
- Status code: `200`
- Response body: the updated book object

Response body example:

    {
        "id": "b1",
        "authorId": "a2",
        "title": "Updated Book Title",
        "publicationDate": "2026-02-20"
    }

Errors:
- `400` if a required field is missing
- `400` if a field has the wrong type or `publicationDate` is not in `YYYY-MM-DD` format
- `400` if the `authorId` does not match an existing author
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

#### DELETE /books/:id
Purpose: Delete an existing book.

Success:
- Status code: `204`
- Response body: none

Errors:
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

### Swagger Documentation
Swagger must document every book route. Each route must include the `id` parameter (where it applies), the request body example (POST and PUT), and every status code listed in this spec.

### Deployment Expectations
After implementation, the book routes must work locally and from the deployed Render application. The deployed Swagger page at `/api-docs` must allow someone to test every book route from the browser.

## Feature 2: Author CRUD Operations

### Goal
Add an author collection that stores author ids, names and birth years. The API supports all CRUD operations for authors. Every author route must be documented and testable in Swagger.

### Data Model
MongoDB collection: authors

Required author fields:
- `id`: string, required, custom id such as `a1`
- `name`: string, required
- `birthYear`: integer, required (for example, 1892)

Authors will use custom string ids instead of MongoDB `_id` values for route parameters.

### Relationship to Books
Authors are referenced by documents in the `books` collection through the `authorId` field. The value of `authorId` in a book matches the custom `id` of an author.

### Routes

#### GET /authors
Purpose: Return all authors.

Success:
- Status code: `200`
- Response body: an array of author objects

Response body example:

    [
        {
            "id": "a1",
            "name": "Maya Rivera",
            "birthYear": 1985
        }
    ]

Errors:
- `500` if an unexpected server or database error occurs

#### GET /authors/:id
Purpose: Return one author by its custom id.

Success:
- Status code: `200`
- Response body: the matching author object

Response body example:

    {
        "id": "a1",
        "name": "Maya Rivera",
        "birthYear": 1985
    }

Errors:
- `404` if no author exists with that id
- `500` if an unexpected server or database error occurs

#### POST /authors
Purpose: Create a new author.

Request body:

    {
        "id": "a3",
        "name": "J.R.R. Tolkien",
        "birthYear": 1892
    }

Success:
- Status code: `201`
- Response body: the newly created author object

Errors:
- `400` if a required field is missing
- `400` if `id` or `name` is not a string, or `birthYear` is not an integer
- `409` if an author with the same `id` already exists
- `500` if an unexpected server or database error occurs

#### PUT /authors/:id
Purpose: Update an existing author. The `id` cannot be changed through this route.

Request body:

    {
        "name": "Updated Author Name",
        "birthYear": 1900
    }

Success:
- Status code: `200`
- Response body: the updated author object

Response body example:

    {
        "id": "a3",
        "name": "Updated Author Name",
        "birthYear": 1900
    }

Errors:
- `400` if a required field is missing
- `400` if `name` is not a string or `birthYear` is not an integer
- `404` if no author exists with that id
- `500` if an unexpected server or database error occurs

#### DELETE /authors/:id
Purpose: Delete an existing author.

Success:
- Status code: `204`
- Response body: none

Errors:
- `404` if no author exists with that id
- `409` if the author still has books assigned to it (books with a matching `authorId`). The author is not deleted.
- `500` if an unexpected server or database error occurs

### Swagger Documentation
Swagger must document every author route. Each route must include the `id` parameter (where it applies), the request body example (POST and PUT), and every status code listed in this spec.

### Deployment Expectations
After implementation, the author routes must work locally and from the deployed Render application. The deployed Swagger page at `/api-docs` must allow someone to test every author route from the browser.

## Error Handling
Applies to both books and authors.

- Do not return stack traces to clients.
- Errors return a JSON body with a `message`:
    - `400`: { "message": "Missing required field" }, { "message": "Invalid field value" } or { "message": "Author does not exist" }
    - `404`: { "message": "Book not found" } or { "message": "Author not found" }
    - `409`: { "message": "Id already exists" } or { "message": "Cannot delete author with assigned books" }
    - `500`: { "message": "Internal server error" }
- A required field is missing if it is not sent, is `null`, or is an empty string.
- Only the fields in the data model are saved. Extra fields in the request body are ignored.

## Implementation Notes
- Use MongoDB collections named `books` and `authors`.
- Create a unique index on `id` in both collections so two documents cannot have the same id.
- Seed the `authors` collection first, then the `books` collection (books use the `authorId` of the seeded authors). Replace the old `author` name field in the Week 01 books with `authorId`.
- Check that the values sent in the request body are the right type before using them in a MongoDB query.
- Use environment variables for the MongoDB connection, locally and in Render.

## Out of Scope for Week 2
- Authentication and authorization
- Pagination, search and filters