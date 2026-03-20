
1. User (Student/Member)
The primary consumer of the library resources.

Attributes (Model): userId, email, passwordHash, department, searchHistory (List of SearchQuery), issueHistory (List of IssueRecord).

Functions (Controller Access):
POST /api/auth/login & register: Access to the system.
GET /api/search: Query the book database.
GET /api/user/analytics: View their own immutable search history.
GET /api/archive/stream: Access and read PDF/EPUB resources.
POST /api/issue/request: Initiate a hold on a physical book.

2. Faculty (Department Admin)
A specialized user with content-contribution privileges.

Attributes (Model): All User attributes + employeeCode, departmentResourceArchive (List of PDF metadata), deptAnalytics.

Functions (Controller Access):
All User Permissions.
POST /api/faculty/upload: Upload academic notes/PDFs (restricted to .pdf, .epub, .docx).

3. Librarian (Inventory Admin)
The gatekeeper of physical and digital inventory.

Attributes (Model): staffId, staffCode, inventoryLog, pendingApprovals.

Functions (Controller Access):
POST /api/external/sync: Trigger Open Library/Google Books API to auto-populate book metadata.
POST /api/books/manual-entry: Manually add books to the catalog.
PUT /api/issue/confirm: Transition a book status from "Requested" to "Issued".
POST /api/fines/manage: Calculate and waive/collect fines.

Super Admin (System Owner)
The highest authority for system integrity and user management.
Attributes (Model): adminId, masterKey, systemAuditLogs.

Functions (Controller Access):
Full CRUD on Users: GET, POST, PUT, DELETE on the user table.
PATCH /api/admin/escalate: Change a User’s role (e.g., Student to Faculty).
GET /api/admin/audit: View SystemAuditLogs for security tracking.
POST /api/admin/db-maintenance: Trigger backups or index cleaning.



To integrate both the Open Library API and the Google Books API into your Spring Boot MVC structure, you need a robust External Service Layer. This layer will handle the differing JSON structures of both APIs and map them to a unified BookResponse DTO for your frontend.

Below is the implementation strategy, including the specific attributes and the service logic required to manage both sources.

1. The Service Architecture (Model/Service)
You will use an interface-driven approach. This allows the SearchController to remain agnostic of which API is being called.

ExternalBookService (Interface): Defines searchByIsbn(String isbn) and searchByQuery(String query).

OpenLibraryServiceImpl: Implements logic for openlibrary.org/api/volumes.

GoogleBooksServiceImpl: Implements logic for googleapis.com/books/v1/volumes.

2. Implementation of the API Clients
A. Open Library Integration
Endpoint: https://openlibrary.org/search.json?q={query}
Key Attributes to Map:

title: The book's name.

author_name: Array of authors.

isbn: Array of identifiers (crucial for your IssueRecord).

cover_i: Cover ID (used to construct the image URL: https://covers.openlibrary.org/b/id/{id}-L.jpg).

B. Google Books Integration (Secondary/Backup)
Endpoint: https://www.googleapis.com/books/v1/volumes?q={query}
Key Attributes to Map:

volumeInfo.title

volumeInfo.authors

volumeInfo.description: Usually more detailed than Open Library.

volumeInfo.imageLinks.thumbnail


