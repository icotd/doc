--------------------------------
# IMPORTANT Rules Start 
--------------------------------
## Tech stack
- When creating code or examples, default strictly to the following tech stack:
   - Runtime: Bun
   - Bundler/Dev Server: Vite
   - Language: TypeScript
   - Backend Framework: ElysiaJS 
   - ORM: Drizzle ORM
   - Frontend Framework: Svelte 5 with SvelteKit
   - Styling: Tailwind CSS integrated with DaisyUI
   - Icons: iconify for Tailwind v4, specifically the iconify-lucide-icons package
- Do not include any libraries or technologies outside of this stack unless absolutely necessary and explicitly justified 
- Use DaisyUI classes first, Tailwind utilities second
- Do not use `on:click|preventDefault={}` or `onclick|preventDefault={}` , instead use `onclick={(e) => { e.preventDefault() }}`
- Do not use `href="#"` as placeholder link, instead use `href="#/"` as a placeholder link
- Do not create tailwindcss.config.js (refer styling rules)
- Non-interactive elements with a click event must be accompanied by a keyboard event handler. Consider whether an interactive element such as `<button type="button">` or `<a>` might be more appropriate. `<div>` with a click handler must have an ARIA role

## DRY (Don't Repeat Yourself)
    - Extract repeated code into reusable functions
    - Share common logic through proper abstraction
    - Maintain single sources of truth
    - Do not hardcode data instead use a data source like a javascript array or an API

   Example 1: Utility function
    src/lib/utils/formatName.ts
    ```ts
    export function formatName(user) {
        return `${user.firstName} ${user.lastName}`;
    }
    ```

    Example 2: Data source
    src/lib/data/users.ts
    ```ts
    export const users = [
        { id: 1, firstName: 'John', lastName: 'Doe' },
        { id: 2, firstName: 'Jane', lastName: 'Doe' },
    ];

    Example 3: Reusable component 

    ```svelte
        // src/lib/components/UserCard.svelte
        <script>
            let { user } = $props();
        </script>

        <div class="card">
            <h3>{user.firstName} {user.lastName}</h3>
            <p>Role: {user.role}</p>
        </div>
    ```
 
    Example 4: Reusing svelte components
    ```svelte
        // src/routes/index.svelte
        <script>
            import { users } from '$lib/data';
            import UserCard from '$lib/components/UserCard.svelte';
        </script>

        <h2>All Users</h2>
        {#each users as user}
            <UserCard {user} />
        {/each}
    ```

## Clean Code Guidelines 
  - Constants Over Magic Numbers
    - Replace hard-coded values with named constants
    - Use descriptive constant names that explain the value's purpose
    - Keep constants at the top of the file or in a dedicated constants file

    Example 1: Constants
    src/lib/constants/maxUsers.ts
    ```ts
    export const MAX_USERS = 10;
    ```

  - Meaningful Names
    - Variables, functions, and classes should reveal their purpose
    - Names should explain why something exists and how it's used
    - Avoid abbreviations unless they're universally understood

  - Smart Comments
    - Don't comment on what the code does - make the code self-documenting
    - Use comments to explain why something is done a certain way
    - Document APIs, complex algorithms, and non-obvious side effects

  - Single Responsibility
    - Each function should do exactly one thing
    - Functions should be small and focused
    - If a function needs a comment to explain what it does, it should be split

  - Clean Structure
    - Keep related code together
    - Organize code in a logical hierarchy
    - Use consistent file and folder naming conventions

  - Encapsulation
    - Hide implementation details
    - Expose clear interfaces
    - Move nested conditionals into well-named functions

  - Code Quality Maintenance
    - Refactor continuously
    - Fix technical debt early
    - Leave code cleaner than you found it

  - Testing
    - Write tests before fixing bugs
    - Keep tests readable and maintainable
    - Test edge cases and error conditions

  - Version Control
    - Write clear commit messages
    - Make small, focused commits
    - Use meaningful branch names 


--------------------------------
# IMPORTANT Rules End 
--------------------------------
  
--------------------------------
# TypeScript Best Practices Rules Start 
--------------------------------

## Type System
- Prefer interfaces over types for object definitions
- Use type for unions, intersections, and mapped types
- Avoid using `any`, prefer `unknown` for unknown types
- Use strict TypeScript configuration
- Leverage TypeScript's built-in utility types
- Use generics for reusable type patterns

## Naming Conventions
- Use PascalCase for type names and interfaces
- Use camelCase for variables and functions
- Use UPPER_CASE for constants
- Use descriptive names with auxiliary verbs (e.g., isLoading, hasError)
- Prefix interfaces for React props with 'Props' (e.g., ButtonProps)

## Code Organization
- Keep type definitions close to where they're used
- Export types and interfaces from dedicated type files when shared
- Use barrel exports (index.ts) for organizing exports
- Place shared types in a `types` directory
- Co-locate component props with their components

## Functions
- Use explicit return types for public functions
- Use arrow functions for callbacks and methods
- Implement proper error handling with custom error types
- Use function overloads for complex type scenarios
- Prefer async/await over Promises

## Best Practices
- Enable strict mode in tsconfig.json
- Use readonly for immutable properties
- Leverage discriminated unions for type safety
- Use type guards for runtime type checking
- Implement proper null checking
- Avoid type assertions unless necessary

## Error Handling
- Create custom error types for domain-specific errors
- Use Result types for operations that can fail
- Implement proper error boundaries
- Use try-catch blocks with typed catch clauses
- Handle Promise rejections properly

## Patterns
- Use the Builder pattern for complex object creation
- Implement the Repository pattern for data access
- Use the Factory pattern for object creation
- Leverage dependency injection
- Use the Module pattern for encapsulation 


--------------------------------
# Code Quality Guidelines Start 
--------------------------------
## Verify Information
Always verify information before presenting it. Do not make assumptions or speculate without clear evidence.
## File-by-File Changes
Make changes file by file and give me a chance to spot mistakes.
## No Apologies
Never use apologies.
## No Understanding Feedback
Avoid giving feedback about understanding in comments or documentation.
## No Whitespace Suggestions
Don't suggest whitespace changes.
## No Summaries
Don't summarize changes made.
## No Inventions
Don't invent changes other than what's explicitly requested.
## No Unnecessary Confirmations
Don't ask for confirmation of information already provided in the context.
## Preserve Existing Code
Don't remove unrelated code or functionalities. Pay attention to preserving existing structures.
## Single Chunk Edits
Provide all edits in a single chunk instead of multiple-step instructions or explanations for the same file.
## No Implementation Checks
Don't ask the user to verify implementations that are visible in the provided context.
## No Unnecessary Updates
Don't suggest updates or changes to files when there are no actual modifications needed.
## Provide Real File Links
Always provide links to the real files, not x.md.
## No Current Implementation
Don't show or discuss the current implementation unless specifically requested.
--------------------------------
# Code Quality Guidelines End 
--------------------------------

--------------------------------
# Drizzle ORM Rules Start 
--------------------------------

## Project Setup (Bun SQL + Drizzle ORM)

This guide assumes familiarity with:
- Bun - javaScript all-in-one toolkit 
- Bun SQL - native bindings for working with PostgreSQL databases 
- When using Bun SQL, dialect should be set to 'postgresql'

```sh
bun add drizzle-orm
bun add -D drizzle-kit
```

📦 <project root>
 ├ 📂 drizzle
 ├ 📂 src
    ├ 📂 lib
        ├ 📂 db
            └ 📜 schema.ts
            └ 📜 index.ts
 ├ 📜 .env
 ├ 📜 drizzle.config.ts
 ├ 📜 package.json
 └ 📜 tsconfig.json

## Create .env file
```
DATABASE_URL="sqlite:./db/dev.db"
```
 
## Create drizzle config 

```ts
// drizzle.config.ts
import { defineConfig } from 'drizzle-kit';

export default defineConfig({
  out: './drizzle',
  schema: './src/lib/db/schema.ts',
  dialect: 'postgresql',
  dbCredentials: {
    url: process.env.DATABASE_URL!,
  },
});
```

## Connect Drizzle ORM to the database
```ts
// src/lib/db/index.ts
import { drizzle } from 'drizzle-orm/bun-sql';
import { SQL } from 'bun';
const client = new SQL(process.env.DATABASE_URL!);
const db = drizzle({ client });
```
 
## Create schema
- Since we are using Bun SQL, we need to use the pg-core dialect
```ts
// src/lib/db/schema.ts
import { integer, pgTable, varchar } from "drizzle-orm/pg-core";
export const usersTable = pgTable("users", {
  id: integer().primaryKey().generatedAlwaysAsIdentity(),
  name: varchar({ length: 255 }).notNull(),
  age: integer().notNull(),
  email: varchar({ length: 255 }).notNull().unique(),
});
 
```

## Apply changes to the database
```sh
bunx drizzle-kit generate 
bunx drizzle-kit migrate
```

## Open the Drizzle Studio
```sh
bunx drizzle-kit studio
```

## Push changes to the database
```sh
bunx drizzle-kit push
```

## Schema Declaration
- Use `pgTable`, `mysqlTable`, or `sqliteTable` depending on the SQL dialect.
- Always export table definitions to support Drizzle Kit migrations.
- Group schemas by domain or keep in a single file as per project structure.
- Use type inference (`$inferInsert`, `$inferSelect`) to define insert/select types.

## Select Queries
- Use `.select().from(table)` for full row fetches.
- Prefer partial selects for performance and clarity: `.select({ id: table.id, name: table.name })`.
- Avoid `select *`; Drizzle explicitly selects columns for consistent field order.

## Insert Operations
- `.insert(table).values({...})` to add data.
- Use `.returning()` on PostgreSQL and SQLite for getting inserted rows.
- For upserts, use `.onConflictDoNothing()` or `.onConflictDoUpdate()` (PostgreSQL, SQLite only).

## Update Operations
- Use `.update(table).set({...}).where(...)`.
- Combine with `.returning()` to retrieve updated values (PostgreSQL, SQLite only).

## Delete Operations
- Use `.delete(table)` to remove all rows or `.delete(table).where(...)` for conditional deletes.
- PostgreSQL and SQLite support `.returning()` for deleted values.

## Joins
- Use `.leftJoin`, `.innerJoin`, `.rightJoin`, `.fullJoin`.
- Match keys using Drizzle's conditionals: `eq(table1.col, table2.col)`.

## Relations
- Use `relations(table, ({ one, many }) => ...)` to define associations.
- Explicitly reference foreign keys via `fields` and `references`.

## Filtering & Operators
- Use built-in operators like `eq`, `ne`, `gt`, `lt`, `gte`, `lte`, etc.
- Support combining conditions via SQL interpolations or nested where clauses.

## Prepared Statements
- Use `.prepare("name")` to cache SQL strings and improve performance.
- Useful in repeated or large queries.

## Transactions
- Wrap critical operations in `await db.transaction(async (tx) => { ... })`.
- Support nested transactions and savepoints.
- `tx.rollback()` to revert operations programmatically.

## Batch Queries
- Use `db.batch([...])` for LibSQL and D1 to execute multiple queries atomically.

## Set Operations
- Use `union`, `unionAll`, `intersect`, `except` for combining queries.
- Supported in PostgreSQL, MySQL, and SQLite with similar syntax.

## Indexes & Constraints
- Define constraints (`default`, `notNull`, `unique`, `primaryKey`, `foreignKey`) in schema.
- Create indexes via `.uniqueIndex()` or `.index()` functions in table definition.

## Column Types
- Choose types from respective dialect core packages:
  - `drizzle-orm/pg-core`
  - `drizzle-orm/mysql-core`
  - `drizzle-orm/sqlite-core`
- Always specify mode (`number`, `bigint`, `boolean`, etc.) where required.

## Modes
- For MySQL, use `mode: 'default'` or `mode: 'planetscale'` depending on the driver.

## Environment-Specific Drivers
- PlanetScale → `drizzle-orm/planetscale-serverless`
- Neon → `drizzle-orm/neon-http` or `neon-serverless`
- LibSQL → `drizzle-orm/libsql`
- Cloudflare D1 → `drizzle-orm/d1`

# Drizzle ORM – PostgreSQL Edition

## Schema & Table Declaration
- Use `pgTable()` from `drizzle-orm/pg-core` to define PostgreSQL tables.
- Explicitly export tables for Drizzle Kit migrations.
- Namespace schemas using `pgSchema()`:

```typescript
export const mySchema = pgSchema("my_schema");
export const users = mySchema.table("users", {
  id: serial("id").primaryKey(),
  name: text("name").notNull(),
});
```

### Column Types (PostgreSQL-specific)
- Integers: `smallint`, `integer`, `bigint`
- Auto-increment: `serial`, `bigserial`, `smallserial`
- Strings: `text`, `varchar('length')`
- Boolean: `boolean('col')`
- JSON: `json`, `jsonb`
- UUIDs: `uuid().defaultRandom()` or use SQL (`gen_random_uuid()`)

## Enums
Define with `pgEnum`:
```typescript
export const statusEnum = pgEnum('status', ['active', 'inactive']);
```

## Constraints
- `.notNull()` prevents NULLs
- `.default(value)` or `.default(sql``)` for expressions
- `.unique()` or `.unique('name')` for unique constraints
- Composite constraints: `unique().on(...)`

## Indexes
Define indexes in table config callback:
```typescript
(table) => ({
  idx: uniqueIndex('name_idx').on(table.name),
});
```

## CRUD Operations

### Insert
Basic insert:
```typescript
await db.insert(users).values({ name: 'John' });
```

Returning inserted rows:
```typescript
const [user] = await db.insert(users).values({ name: "Dan" }).returning();
```

Upserts with conflicts:
```typescript
await db.insert(users)
  .values({ id: 1, name: 'John' })
  .onConflictDoUpdate({
    target: users.id,
    set: { name: 'Johnny' },
  });
```

### Select
Basic:
```typescript
const result = await db.select().from(users);
```

Partial columns:
```typescript
await db.select({ id: users.id, name: users.name }).from(users);
```

SQL expressions:
```typescript
await db.select({
  id: users.id,
  name: sql<string>`lower(${users.name})`,
}).from(users);
```

### Update
Basic update:
```typescript
await db.update(users).set({ name: "Updated" }).where(eq(users.id, 1));
```
Use `.returning()` for updated rows.

### Delete
```typescript
await db.delete(users).where(eq(users.id, 1)).returning();
```

## Joins
```typescript
await db.select().from(users).leftJoin(posts, eq(users.id, posts.userId));
```
Supports `leftJoin`, `rightJoin`, `innerJoin`, `fullJoin`.

## Relations
Define relations with `relations()`:
```typescript
export const userRelations = relations(users, ({ many }) => ({
  posts: many(posts),
}));
```

## Transactions
Basic transaction:
```typescript
await db.transaction(async (tx) => {
  await tx.insert(users).values({ name: 'A' });
  await tx.insert(users).values({ name: 'B' });
});
```
- Nested transactions for savepoints
- Use `await tx.rollback()` for manual rollbacks

## Performance Tips
Prepared statements:
```typescript
const stmt = db.select().from(users).prepare("getUsers");
await stmt.execute();
```

Use `sql.placeholder()` for safe value binding.

## Set Operations
Use `union`, `unionAll`, `intersect`, `except`:
```typescript
const result = await union(
  db.select({ name: users.name }).from(users),
  db.select({ name: customers.name }).from(customers)
);
```

## Filter Operators
- Comparison: `eq`, `ne`, `gt`, `lt`, `gte`, `lte`
- Logical: `and`, `or`, `not`

Example:
```typescript
await db.select().from(users).where(and(eq(users.name, 'Dan'), gt(users.age, 18)));
```

## Query Builder Modes
- Use standard PostgreSQL access via `drizzle-orm/postgres-js`, `neon-http`, or `pg`.

# Drizzle ORM – PostgreSQL Edition

## Schema & Table Declaration
- Use `pgTable()` from `drizzle-orm/pg-core` to define PostgreSQL tables.
- Explicitly export tables for Drizzle Kit migrations.
- Namespace schemas using `pgSchema()`:

```typescript
export const mySchema = pgSchema("my_schema");
export const users = mySchema.table("users", {
  id: serial("id").primaryKey(),
  name: text("name").notNull(),
});
```

### Column Types (PostgreSQL-specific)
- Integers: `smallint`, `integer`, `bigint`
- Auto-increment: `serial`, `bigserial`, `smallserial`
- Strings: `text`, `varchar('length')`
- Boolean: `boolean('col')`
- JSON: `json`, `jsonb`
- UUIDs: `uuid().defaultRandom()` or use SQL (`gen_random_uuid()`)

## Enums
Define with `pgEnum`:
```typescript
export const statusEnum = pgEnum('status', ['active', 'inactive']);
```

## Constraints
- `.notNull()` prevents NULLs
- `.default(value)` or `.default(sql``)` for expressions
- `.unique()` or `.unique('name')` for unique constraints
- Composite constraints: `unique().on(...)`

## Indexes
Define indexes in table config callback:
```typescript
(table) => ({
  idx: uniqueIndex('name_idx').on(table.name),
});
```

## CRUD Operations

### Insert
Basic insert:
```typescript
await db.insert(users).values({ name: 'John' });
```

Returning inserted rows:
```typescript
const [user] = await db.insert(users).values({ name: "Dan" }).returning();
```

Upserts with conflicts:
```typescript
await db.insert(users)
  .values({ id: 1, name: 'John' })
  .onConflictDoUpdate({
    target: users.id,
    set: { name: 'Johnny' },
  });
```

### Select
Basic:
```typescript
const result = await db.select().from(users);
```

Partial columns:
```typescript
await db.select({ id: users.id, name: users.name }).from(users);
```

SQL expressions:
```typescript
await db.select({
  id: users.id,
  name: sql<string>`lower(${users.name})`,
}).from(users);
```

### Update
Basic update:
```typescript
await db.update(users).set({ name: "Updated" }).where(eq(users.id, 1));
```
Use `.returning()` for updated rows.

### Delete
```typescript
await db.delete(users).where(eq(users.id, 1)).returning();
```

## Joins
```typescript
await db.select().from(users).leftJoin(posts, eq(users.id, posts.userId));
```
Supports `leftJoin`, `rightJoin`, `innerJoin`, `fullJoin`.

## Relations
Define relations with `relations()`:
```typescript
export const userRelations = relations(users, ({ many }) => ({
  posts: many(posts),
}));
```

## Transactions
Basic transaction:
```typescript
await db.transaction(async (tx) => {
  await tx.insert(users).values({ name: 'A' });
  await tx.insert(users).values({ name: 'B' });
});
```
- Nested transactions for savepoints
- Use `await tx.rollback()` for manual rollbacks

## Performance Tips
Prepared statements:
```typescript
const stmt = db.select().from(users).prepare("getUsers");
await stmt.execute();
```

Use `sql.placeholder()` for safe value binding.

## Set Operations
Use `union`, `unionAll`, `intersect`, `except`:
```typescript
const result = await union(
  db.select({ name: users.name }).from(users),
  db.select({ name: customers.name }).from(customers)
);
```

## Filter Operators
- Comparison: `eq`, `ne`, `gt`, `lt`, `gte`, `lte`
- Logical: `and`, `or`, `not`

Example:
```typescript
await db.select().from(users).where(and(eq(users.name, 'Dan'), gt(users.age, 18)));
```

## Query Builder Modes
- Use standard PostgreSQL access via `drizzle-orm/postgres-js`, `neon-http`, or `pg`.

## Filter Operators
- Use eq, ne, gt, lt, gte, lte, and, or, not for compound conditions

Example:
```typescript
await db.select().from(users).where(and(eq(users.name, 'Dan'), gt(users.age, 18)));
```
--------------------------------
# Drizzle ORM Rules End  
--------------------------------


--------------------------------
# ElysiaJS Rules Start 
--------------------------------
 
## Key Principles 
- Respond with concise, friendly code snippets in TypeScript. 
- Always format responses for readability. 
- Highlight things like TIP, WARNING, or BEST PRACTICE where relevant.
  
## Development Principles
- Use method chaining to ensure full type propagation across .state(), .get(), .post(), etc.
- Avoid non-chained or split declarations, which break type inference in Eden.
- Prefer TS strict mode and TypeScript >= 5.0 to enable full Eden support.
- Highlight type-safe inputs using t.Object() or Elysia models.
- Promote schema reusability via .model() or shared types.

## Plugin Best Practices
- Use @elysiajs/cors with domain-regex rules for secure cross-origin access.
- Apply @elysiajs/html for JSX templating; ensure JSX config in tsconfig.json.
- Serve static files using @elysiajs/static with a dedicated /public path.
- Track performance with @elysiajs/server-timing — especially useful during development.
- Schedule tasks using @elysiajs/cron; recommend descriptive cron names and error handling.
- Validate Bearer tokens with @elysiajs/bearer, but remind users it does not handle auth logic.
- For real-time data, promote Stream plugin or WebSockets (via Eden).
- Recommend @elysiajs/jwt for authentication, pairing it with Bearer when needed.

## Integration Rules
- For Astro, Next.js, SvelteKit, Expo:
  - Always define an Elysia handler inside the appropriate route file ([...slugs].ts, +server.ts, etc.).
  - Use prefix: '/api' in Elysia if it's mounted in a subpath.
  - Explain WinterCG compliance and when Bun-only features apply.
- Promote co-location of frontend and backend for full-stack workflows.

## Testing & API Clients
- Use Eden Treaty for object-like API access with end-to-end typing.
- Use Eden Fetch for lazy, fast, Fetch-style API calls (better TS performance).
- Prefer bun test for unit/integration testing.
- Show how to export App = typeof app from server for type inference in clients.

## Security & Performance
- Enforce use of safe JSX attributes (safe) to avoid XSS.
- Encourage response schema typing to improve Eden error narrowing.
- For file uploads, show how to use t.Files() in t.Object() with Eden Treaty.
- Use .macro() to define reusable logic like role-based access control.
- Highlight onBeforeHandle, onAfterHandle, onError in macro lifecycles.

## Deployment & Docker
- Prefer oven/bun base image in Docker.
- Mention distroless option for production builds.
- In dev, use docker-compose with volume mounts and bun run --watch.
--------------------------------
# ElysiaJS Rules End 
--------------------------------

--------------------------------
# Svelte 5 and SvelteKit Rules Start 
--------------------------------

# Svelte 5 + SvelteKit Project Setup & Structure Guide (TypeScript)

## Creating a Project

- Use the official SvelteKit CLI powered by Vite:

```bash
bunx sv create my-app
cd my-app
bun install
bun run dev
```

- Select the following options
```bash
◆  Which template would you like?
│  ● SvelteKit minimal (barebones scaffolding for your new app)
│  ○ SvelteKit demo
│  ○ Svelte library
◆  Add type checking with TypeScript?
│  ● Yes, using TypeScript syntax
│  ○ Yes, using JavaScript with JSDoc comments
│  ○ No
◆  What would you like to add to your project? (use arrow keys / space bar)
│  ◼ prettier
│  ◼ eslint
│  ◻ vitest
│  ◻ playwright
│  ◼ tailwindcss (css framework - https://tailwindcss.com)
│  ◻ sveltekit-adapter
│  ◻ drizzle
│  ◻ lucia
│  ◻ mdsvex
◆  tailwindcss: Which plugins would you like to add?
│  ◼ typography
│  ◼ forms (@tailwindcss/forms)
```
> This sets up a full SvelteKit project using Vite, with optional TypeScript, ESLint, Prettier, Playwright, and Vitest integrations. Choose TypeScript when prompted.

## Project Structure

```bash
my-app/
├ test/
│ └ demo.test.ts
├ src/
│ ├ lib/
│ │ ├ server/                 # Server-only code
│ │ └ index.ts               # Entry point for lib exports
│ ├ params/                  # Custom route param matchers
│ ├ routes/                  # App routes and pages
│ │ ├ +page.svelte           # Root page (/)
│ │ ├ +layout.svelte         # Root layout
│ │ ├ +error.svelte          # Error fallback for all routes
│ │ ├ about/
│ │ │ └ +page.svelte         # About page (/about)
│ │ ├ blog/
│ │ │ ├ +page.svelte         # Blog index (/blog)
│ │ │ └ [slug]/
│ │ │   ├ +page.svelte       # Dynamic blog post page (/blog/[slug])
│ │ │   └ +page.server.ts    # Server-side data fetching
│ │ └ api/
│ │   └ hello/
│ │     └ +server.ts         # API endpoint (/api/hello)
│ ├ app.css                  # Global styles
│ ├ app.d.ts                 # Type declarations
│ ├ app.html                 # Template HTML
│ └ demo.spec.ts            # Unit tests
├ static/
│ └ favicon.png             # Static assets
├ .gitignore
├ .npmrc
├ .prettierignore
├ .prettierrc
├ README.md
├ eslint.config.js
├ package.json
├ svelte.config.js
├ tsconfig.json
├ vite.config.ts
```
 

## 📄 Special Files (TypeScript)

### `+page.svelte`
Defines a page component for a specific route.

### `+page.ts` / `+page.server.ts`
`load` function for fetching data either client-side or server-only.

### `+layout.svelte`
Reusable layout for nested pages, must render `{@render children()}`.

### `+error.svelte`
Custom error handler for failed `load()` calls or thrown errors.

### `+server.ts`
Create API endpoints with exported HTTP methods (GET, POST, etc.).

## ⚙ Configuration Highlights

- `svelte.config.js`: Defines the adapter, preprocessors, alias, etc.

```js
// svelte.config.js
import adapter from '@sveltejs/adapter-auto';
import { vitePreprocess } from '@sveltejs/vite-plugin-svelte';
/** @type {import('@sveltejs/kit').Config} */
const config = {
	preprocess: vitePreprocess(),
	kit: {
		adapter: adapter()
	}
};
export default config;
```

- `vite.config.ts`: For advanced Vite setup and plugins.

```ts
// vite.config.ts
import tailwindcss from '@tailwindcss/vite';
import { sveltekit } from '@sveltejs/kit/vite';
import { defineConfig } from 'vite';

export default defineConfig({
	plugins: [tailwindcss(), sveltekit()]
});
```

- `tsconfig.json`: Ensures correct type hints and autocompletion.
```json
{
	"extends": "./.svelte-kit/tsconfig.json",
	"compilerOptions": {
		"allowJs": true,
		"checkJs": true,
		"esModuleInterop": true,
		"forceConsistentCasingInFileNames": true,
		"resolveJsonModule": true,
		"skipLibCheck": true,
		"sourceMap": true,
		"strict": true,
		"moduleResolution": "bundler"
	}
}
```

- `eslint.config.js`: Custom ESLint configuration.
```js
// eslint.config.js
import prettier from 'eslint-config-prettier';
import js from '@eslint/js';
import { includeIgnoreFile } from '@eslint/compat';
import svelte from 'eslint-plugin-svelte';
import globals from 'globals';
import { fileURLToPath } from 'node:url';
import ts from 'typescript-eslint';
import svelteConfig from './svelte.config.js';

const gitignorePath = fileURLToPath(new URL('./.gitignore', import.meta.url));

export default ts.config(
	includeIgnoreFile(gitignorePath),
	js.configs.recommended,
	...ts.configs.recommended,
	...svelte.configs.recommended,
	prettier,
	...svelte.configs.prettier,
	{
		languageOptions: {
			globals: { ...globals.browser, ...globals.node }
		},
		rules: { 'no-undef': 'off' }
	},
	{
		files: ['**/*.svelte', '**/*.svelte.ts', '**/*.svelte.js'],
		ignores: ['eslint.config.js', 'svelte.config.js'],
		languageOptions: {
			parserOptions: {
				projectService: true,
				extraFileExtensions: ['.svelte'],
				parser: ts.parser,
				svelteConfig
			}
		}
	}
);
```

- `.prettierrc`: Custom Prettier configuration.
```json
{
	"useTabs": true,
	"singleQuote": true,
	"trailingComma": "none",
	"printWidth": 100,
	"plugins": ["prettier-plugin-svelte", "prettier-plugin-tailwindcss"],
	"overrides": [
		{
			"files": "*.svelte",
			"options": {
				"parser": "svelte"
			}
		}
	]
}
```


## 🌐 Routing Rules

- Files in `src/routes/` become routes.
- Dynamic params: `[slug]` → `/blog/hello-world`
- Nested layouts: `+layout.svelte` cascades downward.
- Error fallbacks: `+error.svelte`, or `src/error.html`.

## 🔄 Rendering Modes

In `+page(.server).ts` or `+layout(.server).ts`, export flags:

```ts
export const prerender = true; // Static site generation
export const ssr = true;       // Server-side rendering
export const csr = true;       // Client-side rendering
```

##  Development Tips

- Use `$props()` to access props inside components
- Use runes for reactivity:
  - `$state` → for deep reactive state
  - `$derived` → for computed values
  - `$effect` → for side effects
- Use `$lib` to import shared code
- Customize server/client behavior via hooks

## GENERAL RULES 

1. All components must use Svelte 5 Runes API exclusively.
2. Do NOT use deprecated Svelte 4 syntax like:
   - `export let`
   - `$:`
   - `writable`, `readable`
   - `createEventDispatcher`
   - `on:` instead use `on` without the colon
   - `<slot></slot>`instead use snippets and `{@render ...}` tag.
   
3. All JavaScript must be written in TypeScript.
4. Component files must use `.svelte` or `.svelte.ts` extensions.
5. Code must be modular, declarative, and follow idiomatic Svelte 5 practices.
6. Use `on` instead of `on:` for event listeners.
7. Ensure the HTML markup follows accessibility best practices: Do not assign tabindex to noninteractive elements (like <div>, <ul>, <span>, etc.). Only interactive elements (like <button>, <a>, or elements with an appropriate ARIA role) should have nonnegative tabindex values.
8. Always explicitly close non-void HTML elements. Only HTML void elements (img, br, input, hr, link, meta, etc.) should use self-closing tags in Svelte components.

## File Handling
### Component Files
- `.svelte`: Standard Svelte component file. Use `<script>`, `<style>`, and HTML.
- `.svelte.js` / `.svelte.ts`: Logic-only modules with reactive runes, no markup allowed.
- Avoid legacy `.html`-style syntax. Use modern Svelte 5 features.

### Routing Files (SvelteKit)

```txt
src/routes/
├─ +page.svelte           // UI Component
├─ +page.js / .ts         // Shared client+server loader
├─ +page.server.js / .ts  // Server-only loader + actions
├─ +layout.svelte         // Optional shared layout
├─ +error.svelte          // Custom error boundary
├─ +server.js             // API routes
```

- Client code must never import from `lib/server`
- Always colocate loaders and actions with routes

---

## Reactivity — The Runes

Use runes instead of legacy `$:` statements.

### `$state()`

```js
let count = $state(0);
count++;
```

- Supports primitives, objects, arrays
- Objects are deep proxies
- Use `$state.raw()` to disable deep reactivity

### `$derived()`

```js
let doubled = $derived(count * 2);
```

- Avoid side effects inside
- Use `$derived.by()` for multiline logic

### `$effect()`

```js
$effect(() => {
	if (count > 10) {
		console.log("Warning: high count");
	}
});
```
- Runs after render and when dependencies change
- Cleanup with return function

### onMount
- Unlike $effect, the onMount only runs once.

### `$props()`

```js
let { name = 'default', ...rest } = $props();
```

- Use in components instead of `export let`
- Props are reactive and bindable with `$bindable`

### `$bindable()`

```js
let { value } = $bindable();
```

- Enables two-way binding for props
- Syncs parent <-> child

---

## Component Patterns

### Basic

```js
<script>
	let { title } = $props();
</script>

<h1>{title}</h1>
```

### Events

```js
<button onclick={() => dispatch('clicked')}>Click</button>
```

Use `createEventDispatcher` only in `.svelte`, not `.svelte.js`.

### Snippets
- Use snippets and render tags to create reusable chunks of markup inside components.
- Snippets help reduce duplication and enhance maintainability.
```html
{@snippet content()}
```

Used instead of `<slot>` — Svelte 5’s composition primitive.

---

## Modules & Shared Logic

Use `.svelte.js` or `.svelte.ts` files for shared reactive logic.

```js
// counter.svelte.js
let count = $state(0);
export { count };
```

In components:

```js
import { count } from '$lib/counter.svelte.js';
```

---

## Lifecycle Rules

- Use `$effect` for DOM or state-based side effects
- Access DOM with `bind:this`
- Use `onMount()` only for imperative setup (not reactivity)

 
## SvelteKit specific rules Start 
 

### Data Loading

#### Client/Server (shared)

```js
// +page.js
export function load({ fetch, params }) {
	const res = await fetch('/api/data');
	return { stuff: await res.json() };
}
```

#### Server-only

```js
// +page.server.js
export async function load({ params }) {
	const db = await getPost(params.slug);
	return { post: db };
}
```

### Form Actions

```js
export const actions = {
	login: async ({ request }) => {
		const data = await request.formData();
		return { success: !!data.get('email') };
	}
};
```

---

## Testing & Debug

- Use `$inspect()` for runtime state debug.
- Prefer Vitest for unit tests, Playwright for browser automation.
- `@testing-library/svelte` is compatible.

---

## Performance Best Practices

- Use `$state.raw()` for large, immutable objects
- Use `$derived.by()` when computation is expensive
- Avoid deep `$effect()` chains
- Use `<svelte:options immutable />` where possible
 

## STATE MANAGEMENT ==

- Use `$state(...)` for reactive state.
- Use `$state.raw(...)` when working with immutable update patterns.
- Use `$state.snapshot(...)` for compatibility with non-reactive consumers.
- Never mutate raw state directly unless intentionally using `.raw`.

  CORRECT:
    let count = $state(0);
    count++;

  INCORRECT:
    let count = 0;
    $: double = count * 2;

## DERIVED STATE ==

- Use `$derived(...)` for simple computations.
- Use `$derived.by(...)` for more complex logic (e.g. arrays, conditionals).
- Do NOT export derived values directly.

  CORRECT:
    let double = $derived(count * 2);

  INCORRECT:
    $: double = count * 2;

  Instead, expose derived data via a getter:
    export function getDouble() {
      return count * 2;
    }

## SIDE EFFECTS ==

- Use `$effect(...)` for reactive side effects.
- Do not use `$:` or `onMount`.

  CORRECT:
    $effect(() => console.log(count));

## PROPS ==

- Use `$props()` only at the top level of a `.svelte` component.
- Destructure with defaults or renaming.
- Use `$bindable(...)` for two-way bindings.

  CORRECT:
    let { value = 0 } = $props();
    let { model = $bindable("") } = $props();

  INCORRECT:
    export let value;

## EVENTS ==

- Use `on` from `svelte/events` for declarative event handlers.
- Use `$host()` to emit events from the component.

  CORRECT:
    import { on } from 'svelte/events';
    on(button, 'click', doSomething);

    $host().dispatchEvent(new CustomEvent('save', { detail: value }));

## SLOTS / RENDERING ==

- Use `{@render ...}` for content projection.
- Do not use `<slot>` or named slots.

  CORRECT:
    {@render header?.()}
 

## EXPORTING STATE SAFELY ==

- Do not export `$state` or `$derived` directly.
- Always use getter and setter functions.

  CORRECT:
    let counter = $state(0);

    export function getCounter() {
      return counter;
    }

    export function setCounter(val: number) {
      counter = val;
    }

## TESTING AND SYNC ==

- Use `flushSync()` from `svelte` to force updates in testing.

  Example:
    import { flushSync } from 'svelte';
    flushSync(() => count++);

## VALID FILE STRUCTURE ==

- `.svelte` for components.
- `.svelte.ts` for reactive modules (e.g. logic, stores).
- Never use plain `.ts` files for stateful code.

## SVELTE 5 BUILT-IN TYPES ==

- Use `SvelteMap`, `SvelteSet`, `SvelteDate`, `SvelteURL` for reactivity-safe data structures.

  Example:
    import { SvelteURL } from 'svelte/reactivity';
    const url = new SvelteURL("https://example.com");
 
--------------------------------
# Svelte 5 and SvelteKit Rules End 
--------------------------------

--------------------------------
# Styling Rules Start 
--------------------------------
- Always use DaisyUI with Tailwind CSS for styling
- Use DaisyUI component classes for styling
- Avoid custom CSS unless absolutely necessary
- Maintain consistent order of classes
- Customize DaisyUI components only when necessary
  
## SETUP GUIDE
- Install Tailwind CSS v4 and daisyUI 
  ```bash
  bun add -D tailwindcss @tailwindcss/vite daisyui@latest
  ```

- Add vite config
  vite.config.ts
  ```ts
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'
export default defineConfig({
  plugins: [
    tailwindcss(),
  ],
})
  ```
- Add daisyUI to Tailwind as a Tailwind plugin 
  app.css
  ```css
  @import "tailwindcss";
  @plugin "daisyui";
  ``` 
- List of daisyUI themes:  light, dark, cupcake, bumblebee, emerald, corporate, synthwave, retro, cyberpunk, valentine, halloween, garden, forest, aqua, lofi, pastel, fantasy, wireframe, black, luxury, dracula, cmyk, autumn, business, acid, lemonade, night, coffee, winter, dim, nord, sunset, caramellatte, cyberpunk, abyss, silk 
 
- By default,light and dark themes are enabled.
- To enable other themes, add them to the daisyUI config as below:
  ```css
  @plugin "daisyui" {
    themes: light --default, dark --prefersdark, corporate;
  }
```
And set corporate theme for the page by adding the following to the index.html file:
```html
<html data-theme="corporate"></html>
```
- To add a new theme, use @plugin "daisyui/theme" {} in app.css, with the following structure:
```css
@import "tailwindcss";
@plugin "daisyui";
@plugin "daisyui/theme" {
  name: "mytheme";
  default: true; /* set as default */
  prefersdark: false; /* set as default dark mode (prefers-color-scheme:dark) */
  color-scheme: light; /* color of browser-provided UI */

  --color-base-100: oklch(98% 0.02 240);
  --color-base-200: oklch(95% 0.03 240);
  --color-base-300: oklch(92% 0.04 240);
  --color-base-content: oklch(20% 0.05 240);
  --color-primary: oklch(55% 0.3 240);
  --color-primary-content: oklch(98% 0.01 240);
  --color-secondary: oklch(70% 0.25 200);
  --color-secondary-content: oklch(98% 0.01 200);
  --color-accent: oklch(65% 0.25 160);
  --color-accent-content: oklch(98% 0.01 160);
  --color-neutral: oklch(50% 0.05 240);
  --color-neutral-content: oklch(98% 0.01 240);
  --color-info: oklch(70% 0.2 220);
  --color-info-content: oklch(98% 0.01 220);
  --color-success: oklch(65% 0.25 140);
  --color-success-content: oklch(98% 0.01 140);
  --color-warning: oklch(80% 0.25 80);
  --color-warning-content: oklch(20% 0.05 80);
  --color-error: oklch(65% 0.3 30);
  --color-error-content: oklch(98% 0.01 30);

  /* border radius */
  --radius-selector: 1rem;
  --radius-field: 0.25rem;
  --radius-box: 0.5rem;

  /* base sizes */
  --size-selector: 0.25rem;
  --size-field: 0.25rem;

  /* border size */
  --border: 1px;

  /* effects */
  --depth: 1;
  --noise: 0;
}
```
## EXTENDING & CUSTOMIZING daisyUI
- To customize a built-in theme, you can use the same structure as adding a new theme, but with the same name as the built-in theme. For example, to customize the light theme add the following to app.css:
```css
@import "tailwindcss";
@plugin "daisyui";
@plugin "daisyui/theme" {
  name: "light";
  default: true;
  --color-primary: blue;
  --color-secondary: teal;
}
```
- To add custom styles for a specific theme, add the following to app.css:
```css
[data-theme="light"] {
  .my-btn {
    background-color: #1EA1F1;
    border-color: #1EA1F1;
  }
  .my-btn:hover {
    background-color: #1C96E1;
    border-color: #1C96E1;
  }
}
```
- To apply Tailwind's 'dark:' selector for specific themes use @custom-variant as below:
```css
@import "tailwindcss";
@plugin "daisyui" {
  themes: winter --default, night --prefersdark;
}
@custom-variant dark (&:where([data-theme=night], [data-theme=night] *));
```
then use the dark variant as below:
```html
<div class="p-10 dark:p-20">
  I will have 10 padding on winter theme and 20 padding on night theme
</div>
```

- daisyUI base style
  - properties	For necessary at-rules, like variable type for--radialprogress
  - rootcolor	For:rootand[data-theme]it sets background-color tobase-100and text color tobase-content
  - scrollbar	Sets scrollbar-color for:root
  - rootscrolllock	Sets:roottooverflow: hiddenwhen modal or drawer is open
  - rootscrollgutter	Adds ascrollbar-gutterto:rootwhen modal or drawer is open, to avoid layout shift
  - svg	Contains small SVG images for noise filter, chat bubble tail mask, and tooltip tail mask. Can be disabled to use custom images.

To opt out of each part use exclude property as below:
```css
@plugin "daisyui" {
  exclude: rootscrollgutter, rootcolor;
}
```

## daisyUI Color utility classes
  - bg-primary	Sets the background color to the primary color
  - to-primary	Sets the ending color for a gradient to the primary color
  - via-primary	Sets the middle color for a gradient to the primary color
  - from-primary	Sets the starting color for a gradient to the primary color
  - text-primary	Sets the text color to the primary color
  - ring-primary	Sets the ring color to the primary color
  - fill-primary	Sets the fill color for SVG elements to the primary color
  - caret-primary	Sets the caret color to the primary color
  - stroke-primary	Sets the stroke color for SVG elements to the primary color
  - border-primary	Sets the border color to the primary color
  - divide-primary	Sets the color for dividing borders to the primary color
  - accent-primary	Sets the accent color to the primary color
  - shadow-primary	Sets the shadow color to the primary color
  - outline-primary	Sets the outline color to the primary color
  - decoration-primary	Sets the text decoration color to the primary color
  - placeholder-primary	Sets the placeholder text color to the primary color
  - ring-offset-primary	Sets the ring offset color to the primary color

## daisyUI Border radius classes
  - rounded-box	var(--radius-box)	For large sized components like card, modal, alert, etc.
  - rounded-field	var(--radius-field)	For medium sized components like button, input, select, tab, etc.
  - rounded-selector	var(--radius-selector)	For small sized components like checkbox, toggle, badge, etc.

## daisyUI Theme CSS variables
  --color-primary	Primary brand color
  --color-primary-content	Foreground content color to use on primary color
  --color-secondary	Secondary brand color
  --color-secondary-content	Foreground content color to use on secondary color
  --color-accent	Accent brand color
  --color-accent-content	Foreground content color to use on accent color
  --color-neutral	Neutral dark color
  --color-neutral-content	Foreground content color to use on neutral color
  --color-base-100	Base color of page, used for blank backgrounds
  --color-base-200	Base color, darker shade
  --color-base-300	Base color, even more darker shade
  --color-base-content	Foreground content color to use on base color
  --color-info	Info color
  --color-info-content	Foreground content color to use on info color
  --color-success	Success color
  --color-success-content	Foreground content color to use on success color
  --color-warning	Warning color
  --color-warning-content	Foreground content color to use on warning color
  --color-error	Error color
  --color-error-content	Foreground content color to use on error color
  --radius-selector	Border radius for selectors like checkbox, toggle, badge, etc
  --radius-field	Border radius for fields like input, select, tab, etc
  --radius-box	Border radius for boxes like card, modal, alert, etc
  --size-selector	Base scale size for selectors like checkbox, toggle, badge, etc
  --size-field	Base scale size for fields like input, select, tab, etc
  --border	Border width of all components
  --depth	(binary) Adds a depth effect for relevant components
  --noise	(binary) Adds a background noise effect for relevant components

## daisyUI Component specific CSS variables
  Alert	--alert-color	color of the alert
  Badge	--badge-color	color of the badge
  --size	size of the badge
  Button	--btn-color	background color of the button
  --btn-fg	foreground color of the button
  --btn-shadow	shadow of the button
  --btn-noise	noise background of the button if enabled
  --btn-p	padding of the button
  --size	size of the button
  Card	--card-p	padding of the card body
  --card-fs	font size of the card body
  --cardtitle-fs	font size of the card title
  Checkbox	--size	size of the checkbox
  Countdown	--value	value of the countdown
  Divider	--divider-m	margin of the divider
  Dropdown	--anchor-v	vertical position of the anchor
  --anchor-h	horizontal position of the anchor
  File input	--input-color	color of the file input
  --size	size of the file input
  Indicator	--indicator-t	top position of the indicator
  --indicator-b	bottom position of the indicator
  --indicator-s	start position of the indicator
  --indicator-e	end position of the indicator
  --indicator-y	vertical position of the indicator
  --indicator-x	horizontal position of the indicator
  Input	--input-color	color of the input
  --size	size of the input
  Kbd	--size	size of the kbd
  List	--list-grid-cols	grid columns of the list
  Menu	--menu-active-fg	foreground color of the active menu item
  --menu-active-bg	background color of the active menu item
  Modal	--modal-tl	top left border radius of the modal
  --modal-tr	top right border radius of the modal
  --modal-bl	bottom left border radius of the modal
  --modal-br	bottom right border radius of the modal
  Radial progress	--value	value of the radial progress
  --size	size of the radial progress
  --thickness	thickness of the radial progress
  --radialprogress	for calculating the radial progress position
  Radio	--size	size of the radio
  Range	--range-bg	background color of the range slider thumb
  --range-thumb	color of the range slider thumb
  --range-thumb-size	size of the range slider thumb
  --range-progress	color of the range slider progress
  --range-fill	binary, whether to fill the range slider progress or not
  --range-p	padding of the range slider thumb
  --size	size of the range slider
  Select	--input-color	color of the input
  --size	size of the select
  Tab	--tabs-height	height of the tabs
  --tabs-direction	direction of the tabs
  --tab-p	padding of the tab
  --tab-bg	background color of the tab
  --tab-border-color	border color of the tab
  --tab-radius-ss	start start border radius of the tab
  --tab-radius-se	start end border radius of the tab
  --tab-radius-es	end start border radius of the tab
  --tab-radius-ee	end end border radius of the tab
  --tab-order	Order of the tab
  --tab-radius-min	minimum border radius of the lift tab
  --tab-paddings	all padding values of the lift tab
  --tab-border-colors	all border color values of the lift tab
  --tab-corner-width	corner width of the lift tab
  --tab-corner-height	corner height of the lift tab
  --tab-corner-width	corner width of the lift tab
  --tab-corner-position	corner position of the lift tab
  --tab-inset	inset position of the lift tab
  --radius-start	border radius for the corner of the lift tab
  --radius-end	border radius for the corner of the lift tab
  --tabcontent-margin	margin of the tab content
  --tabcontent-radius-ss	start start border radius of the tab content
  --tabcontent-radius-se	start end border radius of the tab content
  --tabcontent-radius-es	end start border radius of the tab content
  --tabcontent-radius-ee	end end border radius of the tab content
  --tabcontent-order	order of the tab content
  Textarea	--input-color	color of the input
  --size	size of the textarea
  Timeline	--timeline-row-start	start position of the timeline row
  --timeline-row-end	end position of the timeline row
  --timeline-col-start	start position of the timeline column
  --timeline-col-end	end position of the timeline column
  Toast	--toast-x	horizontal position of the toast
  --toast-y	vertical position of the toast
  Toggle	--toggle-p	padding of the toggle
  --size	size of the toggle
  Tooltip	--tt-bg	background color of the tooltip
  --tt-off	offset of the tooltip
  --tt-tailw	position of the tooltip tail
  Glass	--glass-blur	blur of the glass effect
  --glass-opacity	opacity of the glass effect
  --glass-reflect-degree	degree of the glass reflection
  --glass-reflect-opacity	opacity of the glass reflection
  --glass-border-opacity	opacity of the glass border
  --glass-text-shadow-opacity	opacity of the glass text shadow
  Join	--join-ss	start start border radius of the join
  --join-se	start end border radius of the join
  --join-es	end start border radius of the join
  --join-ee	end end border radius of the join

## Layout: Layout, sizing, grids, spacing, etc. all will be handled by Tailwind CSS's utility classes.
## Typography: daisyUI supports theTailwindCSS Typography plugin. All parts are compatible with daisyUI themes.

## GENERAL RULES
- Always use DaisyUI components when applicable (e.g., `btn`, `card`, `modal`, etc.) over building raw Tailwind UI unless explicitly instructed otherwise.
- Follow Tailwind utility-first approach with DaisyUI for spacing, layout, and responsiveness.
- Use DaisyUI's theme tokens for colors and avoid hardcoding HEX or RGB values unless customizing themes.
- Prefer semantic HTML elements where possible (`<button>`, `<form>`, `<input>`, `<section>`, etc.).
- Avoid repeating utility classes when DaisyUI provides a utility shortcut (e.g., use `btn` instead of manually applying padding, border, etc.).
- Always close all HTML tags and ensure accessibility attributes (e.g., `aria-*`, `role`, `tabindex`) are considered when needed.

## THEMING & COLOR USAGE
- Use DaisyUI theme color tokens such as:
  - `bg-primary`, `text-primary-content`
  - `bg-secondary`, `text-secondary-content`
  - `bg-accent`, `text-accent-content`
  - `bg-base-100`, `text-base-content`
  - `bg-info`, `bg-success`, `bg-warning`, `bg-error`
- When changing themes dynamically, use `data-theme="themeName"` on a wrapper like `<html>` or `<body>`.

## COMPONENTS (Grouped)

### BUTTONS
- Classes: `btn`, `btn-primary`, `btn-secondary`, `btn-accent`, `btn-info`, `btn-success`, `btn-warning`, `btn-error`, `btn-outline`, `btn-link`, `btn-ghost`, `btn-active`, `btn-disabled`, `btn-sm`, `btn-md`, `btn-lg`, `btn-block`, `btn-circle`, `btn-square`
- Always use `<button>` element or `<a>` with `role="button"` if necessary.

### FORM ELEMENTS
- Inputs: `input`, `input-bordered`, `input-primary`, `input-secondary`, `input-accent`, `input-disabled`, `input-sm`, `input-lg`
- Textarea: `textarea`, same modifiers as input
- Checkbox/Radio: `checkbox`, `radio`, `toggle`
- Select: `select`, `select-bordered`, `select-primary`, etc.
- Use `form-control` wrapper for structured layout
- Include `label` with `for` attribute tied to `input id`

### MODALS
- Use: `modal`, `modal-box`, `modal-action`, `modal-toggle`
- Modal controlled via checkbox method or JS toggle

### TABS
- Classes: `tabs`, `tab`, `tab-active`, `tab-bordered`, `tab-lifted`, `tab-sm`, `tab-lg`

### ALERTS / BADGES / CHIPS
- Alerts: `alert`, `alert-info`, `alert-success`, `alert-warning`, `alert-error`
- Badges: `badge`, `badge-primary`, `badge-secondary`, etc.
- Use icons from Heroicons or another SVG source for contextual alerts

### ACCORDIONS / COLLAPSE
- Class: `collapse`, `collapse-title`, `collapse-content`
- Optional: `collapse-arrow`, `collapse-open`

### CARDS
- Classes: `card`, `card-body`, `card-title`, `card-actions`, `card-bordered`, `card-compact`
- Structure with image: use `figure` for media

### BREADCRUMBS
- Use `breadcrumbs` wrapper and `li > a` structure

### DRAWER / SIDEBAR
- Classes: `drawer`, `drawer-toggle`, `drawer-content`, `drawer-side`, `drawer-overlay`

### DROPDOWN
- Classes: `dropdown`, `dropdown-content`, `dropdown-end`, `dropdown-hover`

### TABLE
- Use `table`, `table-zebra`, `table-auto`, `table-fixed`, etc.
- Wrap in `overflow-x-auto` for responsive

### STEPPER / PROGRESS
- `step`, `steps`, `step-primary`, `progress`, `progress-primary`, `radial-progress`

### OTHER UTILITIES
- `stats`, `toast`, `tooltip`, `avatar`, `indicator`, `kbd`, `loading`, `skeleton`, `timeline`, `mockup-phone`, `mockup-window`

---

## LAYOUT & RESPONSIVENESS
- Use Tailwind utility classes (`grid`, `flex`, `w-`, `h-`, `gap-`, `p-`, `m-`, `space-`, `max-w-`, `min-h-`, etc.)
- Use responsive prefixes (`sm:`, `md:`, `lg:`, `xl:`, `2xl:`)
- Combine DaisyUI and Tailwind for full layout control
  - Example:
    ```html
    <div class="card md:w-96 w-full bg-base-100 shadow-xl">
      ...
    </div>
    ```
 
## BEST PRACTICES

- Use DaisyUI classes first, Tailwind utilities second
- Stick to semantic HTML and accessibility
- Extract reusable components if repetition appears
- Ensure mobile-first responsiveness
- Use `data-theme` for theme switching
- Keep forms inside `form-control` containers
- Prefer icon SVGs via `<svg>` over icon fonts
- Use DaisyUI's modal, toast, and drawer components with `checkbox`-based toggling where possible to avoid JS
 

## AVOID
- Hardcoding styles when DaisyUI provides utility
- Nesting DaisyUI components incorrectly (e.g., `btn` inside `btn`)
- Using DaisyUI components without understanding underlying HTML roles
- Mixing Bootstrap or other frameworks with DaisyUI


## DaisyUI Components with Usage Examples Compatible with Tailwind CSS 
 
# Layout Components

## 1. Artboard
```html
<div class="artboard artboard-demo phone-1">Content</div>
```

## 2. Button Group
```html
<div class="btn-group">
  <button class="btn">Left</button>
  <button class="btn">Middle</button>
  <button class="btn">Right</button>
</div>
```

## 3. Card
```html
<div class="card w-96 bg-base-100 shadow-xl">
  <figure><img src="img.jpg" alt="Image"/></figure>
  <div class="card-body">
    <h2 class="card-title">Title</h2>
    <p>Description</p>
    <div class="card-actions justify-end">
      <button class="btn btn-primary">Action</button>
    </div>
  </div>
</div>
```

## 4. Carousel
```html
<div class="carousel w-full">
  <div class="carousel-item w-full">
    <img src="img1.jpg" class="w-full" />
  </div>
</div>
```

## 5. Collapse
```html
<div class="collapse">
  <input type="checkbox" />
  <div class="collapse-title">Click me</div>
  <div class="collapse-content">Hidden content</div>
</div>
```

## 6. Divider
```html
<div class="divider">OR</div>
```

## 7. Drawer
```html
<div class="drawer">
  <input id="drawer-toggle" type="checkbox" class="drawer-toggle" />
  <div class="drawer-content">Main content</div>
  <div class="drawer-side">
    <label for="drawer-toggle" class="drawer-overlay"></label>
    <ul class="menu p-4 w-80 bg-base-100 text-base-content">
      <li><a>Item 1</a></li>
    </ul>
  </div>
</div>
```

## 8. Footer
```html
<footer class="footer p-10 bg-neutral text-neutral-content">
  <div><p>Company © 2025</p></div>
</footer>
```

## 9. Hero
```html
<div class="hero min-h-screen bg-base-200">
  <div class="hero-content text-center">
    <div class="max-w-md">
      <h1 class="text-5xl font-bold">Hello there</h1>
    </div>
  </div>
</div>
```

## 10. Indicator
```html
<div class="indicator">
  <span class="indicator-item badge badge-secondary">New</span>
  <button class="btn">Inbox</button>
</div>
```

## 11. Join
```html
<div class="join">
  <input class="input input-bordered join-item" />
  <button class="btn join-item">Go</button>
</div>
```

## 12. Mask
```html
<div class="mask mask-squircle bg-primary w-24 h-24"></div>
```

## 13. Stack
```html
<div class="stack">
  <div class="btn">First</div>
  <div class="btn">Second</div>
</div>
```

## 14. Stat
```html
<div class="stats shadow">
  <div class="stat">
    <div class="stat-title">Downloads</div>
    <div class="stat-value">31K</div>
  </div>
</div>
```

## 15. Toast
```html
<div class="toast">
  <div class="alert alert-info">
    <span>New message arrived.</span>
  </div>
</div>
```

---

# Navigation Components

## 16. Breadcrumbs
```html
<div class="breadcrumbs">
  <ul>
    <li><a>Home</a></li>
    <li><a>Docs</a></li>
  </ul>
</div>
```

## 17. Bottom Navigation
```html
<div class="btm-nav">
  <button class="active">Home</button>
</div>
```

## 18. Link
```html
<a class="link link-primary">Clickable link</a>
```

## 19. Menu
```html
<ul class="menu bg-base-200 w-56 rounded-box">
  <li><a>Dashboard</a></li>
</ul>
```

## 20. Navbar
```html
<div class="navbar bg-base-100">
  <div class="navbar-start">
    <a class="btn btn-ghost text-xl">Logo</a>
  </div>
</div>
```

## 21. Pagination
```html
<div class="join">
  <button class="join-item btn">Prev</button>
  <button class="join-item btn">1</button>
  <button class="join-item btn">Next</button>
</div>
```

## 22. Steps
```html
<ul class="steps">
  <li class="step step-primary">Step 1</li>
  <li class="step">Step 2</li>
</ul>
```

## 23. Tab
```html
<div class="tabs">
  <a class="tab tab-active">Tab 1</a>
  <a class="tab">Tab 2</a>
</div>
```

---

# Form Components

## 24. Checkbox
```html
<input type="checkbox" class="checkbox" />
```

## 25. File Input
```html
<input type="file" class="file-input file-input-bordered" />
```

## 26. Input
```html
<input type="text" placeholder="Type here" class="input input-bordered" />
```

## 27. Radio
```html
<input type="radio" name="radio-1" class="radio" />
```

## 28. Range
```html
<input type="range" min="0" max="100" class="range" />
```

## 29. Rating
```html
<div class="rating">
  <input type="radio" name="rating" class="mask mask-star" />
</div>
```

## 30. Select
```html
<select class="select select-bordered">
  <option>Option 1</option>
</select>
```

## 31. Textarea
```html
<textarea class="textarea textarea-bordered" placeholder="Comment"></textarea>
```

## 32. Toggle
```html
<input type="checkbox" class="toggle" />
```

## 33. Form Control
```html
<div class="form-control">
  <label class="label">
    <span class="label-text">Name</span>
  </label>
  <input type="text" class="input input-bordered" />
</div>
```

## 34. Input Group
```html
<label class="input-group">
  <span>https://</span>
  <input type="text" class="input input-bordered" />
</label>
```

## 35. Label
```html
<label class="label">
  <span class="label-text">Username</span>
</label>
```

---

# Data Display Components

## 36. Avatar
```html
<div class="avatar">
  <div class="w-24 rounded-full">
    <img src="avatar.jpg" />
  </div>
</div>
```

## 37. Badge
```html
<span class="badge">NEW</span>
```

## 38. Chat
```html
<div class="chat chat-start">
  <div class="chat-bubble">Hello!</div>
</div>
```

## 39. Countdown
```html
<span class="countdown font-mono text-2xl">
  <span style="--value:10;"></span>
</span>
```

## 40. Kbd
```html
<kbd class="kbd">Ctrl</kbd>
```

## 41. Table
```html
<table class="table">
  <thead><tr><th>Name</th></tr></thead>
  <tbody><tr><td>John</td></tr></tbody>
</table>
```

## 42. Timeline
```html
<ul class="timeline">
  <li><div class="timeline-start">2023</div></li>
</ul>
```

## 43. Tooltip
```html
<div class="tooltip" data-tip="Tooltip text">
  <button class="btn">Hover me</button>
</div>
```

---

# Feedback Components

## 44. Alert
```html
<div class="alert alert-error">
  <span>Error!</span>
</div>
```

## 45. Loading
```html
<span class="loading loading-spinner"></span>
```

## 46. Modal
```html
<input type="checkbox" id="my_modal" class="modal-toggle" />
<div class="modal">
  <div class="modal-box">
    <h3 class="font-bold text-lg">Hello!</h3>
    <div class="modal-action">
      <label for="my_modal" class="btn">Close</label>
    </div>
  </div>
</div>
```

## 47. Popover
```html
<div class="popover">
  <div class="popover-trigger">
    <button class="btn">Info</button>
  </div>
  <div class="popover-content">Popover content</div>
</div>
```

## 48. Progress
```html
<progress class="progress w-56" value="70" max="100"></progress>
```

## 49. Skeleton
```html
<div class="skeleton h-4 w-24"></div>
```

## 50. Swap
```html
<label class="swap">
  <input type="checkbox" />
  <div class="swap-on">ON</div>
  <div class="swap-off">OFF</div>
</label>
```

---

# Utility Components

## 51. Backdrop
```html
<div class="backdrop backdrop-blur-sm"></div>
```

## 52. Code
```html
<code class="bg-base-200 p-1 rounded">npm install</code>
```

## 53. Container
```html
<div class="container mx-auto">Centered content</div>
```

## 54. Glass
```html
<div class="glass p-4">Glass effect</div>
```

## 55. Overlay
```html
<div class="fixed inset-0 bg-black bg-opacity-50"></div>
```

## 56. Prose (Typography)
```html
<article class="prose">
  <h1>Heading</h1>
  <p>Paragraph text</p>
</article>
```

## 57. Scrollspy
```html
<ul class="scrollspy">
  <li><a href="#item1">Item 1</a></li>
</ul>
```

## 58. Swap (Duplicate component)
```html
<label class="swap">
  <input type="checkbox" />
  <div class="swap-on">ON</div>
  <div class="swap-off">OFF</div>
</label>
```

## 59. Theme Controller
```html
<select data-choose-theme class="select">
  <option value="light">Light</option>
  <option value="dark">Dark</option>
</select>
```

## 60. Window
```html
<div class="mockup-window border bg-base-300">
  <div class="flex justify-center px-4 py-16">Content</div>
</div>
```

## 61. Mockup (Browser / Code / Phone / Window)
```html
<div class="mockup-browser border bg-base-300">
  <div class="mockup-browser-toolbar">
    <div class="input">https://example.com</div>
  </div>
  <div class="px-4 py-16">Website Preview</div>
</div>
```
 

# Tailwind CSS 4.0 Rules

## General Guidelines
- Use `@import "tailwindcss";` at the top of all theme and utility files.
- Prefer `@theme` for defining design tokens instead of `theme.extend` in JS.
- Always generate `@theme` variables using Tailwind’s official namespace conventions.
- Avoid JavaScript-based configuration like `tailwind.config.js`.

---

## Theme Token Rules

### Format for Tokens
```css
@theme {
  --color-primary: oklch(0.72 0.15 210);
  --font-body: "Inter", sans-serif;
  --text-lg: 1.125rem;
  --text-lg--line-height: calc(1.75 / 1.125);
}
```

### Accepted Namespaces
| Namespace             | Example                      | Class Generated       |
|-----------------------|------------------------------|------------------------|
| `--color-*`           | `--color-indigo-500`         | `bg-indigo-500`       |
| `--font-*`            | `--font-body`                | `font-body`           |
| `--text-*`            | `--text-xl`                  | `text-xl`             |
| `--spacing-*`         | `--spacing-8`                | `mt-8`, `px-8`        |
| `--breakpoint-*`      | `--breakpoint-lg`            | `lg:*`                |
| `--radius-*`          | `--radius-md`                | `rounded-md`          |
| `--shadow-*`          | `--shadow-md`                | `shadow-md`           |
| `--animate-*`         | `--animate-fade`             | `animate-fade`        |

> When generating theme variables, only use the above namespaces.
 

## Utility Generation Rules

### Dynamic Utilities
Use `calc(var(--spacing) * N)` for spacing-related utilities:
```css
@layer utilities {
  .pt-10 { padding-top: calc(var(--spacing) * 10); }
  .w-96 { width: calc(var(--spacing) * 96); }
}
```

### Color with Opacity
```css
.bg-primary\/50 {
  background-color: color-mix(in oklab, var(--color-primary) 50%, transparent);
}
```

### Data Attribute Variants
```html
<div data-current class="opacity-75 data-current:opacity-100"></div>
```

---

## 🧩 Custom Utilities

Define in `@layer utilities`, optionally grouped by feature.

```css
@layer utilities {
  .border-thick {
    border-width: 4px;
  }

  .glow {
    box-shadow: 0 0 12px var(--color-primary);
  }
}
```

---

## 🔁 Reusable Patterns

### Responsive Container Query
```html
<div class="@container">
  <div class="grid @sm:grid-cols-2 @lg:grid-cols-4">...</div>
</div>
```

### Gradient Utilities
```html
<div class="bg-linear-45 from-purple-500 to-pink-500"></div>
<div class="bg-radial-[at_25%_25%] from-white to-black"></div>
```

---
## AI Prompt Hints
- Output CSS-first config (no JS).
- Use `@theme` for all tokens.
- Default to `oklch()` for color.
- Reference variables via `var(--token)` in utility output.
- Don't include any Tailwind plugin config or `tailwind.config.ts`.

---

## ✅ Example Prompt to AI

> "Generate a Tailwind CSS v4.0 CSS-first theme using @theme. Include a primary color, text sizes, and spacing scale using `--spacing`."
 
## File Output Recommendation

| File                     | Purpose                     |
|--------------------------|-----------------------------|
| `app.css`                | Main entry point            |
| `theme.css`              | All `@theme` tokens         |
| `utilities.css`          | Custom utility classes      |
 

 
--------------------------------
# Styling Rules End 
--------------------------------

--------------------------------
# ICONs Rules Start 
--------------------------------
- use iconify for tailwind with iconify lucide icons package
- install the following iconify packages 
```bash
bun add -D @iconify-json/lucide @iconify/tailwind4 ,
```
- add this to app.css
```css
@plugin "@iconify/tailwind4" {
    prefix: "iconify";
    prefixes: lucide;
  }
```
- usage in svelte
```svelte
<span class="iconify lucide--sun"></span>
```
--------------------------------
# ICON Preffered Rules End 
--------------------------------

--------------------------------
# TESTING UNIT RULES Start 
--------------------------------

 
- Use `bun:test` to define and run unit and integration tests in Bun. It’s Bun’s built-in test runner — no need for external libraries like Jest or Mocha.

---

## Structure

Each test file should follow this pattern:

```ts
// Import test helpers from bun
import { describe, it, expect, beforeAll, afterAll } from 'bun:test'

// Define test suite
describe('Feature or Module Name', () => {
  beforeAll(() => {
    // Setup logic (optional)
  })

  it('should do something', () => {
    // Assertion
    expect(true).toBe(true)
  })

  afterAll(() => {
    // Cleanup logic (optional)
  })
})
```

---

## File Naming

- Test files should end with `.test.ts`, `.test.js`, `.spec.ts`, or `.spec.js`
- Place them in a `/test` directory or alongside the file being tested.

---

## Run Tests

```bash
bun test
```

- Runs all matching test files recursively.
- To target a specific test file:
  ```bash
  bun test test/example.test.ts
  ```

---

## Assertions

Use `expect()` for various assertion types:

```ts
expect(2 + 2).toBe(4)
expect('hello world').toContain('world')
expect([1, 2, 3]).toEqual([1, 2, 3])
expect(typeof 'abc').toBe('string')
```

---

## Async Tests

```ts
it('handles async logic', async () => {
  const response = await fetch('https://api.example.com')
  const result = await response.json()

  expect(response.status).toBe(200)
  expect(result).toHaveProperty('data')
})
```

---

## Pro Tips

- Keep tests short and focused on one behavior.
- Use `beforeAll`, `beforeEach`, `afterAll`, `afterEach` for setup/teardown.
- Avoid global side effects; keep tests isolated.
- Prefer `.test.ts` for clarity and editor integration.

```ts
// Example of setup per test
beforeEach(() => {
  // Reset state or create mock data
})
```
--------------------------------
# TESTING UNIT RULES End 
--------------------------------

--------------------------------
# Postgres Rules Start 
--------------------------------
 
This reference covers:
  ✔️ Cursor declarations and usage patterns
  ✔️ Scrollability, holdability, parameters
  ✔️ Refcursors and dynamic SQL
  ✔️ Transactional behavior
  ✔️ Performance caveats and tuning


## SECTION 1: BASIC CURSOR DECLARATION


-- Declare a forward-only, read-only cursor
DECLARE my_cursor CURSOR FOR
SELECT column1, column2
FROM some_table
WHERE status = 'active';
-- Parameterized cursor inside a function
DECLARE user_cursor CURSOR (user_id INT) FOR
SELECT * FROM users WHERE id = user_id;
-- Scrollable cursor for directional access
DECLARE scroll_cursor SCROLL CURSOR FOR
SELECT * FROM logs ORDER BY created_at;
-- Cursor WITH HOLD to survive COMMIT (beware: loses snapshot!)
DECLARE my_cursor CURSOR WITH HOLD FOR
SELECT * FROM long_running_view;

## SECTION 2: CURSOR CONTROL FLOW


-- OPEN a regular cursor
OPEN my_cursor;
-- OPEN a parameterized cursor
OPEN user_cursor(42);
-- FETCH syntax
FETCH NEXT FROM my_cursor INTO var1, var2;
FETCH PRIOR FROM my_cursor INTO ...;
FETCH FIRST FROM my_cursor INTO ...;
FETCH LAST FROM my_cursor INTO ...;
FETCH RELATIVE 5 FROM my_cursor INTO ...;
FETCH ABSOLUTE 10 FROM my_cursor INTO ...;
-- Use a loop to process each row
LOOP
    FETCH my_cursor INTO rec_var;
    EXIT WHEN NOT FOUND;
    -- process rec_var
END LOOP;
-- CLOSE the cursor explicitly
CLOSE my_cursor;

## SECTION 3: USING FOR LOOPS (IMPLICIT CURSORS)

-- No DECLARE or OPEN needed
FOR row_var IN
    SELECT id, email FROM users WHERE active = true
LOOP
    RAISE NOTICE 'User %: %', row_var.id, row_var.email;
END LOOP;

## SECTION 4: REFCURSOR (GENERIC CURSOR TYPE)

-- Declare a REFCURSOR variable
DECLARE refcur REFCURSOR;
DECLARE result_row RECORD;
-- Assign a name and open the cursor dynamically
OPEN refcur FOR
SELECT * FROM products WHERE price > 100;
-- Use FETCH with REFCURSOR
FETCH NEXT FROM refcur INTO result_row;
-- Return REFCURSOR from a function
CREATE OR REPLACE FUNCTION get_big_orders()
RETURNS REFCURSOR AS $$
DECLARE
    c REFCURSOR;
BEGIN
    OPEN c FOR SELECT * FROM orders WHERE total > 1000;
    RETURN c;
END;
$$ LANGUAGE plpgsql;

## SECTION 5: CURSOR WITH DYNAMIC SQL


-- Use EXECUTE with dynamic SQL and REFCURSOR
DECLARE dyn_query TEXT;
DECLARE dyn_cursor REFCURSOR;

BEGIN
    dyn_query := 'SELECT * FROM ' || quote_ident(some_table_name);
    OPEN dyn_cursor FOR EXECUTE dyn_query;
    LOOP
        FETCH dyn_cursor INTO row_data;
        EXIT WHEN NOT FOUND;
        -- Process row_data
    END LOOP;
    CLOSE dyn_cursor;
END;

## SECTION 6: CURSOR BEHAVIOR AND TRANSACTIONS

-- Regular cursors are bound to the current transaction
-- They are CLOSED automatically on COMMIT or ROLLBACK

-- Cursors WITH HOLD stay open after COMMIT, but:
-- they operate outside of snapshot — beware of consistency issues
-- they do NOT survive ROLLBACK

-- Declaring cursors in autocommit mode (psql) won't work
-- Always wrap in BEGIN ... COMMIT or use DO blocks


## SECTION 7: PERFORMANCE + TUNING TIPS

-- Avoid large FETCH counts if you don't need all rows
-- Consider LIMIT + OFFSET or keyset pagination for faster loads
-- Use cursors when:
  - Need row-by-row logic
  - Processing many rows in server memory
--  Avoid for small result sets or bulk operations — slower than bulk SQL
--  Each FETCH incurs overhead; batch in chunks if needed


## SECTION 8: ERROR HANDLING + SAFETY

BEGIN
    OPEN my_cursor;
    LOOP
        FETCH my_cursor INTO some_var;
        EXIT WHEN NOT FOUND;
        -- work
    END LOOP;
    CLOSE my_cursor;
EXCEPTION
    WHEN OTHERS THEN
        -- log and clean up
        IF FOUND THEN
            CLOSE my_cursor;
        END IF;
        RAISE NOTICE 'Error: %', SQLERRM;
END;

## SECTION 9: CURSOR + JSON RETURN PATTERN (EXAMPLE)

CREATE OR REPLACE FUNCTION stream_data_json()
RETURNS SETOF JSON AS $$
DECLARE
    r RECORD;
    cur CURSOR FOR SELECT * FROM data_stream;
BEGIN
    OPEN cur;
    LOOP
        FETCH cur INTO r;
        EXIT WHEN NOT FOUND;
        RETURN NEXT to_json(r);
    END LOOP;
    CLOSE cur;
END;
$$ LANGUAGE plpgsql;
--------------------------------
# Postgres Rules End 
--------------------------------