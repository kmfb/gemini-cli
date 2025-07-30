# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Build and Development
- `npm run build` - Build the project
- `npm run build:all` - Build all components (main, sandbox, vscode)
- `npm run build:packages` - Build workspace packages
- `npm run bundle` - Create bundled distribution
- `npm start` - Start the CLI in development mode
- `npm run debug` - Start with Node debugger

### Testing
- `npm test` - Run tests for all workspace packages
- `npm run test:ci` - Run CI test suite
- `npm run test:e2e` - Run end-to-end tests
- `npm run test:integration:all` - Run all integration tests
- `npm run test:integration:sandbox:none` - Integration tests without sandbox
- `npm run test:integration:sandbox:docker` - Integration tests with Docker sandbox
- `npm run test:integration:sandbox:podman` - Integration tests with Podman sandbox

### Code Quality
- `npm run lint` - Run ESLint
- `npm run lint:fix` - Auto-fix linting issues
- `npm run lint:ci` - Run linting with zero warnings tolerance
- `npm run format` - Format code with Prettier
- `npm run typecheck` - Run TypeScript type checking
- `npm run preflight` - Complete validation pipeline (clean, install, format, lint, build, typecheck, test)

### Other Commands
- `npm run clean` - Clean build artifacts
- `npm run generate` - Generate git commit info

## Architecture Overview

The Gemini CLI is built as a monorepo with a modular architecture:

### Core Packages
- **`packages/cli/`** - User-facing CLI interface, input processing, display rendering, themes, and configuration
- **`packages/core/`** - Backend engine handling Gemini API communication, prompt construction, tool orchestration, and session management
- **`packages/vscode-ide-companion/`** - VS Code extension integration

### Tools System
Tools are located in `packages/core/src/tools/` and extend the CLI's capabilities:
- File system operations
- Shell command execution
- Web fetching and search
- Memory management
- Multi-file operations

### Interaction Flow
1. User input → CLI package
2. CLI → Core package (with conversation history and tool definitions)
3. Core → Gemini API
4. API response → Tool execution (with user approval for destructive operations)
5. Tool results → API → Core → CLI → User display

## Development Guidelines

### Testing Framework
- Uses **Vitest** as the primary testing framework
- Test files co-located with source files (`*.test.ts`, `*.test.tsx`)
- Mock with `vi.mock()` using `importOriginal` for selective mocking
- Place critical dependency mocks (os, fs) at the top of test files
- Use `vi.hoisted()` for functions needed in mock factories

### TypeScript/JavaScript Standards
- Prefer plain objects with TypeScript interfaces over classes
- Use ES module syntax for encapsulation (import/export)
- Avoid `any` types; prefer `unknown` with type narrowing
- Minimize type assertions (`as Type`)
- Leverage functional array operators (map, filter, reduce)

### React Guidelines (for CLI UI components using Ink)
- Use functional components with Hooks only
- Keep components pure during rendering
- Never mutate state directly; use immutable updates
- Follow Rules of Hooks (top-level, unconditional calls)
- Minimize useEffect usage; prefer event handlers for user actions
- Rely on React Compiler for optimization (avoid manual memoization)

### Code Style
- Use hyphens in flag names (not underscores)
- Minimize comments; write self-documenting code
- Follow existing patterns and conventions in the codebase

## Project Structure
- Main branch: `main`
- Monorepo with npm workspaces
- Node.js 20+ required
- Uses ESM modules throughout
- Built with esbuild for bundling