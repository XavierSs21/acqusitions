# Acquisitions API

A secure authentication and user management API built with Node.js, Express, and PostgreSQL.

## Commands

### Development

- `npm run dev` - Start development server with file watching using Node.js --watch
- `npm run lint` - Run ESLint on the entire codebase
- `npm run lint:fix` - Run ESLint with automatic fixes
- `npm run format` - Format code with Prettier
- `npm run format:check` - Check code formatting with Prettier

### Database (Drizzle ORM)

- `npm run db:generate` - Generate database migrations from schema
- `npm run db:migrate` - Apply pending database migrations
- `npm run db:studio` - Open Drizzle Studio for database management

### Server

- Entry point: `src/index.js` (loads environment and starts server)
- Development server runs on http://localhost:3000 (configurable via PORT env var)
- Health check endpoint: `/health`

## Architecture

### Tech Stack

- **Runtime**: Node.js with ES modules (`"type": "module"`)
- **Framework**: Express.js 5.x with modern security middleware (helmet, cors)
- **Database**: PostgreSQL with Neon serverless and Drizzle ORM
- **Authentication**: JWT tokens with bcrypt password hashing
- **Validation**: Zod schemas with custom error formatting
- **Logging**: Winston with file and console transports
- **Code Quality**: ESLint + Prettier with unix line endings

### Project Structure

Uses import alias mapping defined in package.json for clean imports:

- `#config/*` - Database, logger, and other configuration
- `#controllers/*` - Request handlers and business logic
- `#models/*` - Drizzle ORM database schemas
- `#routes/*` - Express route definitions
- `#services/*` - Business logic and data operations
- `#utils/*` - Utility functions (JWT, cookies, formatting)
- `#validations/*` - Zod validation schemas

### Key Architecture Patterns

**Layered Architecture**: Routes → Controllers → Services → Models

- Routes define endpoints and delegate to controllers
- Controllers handle request/response and validation
- Services contain business logic and database operations
- Models define database schemas using Drizzle ORM

**Error Handling**: Structured error responses with logging

- Validation errors formatted via `formatValidationError` utility
- Service-level errors logged and re-thrown with context
- JWT operations centralized in `#utils/jwt.js`

**Database**: Drizzle ORM with Neon PostgreSQL

- Schema files in `src/models/` (currently only `users` table)
- Database connection configured in `src/config/database.js`
- Migration files generated in `./drizzle/` directory

**Authentication Flow**:

- User registration via `/api/auth/sign-up` with validation and JWT token generation
- JWT tokens stored in HTTP-only cookies via `cookies.set()` utility
- Password hashing using bcrypt with salt rounds of 12

### Environment Configuration

Copy `.env.example` to `.env` and configure:

- `PORT` - Server port (default: 3000)
- `NODE_ENV` - Environment (development/production)
- `LOG_LEVEL` - Winston log level (default: info)
- `DATABASE_URL` - Neon PostgreSQL connection string
- `JWT_SECRET` - JWT signing secret (change in production)

### Current State

This is an early-stage authentication API with:

- User registration endpoint implemented
- Sign-in and sign-out routes stubbed but not implemented
- Basic user model with roles (user/admin)
- Logging configured for development and production
- Database migration system set up but no initial migrations created
