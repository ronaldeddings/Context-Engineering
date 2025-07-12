# TypeScript Claude Code & MCP Integration Manual
*A comprehensive guide for context engineering with Claude Code, focusing on TypeScript development, MCP servers, and intelligent automation*

## Table of Contents
1. [Executive Summary](#executive-summary)
2. [TypeScript Project Structure for Agentic Coding](#typescript-project-structure)
3. [Hooks Configuration with MCP Integration](#hooks-configuration)
4. [Essential MCP Servers for TypeScript](#essential-mcp-servers)
5. [Headless Claude Code Automation](#headless-automation)
6. [Complete Integration Example](#complete-example)
7. [Best Practices & Troubleshooting](#best-practices)

---

## Executive Summary {#executive-summary}

This manual provides a production-ready approach to context engineering with Claude Code for TypeScript projects. It covers:
- **Optimal project structure** that maximizes Claude's understanding
- **Intelligent hooks** that integrate with MCP servers before code generation
- **Essential MCP servers** for TypeScript development
- **Headless automation** patterns for CI/CD integration

### Key Principle: Context Before Code
> "LLMs don't know everything" - Use MCP servers to provide real-time context before Claude writes code.

---

## TypeScript Project Structure for Agentic Coding {#typescript-project-structure}

### 1. Directory Structure

```
your-project/
├── .claude/                    # Claude-specific configuration
│   ├── settings.toml          # Hooks configuration
│   ├── commands/              # Custom slash commands
│   └── mcp-config.json        # MCP server configurations
├── .mcp.json                  # Project-level MCP config (version controlled)
├── CLAUDE.md                  # Project context document
├── src/
│   ├── core/                  # Core business logic
│   │   ├── types/            # Shared TypeScript types
│   │   ├── utils/            # Utility functions
│   │   └── constants/        # Constants and enums
│   ├── services/             # Service layer
│   ├── api/                  # API endpoints
│   └── tests/                # Test files
├── scripts/                   # Build and automation scripts
├── docs/                      # Documentation
├── tsconfig.json             # TypeScript configuration
├── package.json              # Dependencies and scripts
└── .env.example              # Environment variables template
```

### 2. Essential CLAUDE.md Template

```markdown
# CLAUDE.md - Project Context

## Project Overview
**Name**: [Your Project Name]
**Type**: TypeScript Monorepo / Single Package
**Purpose**: [Brief description]

## Technology Stack
- **Runtime**: Node.js 20.x LTS
- **Language**: TypeScript 5.x (strict mode)
- **Package Manager**: pnpm / yarn / npm
- **Testing**: Jest / Vitest
- **Linting**: ESLint + Prettier
- **Build Tool**: esbuild / tsc / webpack

## Critical Commands
```bash
# Development
pnpm dev              # Start development server
pnpm build            # Build for production
pnpm test             # Run all tests
pnpm test:watch       # Run tests in watch mode
pnpm lint             # Lint and format code
pnpm typecheck        # Run TypeScript compiler
pnpm validate         # Run all checks (lint + test + typecheck)

# Code Quality (These run automatically via hooks!)
pnpm format           # Format with Prettier
pnpm lint:fix         # Fix linting issues
```

## Architecture Patterns
1. **Dependency Injection**: Use tsyringe for IoC
2. **Error Handling**: Custom error classes extending BaseError
3. **Validation**: Zod schemas for runtime validation
4. **Logging**: Structured logging with pino
5. **API Design**: RESTful with OpenAPI documentation

## Project Conventions
- **File Naming**: kebab-case for files, PascalCase for components
- **Imports**: Absolute imports via @/ alias
- **Types**: Colocate types with implementation
- **Tests**: *.test.ts files adjacent to source
- **Async**: Always use async/await, no raw promises
- **Comments**: JSDoc for public APIs only

## MCP Context Sources
The following MCP servers provide real-time context:
- **context7**: Latest library documentation
- **github**: Repository issues and PRs
- **database**: Current schema and migrations
- **npm-registry**: Package version information

## Known Issues / Gotchas
- [List any project-specific quirks]
- [Environment-specific considerations]
- [Third-party API limitations]
```

### 3. TypeScript Configuration

```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "lib": ["ES2022"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "allowJs": false,
    "noEmit": false,
    "incremental": true,
    "tsBuildInfoFile": ".tsbuildinfo",
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedIndexedAccess": true,
    "allowSyntheticDefaultImports": true,
    "paths": {
      "@/*": ["./src/*"],
      "@core/*": ["./src/core/*"],
      "@services/*": ["./src/services/*"],
      "@api/*": ["./src/api/*"],
      "@types/*": ["./src/core/types/*"]
    }
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "**/*.test.ts"]
}
```

---

## Hooks Configuration with MCP Integration {#hooks-configuration}

### Core Concept: Context-First Development
Before Claude writes any code, hooks should gather context from MCP servers to ensure accuracy and relevance.

### 1. Master Hooks Configuration

```toml
# .claude/settings.toml

# Pre-code generation: Fetch context from MCP servers
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.ts", "*.tsx"]
command = """
# Fetch latest TypeScript/library docs before editing
claude-code -p "Use Context7 to get the latest documentation for the libraries imported in $CLAUDE_FILE_PATHS" --headless

# Check for existing patterns in codebase
grep -r "similar patterns" src/ || true
"""

# Post-edit: Validate and format
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.ts", "*.tsx"]
command = """
# Type checking first
npx tsc --noEmit --pretty

# Linting and formatting
npx eslint --fix $CLAUDE_FILE_PATHS
npx prettier --write $CLAUDE_FILE_PATHS

# Run affected tests
npx jest --findRelatedTests $CLAUDE_FILE_PATHS --passWithNoTests
"""

# Pre-commit validation
[[hooks]]
event = "Stop"
command = """
# Full validation before any commit
if git diff --cached --name-only | grep -q '\\.ts'; then
  echo "🔍 Running pre-commit validation..."
  pnpm validate
fi
"""

# Smart documentation updates
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["src/**/*.ts"]
run_in_background = true
command = """
# Extract and update API documentation
npx typedoc --json .claude/api-docs.json
claude-code -p "Update the API section in CLAUDE.md based on .claude/api-docs.json" --headless
"""

# MCP-powered code review
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "create_file"
command = """
# Check if similar files exist
echo "🔍 Checking for similar implementations..."
claude-code -p "Use github MCP to search for similar file patterns in this repo" --headless

# Get best practices from Context7
claude-code -p "Use context7 to get best practices for the file type being created" --headless
"""
```

### 2. MCP Integration Hooks

```toml
# Advanced MCP-powered hooks

# Database schema validation
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["**/models/*.ts", "**/entities/*.ts"]
command = """
# Fetch current database schema
claude-code -p "Use database MCP to get current schema" --output-format json > .claude/current-schema.json

# Validate against schema
node scripts/validate-schema.js $CLAUDE_FILE_PATHS .claude/current-schema.json || exit 2
"""

# API compatibility check
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["**/api/**/*.ts"]
command = """
# Check API breaking changes
claude-code -p "Use openapi MCP to validate API compatibility" --headless
"""

# Security scanning
[[hooks]]
event = "PostToolUse"
run_in_background = true
command = """
# Run security checks in background
npm audit
npx snyk test
"""
```

---

## Essential MCP Servers for TypeScript {#essential-mcp-servers}

### 1. Context7 - Real-time Documentation
```json
{
  "context7": {
    "command": "npx",
    "args": ["-y", "@upstash/context7-mcp"],
    "description": "Provides up-to-date documentation for any library"
  }
}
```

**Use Cases**:
- Get latest React hooks documentation
- TypeScript utility types reference
- Node.js API updates
- Framework-specific patterns

### 2. GitHub MCP - Repository Intelligence
```json
{
  "github": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-github"],
    "env": {
      "GITHUB_TOKEN": "${GITHUB_TOKEN}"
    },
    "description": "Access issues, PRs, and code search"
  }
}
```

**Use Cases**:
- Search for similar implementations
- Check related issues before coding
- Review PR feedback patterns
- Access team coding standards

### 3. Database MCP - Schema Awareness
```json
{
  "database": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-postgres"],
    "env": {
      "DATABASE_URL": "${DATABASE_URL}"
    },
    "description": "Live database schema and queries"
  }
}
```

**Use Cases**:
- Validate model definitions
- Generate type-safe queries
- Check migration compatibility
- Optimize query performance

### 4. NPM Registry MCP - Package Intelligence
```json
{
  "npm-registry": {
    "command": "npx",
    "args": ["-y", "npm-registry-mcp"],
    "description": "Package versions and compatibility"
  }
}
```

**Use Cases**:
- Check latest package versions
- Verify peer dependencies
- Find type definitions
- Security vulnerability checks

### 5. OpenAPI MCP - API Contract Management
```json
{
  "openapi": {
    "command": "npx",
    "args": ["-y", "openapi-mcp-server"],
    "config": {
      "specPath": "./openapi.yaml"
    },
    "description": "API specification validation"
  }
}
```

### 6. Playwright MCP - Browser Testing
```json
{
  "playwright": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-playwright"],
    "description": "Browser automation and testing"
  }
}
```

### 7. Filesystem MCP - Enhanced File Operations
```json
{
  "filesystem": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-filesystem"],
    "config": {
      "allowedPaths": ["./src", "./tests", "./docs"]
    },
    "description": "Advanced file operations"
  }
}
```

### Complete .mcp.json Configuration
```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "database": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    },
    "npm-registry": {
      "command": "npx",
      "args": ["-y", "npm-registry-mcp"]
    },
    "openapi": {
      "command": "npx",
      "args": ["-y", "openapi-mcp-server"],
      "config": {
        "specPath": "./openapi.yaml"
      }
    }
  }
}
```

---

## Headless Claude Code Automation {#headless-automation}

### 1. Basic Headless Usage
```bash
# Simple headless command
claude-code -p "Fix all TypeScript errors" --headless

# With JSON output for parsing
claude-code -p "List all TODO comments" --output-format json --headless

# With specific context
claude-code -p "Update the user service to use dependency injection" \
  --context "Use tsyringe for DI implementation" \
  --headless
```

### 2. CI/CD Integration

```yaml
# .github/workflows/claude-assist.yml
name: Claude Code Assistance

on:
  pull_request:
    types: [opened, edited]
  issues:
    types: [opened]

jobs:
  auto-assist:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Claude Code
        run: |
          npm install -g @anthropic/claude-code
          claude-code configure --api-key ${{ secrets.CLAUDE_API_KEY }}
      
      - name: Auto-generate implementation plan
        if: github.event_name == 'issues'
        run: |
          claude-code -p "Based on issue #${{ github.event.issue.number }}, create an implementation plan" \
            --output-format markdown > plan.md
          
      - name: Comment on issue
        uses: actions/github-script@v6
        with:
          script: |
            const plan = require('fs').readFileSync('plan.md', 'utf8');
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: plan
            });
```

### 3. Headless Hook Patterns

```bash
#!/bin/bash
# scripts/smart-refactor.sh

# Use Claude headlessly to analyze before refactoring
ANALYSIS=$(claude-code -p "Analyze src/ for refactoring opportunities" \
  --output-format json --headless)

# Parse analysis and create tasks
echo "$ANALYSIS" | jq -r '.refactoring_tasks[]' | while read task; do
  claude-code -p "Refactor: $task" --headless
  
  # Run tests after each refactor
  npm test || {
    echo "Tests failed after refactoring: $task"
    git checkout -- .
    exit 1
  }
  
  git add -A
  git commit -m "Refactor: $task"
done
```

### 4. MCP + Headless Workflows

```bash
# Pre-code context gathering
claude-code -p "Use context7 to get React 18 concurrent features docs" --headless > .claude/react-docs.md
claude-code -p "Use github to find similar component implementations" --headless > .claude/similar-components.md

# Now generate code with full context
claude-code -p "Implement a concurrent-mode compatible data fetching component using the context from .claude/"
```

---

## Complete Integration Example {#complete-example}

### Project Setup Script
```bash
#!/bin/bash
# setup-claude-typescript-project.sh

PROJECT_NAME=$1

# Create project structure
mkdir -p $PROJECT_NAME/{src/{core/{types,utils,constants},services,api,tests},scripts,docs,.claude/commands}
cd $PROJECT_NAME

# Initialize package.json
cat > package.json << 'EOF'
{
  "name": "@your-org/your-project",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc && tsc-alias",
    "test": "jest",
    "test:watch": "jest --watch",
    "lint": "eslint . --ext .ts,.tsx",
    "lint:fix": "eslint . --ext .ts,.tsx --fix",
    "format": "prettier --write .",
    "typecheck": "tsc --noEmit",
    "validate": "npm run typecheck && npm run lint && npm run test"
  },
  "devDependencies": {
    "@types/node": "^20.0.0",
    "typescript": "^5.0.0",
    "tsx": "^4.0.0",
    "jest": "^29.0.0",
    "@types/jest": "^29.0.0",
    "ts-jest": "^29.0.0",
    "eslint": "^8.0.0",
    "@typescript-eslint/parser": "^6.0.0",
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "prettier": "^3.0.0",
    "tsc-alias": "^1.8.0"
  }
}
EOF

# Create tsconfig.json
cat > tsconfig.json << 'EOF'
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "lib": ["ES2022"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "paths": {
      "@/*": ["./src/*"],
      "@core/*": ["./src/core/*"],
      "@services/*": ["./src/services/*"],
      "@api/*": ["./src/api/*"]
    }
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "**/*.test.ts"]
}
EOF

# Create CLAUDE.md
cat > CLAUDE.md << 'EOF'
# CLAUDE.md - TypeScript Project Context

## Project Overview
**Name**: TypeScript Claude Code Project
**Purpose**: Modern TypeScript application with full Claude Code integration

## Technology Stack
- Node.js 20.x LTS
- TypeScript 5.x (strict mode)
- Jest for testing
- ESLint + Prettier
- MCP servers for context

## Commands
\`\`\`bash
npm run dev       # Start development
npm run build     # Build project
npm run test      # Run tests
npm run validate  # Run all checks
\`\`\`

## MCP Servers Available
- context7: Documentation lookup
- github: Repository intelligence
- database: Schema validation
- npm-registry: Package info
EOF

# Create .mcp.json
cat > .mcp.json << 'EOF'
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
EOF

# Create hooks configuration
mkdir -p .claude
cat > .claude/settings.toml << 'EOF'
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.ts", "*.tsx"]
command = "claude-code -p 'Use context7 to check latest TypeScript patterns' --headless"

[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.ts", "*.tsx"]
command = "npx tsc --noEmit && npx eslint --fix $CLAUDE_FILE_PATHS"
EOF

# Create a sample custom command
cat > .claude/commands/create-service.md << 'EOF'
---
name: create-service
description: Create a new service with full test coverage
---

Create a new TypeScript service with the following:
1. Use dependency injection pattern
2. Include comprehensive error handling
3. Add full test coverage
4. Create interface first, then implementation
5. Add JSDoc documentation
6. Follow project conventions in CLAUDE.md

Service name: $ARGUMENTS
EOF

# Initialize git
git init
echo "node_modules/\ndist/\n.env\n.env.local" > .gitignore

# Install dependencies
npm install

echo "✅ Project setup complete! Run 'claude-code' to start coding."
```

---

## Best Practices & Troubleshooting {#best-practices}

### Best Practices

1. **Context Layering**
   - Use MCP servers for real-time data
   - CLAUDE.md for static project knowledge
   - Hooks for automation
   - Custom commands for workflows

2. **Hook Design Principles**
   - Fast operations in foreground hooks
   - Heavy operations with `run_in_background = true`
   - Use exit code 2 to block dangerous operations
   - Always validate inputs

3. **MCP Server Selection**
   - Start with Context7 + GitHub
   - Add database MCP when working with models
   - Use filesystem MCP for complex file operations
   - Keep server list minimal for performance

4. **Headless Automation**
   - Use JSON output for parsing
   - Chain commands with proper error handling
   - Store context in files between commands
   - Implement retry logic for flaky operations

### Common Issues & Solutions

**Issue**: Hooks running too slowly
```toml
# Solution: Use background execution
[[hooks]]
event = "PostToolUse"
run_in_background = true
command = "npm test"
```

**Issue**: MCP server connection failures
```json
// Solution: Add retry configuration
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"],
      "retries": 3,
      "timeout": 30000
    }
  }
}
```

**Issue**: Context overflow
```bash
# Solution: Use targeted context
claude-code -p "Focus only on src/services/user.service.ts" \
  --exclude "node_modules,dist,tests" \
  --max-files 10
```

### Performance Optimization

1. **Selective Hook Activation**
```toml
# Only run expensive hooks on specific files
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
file_paths = ["src/core/**/*.ts"]  # Not all TS files
command = "npm run expensive-validation"
```

2. **MCP Server Caching**
```bash
# Cache MCP responses
claude-code -p "Use context7 to get React docs" --cache-duration 3600
```

3. **Parallel Operations**
```bash
# Run multiple headless commands in parallel
{
  claude-code -p "Analyze performance" --headless > perf.json &
  claude-code -p "Check security" --headless > security.json &
  wait
}
```

---

## Conclusion

This manual provides a comprehensive approach to context engineering with Claude Code for TypeScript projects. The key insight is using MCP servers to provide real-time context *before* code generation, combined with intelligent hooks for automation.

Remember: **Context is everything**. The better you provide context through MCP servers, CLAUDE.md, and hooks, the better Claude Code performs.

### Quick Start Checklist
- [ ] Create optimal project structure
- [ ] Set up CLAUDE.md with project context
- [ ] Configure .mcp.json with essential servers
- [ ] Create .claude/settings.toml with smart hooks
- [ ] Test headless commands for automation
- [ ] Implement custom commands for workflows

*Last updated: January 2025*