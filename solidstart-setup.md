---
canonical_url: https://zanca.dev/blog/solidstart-setup
daily_note: "[[2025-12-30]]"
timestamp: 2025-12-30T19:40:21
tags:
  - solidjs
  - solidstart
date: 2025-12-30T20:31:39
publish: true
---

> As of writing this, running `bun create solid` doesn't use the latest version, so we'll use `bunx create-solid@latest`.

In this quick guide I'll setup my preferred way to use solidjs/solidstart. Preferring to use a guide such that I don't have to maintain a template over time.

## Starting with the drizzle template

```bash
bunx create-solid@latest -s --ts -t="with-drizzle"
```

> `-s` to make this a solidstart project, `--ts` to use typescript, `-t="with-drizzle"` to use the drizzle template

Update the `.gitignore` to ignore some files.

```bash
# Other lock files (we're using bun)
package-lock.json
yarn.lock
pnpm-lock.yaml

# sqlite file
drizzle/db.sqlite
```

Add this convenience dev script to the `package.json`. This will wipe the db, push the schema and start the dev server.

```diff
"scripts": {
+  "dev:fresh": "rm drizzle/db.sqlite && drizzle-kit push && vinxi dev
}
```

## Change the Drizzle driver to `libsql`

We're swapping out drizzle's built in `better-sqlite3` driver, which requires a custom dockerfile when deploying to railway for `libsql` from turso which is pure javascript. No idea what the trade-offs are but this makes my side projects easier to deploy.

```bash
bun rm @types/better-sqlite3
bun add @libsql/client
```

```diff
// drizzle.config.ts
export default {
  dialect: "sqlite",
  schema: "./drizzle/schema.ts",
  out: "./drizzle/migrations/",
- // driver: "better-sqlite",
  dbCredentials: {
-   url: './drizzle/db.sqlite',
+   url: "file:./drizzle/db.sqlite",
  },
};
```

```ts
// src/api/db.ts
import { drizzle } from "drizzle-orm/libsql";
import { createClient } from "@libsql/client";
import { dirname, resolve } from "path";
import { existsSync, mkdirSync } from "fs";

// Use environment variable for database path, or resolve relative to project root
const dbPath = process.env.DATABASE_PATH
  ? resolve(process.env.DATABASE_PATH)
  : resolve(process.cwd(), "drizzle", "db.sqlite");
const dbDir = dirname(dbPath);

// Ensure the directory exists before creating the client
if (!existsSync(dbDir)) {
  mkdirSync(dbDir, { recursive: true });
}

const client = createClient({
  url: `file:${dbPath}`,
});

export const db = drizzle(client);
```

## Add tailwind

```bash
bun add solid-motionone @tailwindcss/vite
bun add -D tailwindcss
```

```ts
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./src/**/*.{ts,tsx}",
  ],
  darkMode: 'class',
  theme: {
    extend: {},
  },
  plugins: [],
}
```

```diff
# app.config.ts
import { defineConfig } from "@solidjs/start/config";
+import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  vite: {
    ssr: { external: ["drizzle-orm"] },
+    plugins: [tailwindcss()],
  },
});
```

## Notes

The server entry file `src/entry-server.tsx` is a good place to start background tasks or server setup tasks. In dev, this runs once a route is hit but in prod this runs on server startup.
