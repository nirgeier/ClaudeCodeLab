# Claude Code Tasks

- Hands-on Claude Code exercises covering CLI operations, project configuration, code generation, refactoring, and advanced automation.
- Each task includes a clear scenario, helpful hints, and detailed solutions with explanations.
- Practice these tasks to master building production applications using Claude Code.

---

#### Table of Contents

- [01. Install and Verify Claude Code](#01-install-and-verify-claude-code)
- [02. First Interactive Session](#02-first-interactive-session)
- [03. Create a CLAUDE.md File](#03-create-a-claudemd-file)
- [04. Generate a Project from Scratch](#04-generate-a-project-from-scratch)
- [05. One-Shot File Generation](#05-one-shot-file-generation)
- [06. Pipe Input to Claude Code](#06-pipe-input-to-claude-code)
- [07. Generate a Database Schema](#07-generate-a-database-schema)
- [08. Add Features to Existing Code](#08-add-features-to-existing-code)
- [09. Refactor a File](#09-refactor-a-file)
- [10. Bug Diagnosis and Fix](#10-bug-diagnosis-and-fix)
- [11. Generate Unit Tests](#11-generate-unit-tests)
- [12. Create a REST API Endpoint](#12-create-a-rest-api-endpoint)
- [13. Build a React Component](#13-build-a-react-component)
- [14. Git Workflow: Branch and PR](#14-git-workflow-branch-and-pr)
- [15. Code Review with Claude Code](#15-code-review-with-claude-code)
- [16. Generate Documentation](#16-generate-documentation)
- [17. CSV Processing Script](#17-csv-processing-script)
- [18. Environment Configuration](#18-environment-configuration)
- [19. Build a CLI Tool](#19-build-a-cli-tool)
- [20. Multi-File Refactoring](#20-multi-file-refactoring)
- [21. Supabase Edge Function](#21-supabase-edge-function)
- [22. Error Handling Patterns](#22-error-handling-patterns)
- [23. Performance Optimization](#23-performance-optimization)
- [24. Security Audit](#24-security-audit)
- [25. Full Feature: End-to-End](#25-full-feature-end-to-end)

---

#### 01. Install and Verify Claude Code

- Install Claude Code, verify the installation, and check the version and available commands.

#### Scenario:

- You're setting up a new development machine and need Claude Code ready for use. Verify all CLI capabilities.

Hint: Use `npm install -g @anthropic-ai/claude-code` and explore `claude --help`

??? success "Solution"

    ```bash
    # Install
    npm install -g @anthropic-ai/claude-code

    # Verify version
    claude --version

    # See all commands
    claude --help

    # Check available slash commands
    # Start claude, then type /help
    claude
    ```

    Type `/help` inside the interactive session to see all slash commands.

---

#### 02. First Interactive Session

- Start an interactive Claude Code session, ask it to describe the current directory, then ask it to create a simple "Hello World" Node.js app.

#### Scenario:

- Your first time using Claude Code. Learn the interactive workflow: ask questions, approve file changes, iterate.

Hint: Run `claude` to start interactive mode. Ask natural language questions.

??? success "Solution"

    ```bash
    # Start interactive session
    claude

    # Inside the session:
    > Describe the files in the current directory
    > Create a simple Node.js HTTP server in server.js that returns
      "Hello from Elcon!" on port 3000
    > Add a package.json with a start script
    ```

    Claude Code will show you the files it wants to create. Type `y` to approve each one.

---

#### 03. Create a CLAUDE.md File

- Create a comprehensive `CLAUDE.md` file for an Elcon project that includes project context, coding standards, and common commands.

#### Scenario:

- CLAUDE.md is Claude Code's memory file. A well-written one dramatically improves output quality.

Hint: Include: project description, tech stack, file structure, coding conventions, build/test commands, and common gotchas.

??? success "Solution"

    ```bash
    claude "Create a CLAUDE.md file for this project with the following:

    Project: Elcon Supplier Management System
    Tech stack: Next.js 14, TypeScript, Supabase, Tailwind CSS, shadcn/ui
    Database: Supabase PostgreSQL with RLS

    Include sections for:
    1. Project overview (what it does, who uses it)
    2. Tech stack with versions
    3. Project structure (key directories)
    4. Coding conventions (naming, file organization, imports)
    5. Common commands (dev, build, test, deploy)
    6. Database conventions (naming, migrations)
    7. Things to remember (gotchas, edge cases)
    "
    ```

---

#### 04. Generate a Project from Scratch

- Use Claude Code to scaffold a complete Next.js project with Supabase, Tailwind, and shadcn/ui in a single command.

#### Scenario:

- You need to bootstrap a new project quickly. Claude Code can generate the full scaffolding.

Hint: Use the `--print` flag to preview what will be generated, or let Claude Code run the commands.

??? success "Solution"

    ```bash
    claude "Create a new Next.js 14 project called elcon-suppliers with:
    - TypeScript
    - Tailwind CSS
    - shadcn/ui (install button, card, table, input, dialog components)
    - Supabase client library (@supabase/supabase-js)
    - App router
    - src/ directory structure
    - Environment variables template (.env.local.example)

    Initialize git and make the first commit.
    Run all setup commands."
    ```

---

#### 05. One-Shot File Generation

- Use `claude -p` (print mode) to generate a single utility file without an interactive session.

#### Scenario:

- You need a quick one-off file generated. Print mode outputs to stdout without starting a session.

Hint: Use `claude -p "prompt"` for non-interactive generation.

??? success "Solution"

    ```bash
    # Generate a utility file and save it
    claude -p "Create a TypeScript utility file with these functions:
    - formatCurrency(amount: number, currency: string): string
    - formatDate(date: Date, locale?: string): string
    - formatPhoneNumber(phone: string, country?: string): string
    - truncateText(text: string, maxLength: number): string
    All functions should handle edge cases and have JSDoc comments.
    Output only the TypeScript code." > src/utils/formatters.ts
    ```

---

#### 06. Pipe Input to Claude Code

- Pipe the output of a command into Claude Code for analysis.

#### Scenario:

- You have error logs or command output that you want Claude to analyze without copying/pasting.

Hint: Use pipe `|` to send output to `claude -p`.

??? success "Solution"

    ```bash
    # Analyze git log
    git log --oneline -20 | claude -p "Summarize these git commits and
    identify the main themes of recent development"

    # Analyze package dependencies
    cat package.json | claude -p "Review the dependencies.
    Flag any security concerns, outdated packages, or unnecessary deps"

    # Analyze error logs
    cat error.log | claude -p "Analyze these errors. Group by type,
    identify root causes, and suggest fixes"
    ```

---

#### 07. Generate a Database Schema

- Use Claude Code to generate a complete Supabase database schema for a supplier management module.

#### Scenario:

- You need to design and create database tables. Claude Code can generate the SQL and apply it.

Hint: Ask for CREATE TABLE statements with proper constraints, indexes, and RLS policies.

??? success "Solution"

    ```bash
    claude "Create a Supabase database schema for supplier management:

    Tables needed:
    1. suppliers (name, email, phone, address, country, payment_terms,
       rating 1-5, is_active, notes)
    2. contacts (linked to supplier, first_name, last_name, email,
       phone, position, is_primary)
    3. supplier_categories (name, description)
    4. supplier_category_map (many-to-many link)

    Requirements:
    - UUID primary keys
    - created_at, updated_at timestamps with triggers
    - Soft delete (is_active boolean)
    - Foreign key constraints
    - Indexes on frequently queried columns
    - RLS policies for authenticated users
    - 10 rows of seed data for each table

    Generate as a single SQL migration file."
    ```

---

#### 08. Add Features to Existing Code

- Use Claude Code to add search and filter functionality to an existing contacts list page.

#### Scenario:

- You have a working page that lists all contacts. You need to add search by name and filter by company.

Hint: Let Claude Code read the existing file first, then ask for the modification.

??? success "Solution"

    ```bash
    claude "Look at the contacts list page in src/app/contacts/page.tsx.
    Add these features:
    1. Search bar at the top that filters contacts by name or email
       (debounced, 300ms)
    2. Dropdown filter for company (populated from the data)
    3. Sort toggle on table headers (name, email, company)
    4. Clear all filters button
    5. Show result count: 'Showing X of Y contacts'

    Use the existing shadcn/ui components. Keep the same visual style.
    Do NOT change the data fetching logic."
    ```

---

#### 09. Refactor a File

- Use Claude Code to refactor a large component file into smaller, reusable components.

#### Scenario:

- A component has grown to 500+ lines. It needs to be split into logical parts.

Hint: Ask Claude Code to analyze the file first, then suggest and implement the split.

??? success "Solution"

    ```bash
    claude "Analyze src/components/SupplierForm.tsx.

    This file is too large. Refactor it by:
    1. Identify logical sections that can be separate components
    2. Extract each into its own file in src/components/supplier/
    3. Create an index.ts barrel export
    4. Keep the main SupplierForm as the parent, importing the children
    5. Move shared types to a types.ts file
    6. Ensure all props are properly typed

    Show me the plan first, then implement."
    ```

---

#### 10. Bug Diagnosis and Fix

- Use Claude Code to diagnose a runtime error from a stack trace and implement the fix.

#### Scenario:

- Your app crashes with an error. Instead of debugging manually, let Claude Code analyze the trace and fix it.

Hint: Paste the error message and let Claude Code trace it through the codebase.

??? success "Solution"

    ```bash
    claude "I'm getting this error when submitting the supplier form:

    TypeError: Cannot read properties of undefined (reading 'id')
    at SupplierForm (src/components/SupplierForm.tsx:47:23)
    at processChild (node_modules/react-dom/...)

    Diagnose the issue:
    1. Read the file and find line 47
    2. Trace where the undefined value comes from
    3. Identify the root cause
    4. Implement the fix with proper null checks
    5. Add a guard to prevent this class of error in the future"
    ```

---

#### 11. Generate Unit Tests

- Use Claude Code to generate comprehensive tests for a utility module.

#### Scenario:

- You need test coverage for a utility file. Claude Code can read the functions and generate tests.

Hint: Specify the test framework (vitest/jest) and ask for edge cases.

??? success "Solution"

    ```bash
    claude "Read src/utils/formatters.ts and generate comprehensive
    unit tests using Vitest.

    For each function, test:
    - Normal inputs (happy path)
    - Edge cases (empty string, null, undefined, 0, negative numbers)
    - Boundary values (max length, max number)
    - International formats where applicable

    Generate at least 5 tests per function.
    Put tests in src/utils/__tests__/formatters.test.ts
    Include a describe block for each function."
    ```

---

#### 12. Create a REST API Endpoint

- Use Claude Code to create a complete CRUD API endpoint for suppliers using Supabase Edge Functions.

#### Scenario:

- You need a backend API. Claude Code can generate the entire endpoint with validation and error handling.

Hint: Specify the HTTP methods, validation rules, and response format.

??? success "Solution"

    ```bash
    claude "Create a Supabase Edge Function at supabase/functions/suppliers/index.ts

    Endpoints:
    - GET /suppliers - List all (with pagination, search, filter)
    - GET /suppliers/:id - Get one by ID
    - POST /suppliers - Create (validate required fields)
    - PATCH /suppliers/:id - Update (partial update)
    - DELETE /suppliers/:id - Soft delete (set is_active=false)

    Requirements:
    - Validate auth token (Supabase JWT)
    - Input validation with clear error messages
    - Proper HTTP status codes (200, 201, 400, 401, 404, 500)
    - Pagination: ?page=1&limit=20
    - Search: ?q=search_term (searches name, email)
    - Filter: ?country=IL&is_active=true
    - Return consistent JSON: { data, error, meta: { total, page, limit } }
    - Add CORS headers"
    ```

---

#### 13. Build a React Component

- Use Claude Code to build a data table component with sorting, filtering, and pagination.

#### Scenario:

- You need a reusable table component for multiple pages in the CRM.

Hint: Specify the component API (props), features, and visual requirements.

??? success "Solution"

    ```bash
    claude "Create a reusable DataTable component using shadcn/ui and
    @tanstack/react-table.

    File: src/components/ui/data-table.tsx

    Props:
    - columns: ColumnDef[] (from @tanstack/react-table)
    - data: T[]
    - searchKey?: string (which column to search)
    - filterableColumns?: { id: string, title: string, options: [] }[]
    - pageSize?: number (default 20)

    Features:
    - Column sorting (click header to toggle)
    - Global search with debounce
    - Column-specific filters (dropdown)
    - Pagination with page size selector
    - Row selection with checkbox
    - Loading skeleton state
    - Empty state message

    Use Tailwind for styling. Support dark mode."
    ```

---

#### 14. Git Workflow: Branch and PR

- Use Claude Code to create a feature branch, make changes, commit, and prepare a PR description.

#### Scenario:

- You're adding a feature and need to follow a proper Git workflow.

Hint: Use Claude Code's Git integration: it can create branches, commit, and generate PR descriptions.

??? success "Solution"

    ```bash
    claude "Do the following git workflow:
    1. Create a new branch: feature/supplier-rating
    2. Add a 'rating' column (1-5 stars) to the supplier table component
    3. Add a star rating input to the supplier form
    4. Commit with a descriptive message
    5. Generate a PR description with:
       - What changed
       - Why it changed
       - How to test
       - Screenshots description"
    ```

---

#### 15. Code Review with Claude Code

- Pipe a git diff into Claude Code for a thorough code review.

#### Scenario:

- Before merging a PR, you want AI feedback on code quality, security, and potential issues.

Hint: Use `git diff` piped to Claude Code with specific review criteria.

??? success "Solution"

    ```bash
    git diff main..feature/supplier-rating | claude -p \
    "Review this code diff as a senior developer. Check for:
    1. Security issues (XSS, injection, auth bypass)
    2. Performance concerns (unnecessary re-renders, N+1 queries)
    3. Error handling (missing try/catch, unhandled promises)
    4. TypeScript issues (any types, missing generics)
    5. Code style (naming, structure, DRY violations)
    6. Edge cases not handled

    Format as: CRITICAL / WARNING / SUGGESTION
    For each issue, show the line and the suggested fix."
    ```

---

#### 16. Generate Documentation

- Use Claude Code to generate API documentation from existing code.

#### Scenario:

- Your API has no documentation. Claude Code can read the source and generate comprehensive docs.

Hint: Point Claude Code at the source files and specify the documentation format.

??? success "Solution"

    ```bash
    claude "Read all files in supabase/functions/ and generate API
    documentation in docs/API.md.

    For each endpoint, document:
    - HTTP method and URL
    - Description
    - Authentication requirements
    - Request parameters (query, body)
    - Request example (cURL)
    - Response example (JSON)
    - Error codes and meanings
    - TypeScript type definitions

    Add a table of contents at the top.
    Group endpoints by resource (suppliers, contacts, orders)."
    ```

---

#### 17. CSV Processing Script

- Use Claude Code to generate a Node.js script that imports supplier data from a CSV file into Supabase.

#### Scenario:

- Elcon has 500 suppliers in a CSV export from their old system. You need a reliable import script.

Hint: Specify CSV format, validation rules, error handling, and progress reporting.

??? success "Solution"

    ```bash
    claude "Create a Node.js script at scripts/import-suppliers.ts that:

    1. Reads a CSV file (path from CLI argument)
    2. Expected columns: name, email, phone, country, address,
       payment_terms, category
    3. Validates each row:
       - name is required, max 200 chars
       - email format validation
       - country is a valid ISO 2-letter code
       - payment_terms in ['Net 30', 'Net 60', 'Net 90', 'Prepaid']
    4. Batch inserts to Supabase (50 rows per batch)
    5. Handles duplicates (skip if email already exists)
    6. Logs progress: 'Imported 150/500 (30%)'
    7. Generates a report: total, imported, skipped, errors
    8. Writes errors to errors.csv with row number and reason

    Use @supabase/supabase-js. Read credentials from .env."
    ```

---

#### 18. Environment Configuration

- Use Claude Code to set up environment configuration with validation for dev, staging, and production.

#### Scenario:

- Your app needs different configs per environment with validation to catch missing variables early.

Hint: Use zod for runtime validation of environment variables.

??? success "Solution"

    ```bash
    claude "Create environment configuration with validation:

    File: src/lib/env.ts

    Variables:
    - NEXT_PUBLIC_SUPABASE_URL (required, URL format)
    - NEXT_PUBLIC_SUPABASE_ANON_KEY (required)
    - SUPABASE_SERVICE_ROLE_KEY (required, server-only)
    - N8N_WEBHOOK_URL (required, URL format)
    - N8N_API_KEY (required, server-only)
    - NODE_ENV (development | staging | production)

    Requirements:
    - Use zod for runtime validation
    - Throw descriptive error if any variable is missing
    - Separate client-safe vs server-only variables
    - Type-safe access throughout the app
    - Create .env.local.example with all variables listed

    Also update CLAUDE.md with the environment variable documentation."
    ```

---

#### 19. Build a CLI Tool

- Use Claude Code to build a CLI tool for managing Elcon's project from the terminal.

#### Scenario:

- You want a custom CLI for common project tasks: seeding data, running migrations, generating reports.

Hint: Use commander or yargs for CLI argument parsing.

??? success "Solution"

    ```bash
    claude "Create a CLI tool at scripts/elcon-cli.ts using commander.js

    Commands:
    - elcon db:seed - Seed database with sample data
    - elcon db:reset - Reset database (with confirmation prompt)
    - elcon suppliers:import <file> - Import suppliers from CSV
    - elcon suppliers:export - Export suppliers to CSV
    - elcon report:weekly - Generate weekly procurement report
    - elcon health - Check all service connections (DB, n8n, email)

    Requirements:
    - Colorful output (use chalk)
    - Spinner for long operations (use ora)
    - Error handling with helpful messages
    - --dry-run flag for destructive commands
    - --verbose flag for debug output
    - Add to package.json scripts: 'elcon': 'tsx scripts/elcon-cli.ts'"
    ```

---

#### 20. Multi-File Refactoring

- Use Claude Code to refactor the data fetching layer from inline queries to a clean repository pattern.

#### Scenario:

- Database queries are scattered across components. You need a centralized data access layer.

Hint: Ask Claude Code to find all Supabase queries, then consolidate them.

??? success "Solution"

    ```bash
    claude "Find all Supabase query calls across the project
    (grep for 'supabase.from' or '.select(' or '.insert(').

    Refactor to a repository pattern:
    1. Create src/repositories/suppliers.ts
    2. Create src/repositories/contacts.ts
    3. Create src/repositories/orders.ts
    4. Each repository exports typed functions:
       - getAll(filters?) → typed array
       - getById(id) → typed object | null
       - create(data) → typed object
       - update(id, data) → typed object
       - softDelete(id) → void
    5. Update all callers to use the repository functions
    6. Add proper TypeScript return types (infer from Supabase schema)

    Do not change any functionality, only move and centralize the queries."
    ```

---

#### 21. Supabase Edge Function

- Use Claude Code to create a Supabase Edge Function that calculates monthly procurement KPIs.

#### Scenario:

- Your dashboard needs a backend calculation for KPI metrics that are too complex for client-side.

Hint: Edge Functions run on Deno. Specify the input/output contract.

??? success "Solution"

    ```bash
    claude "Create a Supabase Edge Function: supabase/functions/kpi/index.ts

    Endpoint: POST /kpi
    Input: { month: '2026-04', department?: string }

    Calculate and return:
    {
      total_orders: number,
      total_value: number,
      avg_order_value: number,
      on_time_delivery_rate: number (percentage),
      top_suppliers: [{ name, order_count, total_value }], // top 5
      category_breakdown: [{ category, count, value }],
      month_over_month_change: number (percentage),
      overdue_orders: number
    }

    Requirements:
    - Authenticate with Supabase JWT
    - Use SQL queries for performance (not JS array operations)
    - Handle missing data gracefully
    - Return proper error responses
    - Add CORS headers"
    ```

---

#### 22. Error Handling Patterns

- Use Claude Code to add comprehensive error handling to an existing API module.

#### Scenario:

- Your API has minimal error handling. Production needs proper error boundaries, logging, and user-friendly messages.

Hint: Ask Claude Code to identify error-prone areas and add appropriate handling.

??? success "Solution"

    ```bash
    claude "Review src/repositories/suppliers.ts and add robust error handling:

    1. Wrap each function in try/catch
    2. Create custom error classes in src/lib/errors.ts:
       - NotFoundError (404)
       - ValidationError (400)
       - AuthorizationError (403)
       - DatabaseError (500)
    3. Map Supabase error codes to custom errors
    4. Add structured logging (timestamp, function name, error details)
    5. Return user-friendly error messages (never expose DB details)
    6. Add input validation with zod schemas before queries
    7. Ensure all errors are properly typed in function signatures

    Do not change the happy-path logic."
    ```

---

#### 23. Performance Optimization

- Use Claude Code to analyze and optimize a slow page that loads 1000+ rows.

#### Scenario:

- The suppliers list page takes 5+ seconds to load. You need to optimize without changing the feature set.

Hint: Ask Claude Code to identify performance bottlenecks and implement fixes.

??? success "Solution"

    ```bash
    claude "The suppliers list page is slow. Analyze and optimize:

    Current problems:
    - Loads all 1000+ suppliers at once
    - No pagination on the database query
    - Component re-renders on every keystroke in search
    - Each row fetches its own contact count

    Implement these optimizations:
    1. Server-side pagination (20 per page)
    2. Debounced search (300ms, server-side filtering)
    3. Use React.memo on table rows
    4. Batch the contact count into the main query (SQL JOIN)
    5. Add loading skeletons during fetch
    6. Use React Query with staleTime for caching
    7. Virtual scrolling for the table (use @tanstack/react-virtual)

    Benchmark: page should load in under 500ms."
    ```

---

#### 24. Security Audit

- Use Claude Code to perform a security audit on the entire project.

#### Scenario:

- Before production deployment, you need a security review of the codebase.

Hint: Ask Claude Code to scan for common vulnerabilities (OWASP Top 10).

??? success "Solution"

    ```bash
    claude "Perform a security audit on this project. Check for:

    1. Authentication: Are all API routes protected?
    2. Authorization: Are RLS policies properly configured?
    3. Input Validation: Are all user inputs sanitized?
    4. SQL Injection: Any raw SQL with user input?
    5. XSS: Any dangerouslySetInnerHTML or unescaped output?
    6. CSRF: Are state-changing requests protected?
    7. Secrets: Any hardcoded API keys or passwords?
    8. Dependencies: Any known vulnerabilities (check package.json)?
    9. CORS: Are origins properly restricted?
    10. Error Handling: Do errors leak internal details?

    For each finding:
    - Severity: CRITICAL / HIGH / MEDIUM / LOW
    - File and line number
    - Description of the vulnerability
    - Suggested fix with code

    Generate a SECURITY_AUDIT.md report."
    ```

---

#### 25. Full Feature: End-to-End

- Use Claude Code to implement a complete feature from database to UI: supplier rating system with history.

#### Scenario:

- Management wants to rate suppliers after each delivery. This requires: database tables, API, UI components, and notification workflow.

Hint: Break it into steps and let Claude Code handle each one.

??? success "Solution"

    ```bash
    # Step 1: Database
    claude "Create a database migration for a supplier rating system:
    - ratings table (supplier_id, rating 1-5, category, comment,
      rated_by, delivery_id, created_at)
    - Update suppliers table: add avg_rating computed column
    - Create a view: supplier_rating_summary
    - Add RLS: users can create/edit own ratings, read all"

    # Step 2: API
    claude "Create the API layer for supplier ratings:
    - src/repositories/ratings.ts
    - GET /ratings?supplier_id=xxx (list with pagination)
    - POST /ratings (create, validate, recalculate average)
    - PATCH /ratings/:id (edit own rating only)"

    # Step 3: UI
    claude "Create UI components for supplier ratings:
    - StarRating component (interactive, 1-5 stars)
    - RatingForm dialog (star select, category dropdown, comment)
    - RatingHistory list (sorted by date, with user avatar)
    - Add rating display to SupplierCard component
    - Add 'Rate Supplier' button to supplier detail page"

    # Step 4: Integration
    claude "Wire everything together:
    - SupplierDetail page shows rating history and average
    - 'Rate Supplier' button opens RatingForm dialog
    - Submitting recalculates the supplier's average
    - Send notification via n8n webhook when rating < 3"
    ```
