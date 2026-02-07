---
name: backend-development
description: Backend platform patterns, conventions, and validation commands for the Gambling Golfer Express/TypeScript API. Use this when writing, reviewing, or testing backend code.
---

# Backend Development — Gambling Golfer

## Stack

- **Runtime**: Node.js 20+ with TypeScript (`strict: true`)
- **Framework**: Express
- **Database**: PostgreSQL via `pg`
- **Auth**: JWT access + refresh tokens, `bcrypt` for password hashing
- **Testing**: Vitest (unit), Supertest (HTTP)
- **Linting**: ESLint with TypeScript rules
- **Formatting**: Prettier
- **Validation**: Zod schemas

## Project Structure

```
backend/
├── src/
│   ├── config/           # Environment config (Zod-validated)
│   ├── db/               # Migrations, repositories
│   ├── middleware/        # Express middleware
│   ├── routes/           # API route handlers (by resource)
│   ├── types/            # TypeScript type definitions
│   ├── utils/            # Response helpers, utilities
│   ├── app.ts            # Express app factory
│   └── index.ts          # Server entrypoint
├── tests/                # Test files (mirrors src/)
└── dist/                 # Compiled JS (not committed)
```

## TypeScript Conventions

### Strict Typing
```typescript
// ✅ Explicit return types, no `any`
function getUser(id: string): Promise<User | null> {
    return db.query('SELECT * FROM users WHERE id = $1', [id]);
}

// ❌ Never use `any`
function process(data: any) { }
```

### Import Ordering
```typescript
// 1. Builtin modules
import { randomBytes } from 'crypto';
// 2. External packages
import express from 'express';
import { z } from 'zod';
// 3. Internal modules (with .js extension)
import { createApp } from './app.js';
```

### Unused Parameters
```typescript
app.use((_req, res, next) => { next(); });
```

## Implementation Checklist

When writing backend code, ensure:

- [ ] ES module imports use `.js` extensions
- [ ] Imports ordered: builtin → external → internal
- [ ] Zod schemas used for request validation
- [ ] Database queries use parameterized `$1` placeholders
- [ ] Response helpers used for consistent API responses
- [ ] `@openapi` JSDoc annotations added for new/modified endpoints
- [ ] Explicit return types on all functions
- [ ] No `any` type anywhere
- [ ] Environment config via Zod-validated config, not raw `process.env`

## API Response Format

```typescript
// Success
{ "success": true, "data": { ... }, "meta": { "timestamp": "..." } }

// Error
{ "success": false, "error": { "code": "ERROR_CODE", "message": "..." }, "meta": { "timestamp": "..." } }
```

Use response helpers from `src/utils/response.ts`.

## Testing Patterns

```typescript
// HTTP endpoint tests
import request from 'supertest';
import { describe, it, expect } from 'vitest';
import { createApp } from '../src/app.js';

const app = createApp();

describe('GET /api/endpoint', () => {
    it('returns expected response', async () => {
        const res = await request(app).get('/api/endpoint').expect(200);
        expect(res.body.success).toBe(true);
    });
});
```

## Validation Commands

Run these before committing (in order):

```bash
npm run format          # Format with Prettier
npm run lint:fix        # Auto-fix ESLint issues
npm run validate        # Full check: typecheck → lint → format:check → test
```

## Security Requirements

- Parameterized queries only — no SQL string concatenation
- Zod validation on all request inputs
- JWT tokens with short expiry (15m access, 7d refresh)
- `helmet` middleware for security headers
- Secrets in `.env` (never committed)

## API Documentation

- **Swagger UI**: `http://localhost:3000/api-docs` (source of truth)
- **Raw OpenAPI JSON**: `http://localhost:3000/api-docs.json`
- All endpoint changes must update `@openapi` JSDoc annotations
