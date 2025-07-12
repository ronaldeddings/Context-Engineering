# Next.js 15 Claude Code & MCP Integration Manual
*A comprehensive guide for context engineering with Claude Code, focusing on Next.js 15 App Router development, MCP servers, and intelligent automation*

## Table of Contents
1. [Executive Summary](#executive-summary)
2. [Next.js 15 Project Structure for Agentic Coding](#nextjs-project-structure)
3. [Hooks Configuration with MCP Integration](#hooks-configuration)
4. [Essential MCP Servers for Next.js](#essential-mcp-servers)
5. [Headless Claude Code Automation](#headless-automation)
6. [Complete Integration Example](#complete-example)
7. [Best Practices & Troubleshooting](#best-practices)

---

## Executive Summary {#executive-summary}

This manual provides a production-ready approach to context engineering with Claude Code for Next.js 15 projects. It covers:
- **Optimal Next.js App Router structure** that maximizes Claude's understanding of server/client components
- **Intelligent hooks** that integrate with MCP servers before code generation
- **Essential MCP servers** for Next.js full-stack development
- **Headless automation** patterns for CI/CD integration with Vercel and GitHub

### Key Principle: Context Before Code
> "LLMs don't know everything" - Use MCP servers to provide real-time context before Claude writes code.

---

## Next.js 15 Project Structure for Agentic Coding {#nextjs-project-structure}

### 1. Directory Structure

```
nextjs-project/
├── .claude/                    # Claude-specific configuration
│   ├── settings.toml          # Hooks configuration
│   ├── commands/              # Custom slash commands
│   └── mcp-config.json        # MCP server configurations
├── .mcp.json                  # Project-level MCP config (version controlled)
├── CLAUDE.md                  # Project context document
├── app/                       # Next.js 15 App Router
│   ├── (auth)/               # Grouped routes requiring auth
│   │   ├── dashboard/
│   │   └── profile/
│   ├── (public)/             # Public routes
│   │   ├── page.tsx
│   │   └── about/
│   ├── api/                  # API routes
│   │   └── [...]/route.ts
│   ├── layout.tsx            # Root layout
│   ├── error.tsx             # Error boundary
│   └── global-error.tsx      # Global error boundary
├── components/                # React components
│   ├── ui/                   # UI components (buttons, cards, etc.)
│   ├── features/             # Feature-specific components
│   └── providers/            # Context providers
├── lib/                      # Utility functions and helpers
│   ├── actions/             # Server actions
│   ├── db/                  # Database utilities
│   └── utils/               # General utilities
├── hooks/                    # Custom React hooks
├── types/                    # TypeScript type definitions
├── styles/                   # Global styles and themes
├── public/                   # Static assets
├── middleware.ts             # Next.js middleware
├── next.config.ts            # Next.js configuration (TypeScript)
├── tsconfig.json             # TypeScript configuration
├── tailwind.config.ts        # Tailwind configuration
└── .env.local                # Environment variables
```

### 2. Essential CLAUDE.md Template

```markdown
# CLAUDE.md - Next.js 15 Project Context

## Project Overview
**Name**: [Your Project Name]
**Type**: Next.js 15 App Router with TypeScript
**Purpose**: [Brief description]
**Deployment**: Vercel / Self-hosted

## Technology Stack
- **Framework**: Next.js 15.x (App Router)
- **Language**: TypeScript 5.x (strict mode)
- **Styling**: Tailwind CSS / CSS Modules
- **Database**: Prisma / Drizzle with PostgreSQL
- **Authentication**: NextAuth.js / Clerk
- **State Management**: Zustand / TanStack Query
- **Testing**: Jest + React Testing Library + Playwright
- **Package Manager**: pnpm (recommended) / npm / yarn

## Critical Commands
```bash
# Development
pnpm dev              # Start Next.js dev server (port 3000)
pnpm build            # Production build
pnpm start            # Start production server
pnpm lint             # Run ESLint
pnpm typecheck        # Run TypeScript compiler
pnpm test             # Run unit tests
pnpm test:e2e         # Run Playwright E2E tests
pnpm db:push          # Push schema changes to database
pnpm db:studio        # Open Prisma Studio

# Code Quality (These run automatically via hooks!)
pnpm format           # Format with Prettier
pnpm lint:fix         # Fix linting issues
```

## Next.js 15 Architecture

### Server Components vs Client Components
- **Default to Server Components** - All components are server by default
- Use `"use client"` directive only when needed:
  - Interactive elements (onClick, onChange)
  - Browser APIs (window, localStorage)
  - React hooks (useState, useEffect)
  - Third-party client libraries

### App Router Conventions
- `page.tsx` - Page component
- `layout.tsx` - Shared layout
- `loading.tsx` - Loading UI
- `error.tsx` - Error boundary
- `not-found.tsx` - 404 page
- `route.ts` - API route handler
- `middleware.ts` - Request middleware

### Data Fetching Patterns
1. **Server Components**: Fetch directly in components
2. **Server Actions**: Form mutations and data updates
3. **Route Handlers**: RESTful API endpoints
4. **Parallel Data Loading**: Use Promise.all()
5. **Streaming**: Use Suspense boundaries

## Project Conventions
- **Components**: PascalCase, colocated with usage
- **Server Actions**: Prefix with "action" (e.g., `actionCreateUser`)
- **API Routes**: RESTful naming `/api/users/[id]`
- **Types**: Dedicated types/ folder, use .d.ts for declarations
- **Styles**: CSS Modules for component styles, Tailwind for utilities
- **Images**: Use next/image, store in public/images

## Server Actions Best Practices
```typescript
// Always mark with "use server"
"use server"

// Return typed responses
export async function actionCreatePost(data: FormData) {
  // Validate with zod
  const validated = postSchema.parse(Object.fromEntries(data))
  
  // Perform action
  const post = await db.post.create({ data: validated })
  
  // Revalidate cache
  revalidatePath('/posts')
  
  return { success: true, data: post }
}
```

## MCP Context Sources
The following MCP servers provide real-time context:
- **context7**: Latest Next.js/React documentation
- **github**: Repository issues and PRs
- **database**: Current schema and migrations
- **vercel**: Deployment status and analytics
- **npm-registry**: Package version information

## Common Pitfalls to Avoid
- Don't use `"use client"` unnecessarily
- Avoid large client bundles - split code
- Don't fetch data in client components
- Remember: metadata is server-only
- Use dynamic imports for heavy client libraries
```

### 3. Next.js TypeScript Configuration

```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/*": ["./*"],
      "@/components/*": ["./components/*"],
      "@/lib/*": ["./lib/*"],
      "@/hooks/*": ["./hooks/*"],
      "@/types/*": ["./types/*"],
      "@/app/*": ["./app/*"],
      "@/styles/*": ["./styles/*"]
    }
  },
  "include": [
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx",
    ".next/types/**/*.ts"
  ],
  "exclude": ["node_modules"]
}
```

---

## Hooks Configuration with MCP Integration {#hooks-configuration}

### Core Concept: Context-First Development
Before Claude writes any code, hooks should gather context from MCP servers to ensure accuracy and relevance.

### 1. Master Hooks Configuration

```toml
# .claude/settings.toml

# Pre-code generation: Fetch Next.js/React context from MCP servers
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.ts", "*.tsx"]
command = """
# Determine if file is server or client component
if grep -q '"use client"' "$CLAUDE_FILE_PATHS"; then
  echo "🔍 Client component detected"
  claude-code -p "Use Context7 to get React hooks and client-side patterns for the libraries in $CLAUDE_FILE_PATHS" --headless
else
  echo "🔍 Server component detected"
  claude-code -p "Use Context7 to get Next.js 15 server component patterns and data fetching best practices" --headless
fi

# Check for similar components
find app components -name "*.tsx" -type f | head -20
"""

# Post-edit: Next.js-specific validation and format
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.ts", "*.tsx"]
command = """
# Next.js build check (catches server/client issues)
npx next lint $CLAUDE_FILE_PATHS

# Type checking
npx tsc --noEmit --pretty

# Formatting
npx prettier --write $CLAUDE_FILE_PATHS

# Run component tests if they exist
if [ -f "${CLAUDE_FILE_PATHS%.tsx}.test.tsx" ]; then
  npx jest "${CLAUDE_FILE_PATHS%.tsx}.test.tsx" --passWithNoTests
fi
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

# Server Action validation
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["**/actions/*.ts", "app/**/actions.ts"]
command = """
# Ensure server actions are properly configured
if ! grep -q '"use server"' "$CLAUDE_FILE_PATHS"; then
  echo "⚠️  Warning: Server action file missing 'use server' directive"
  exit 2
fi

# Get server action best practices
claude-code -p "Use context7 to get Next.js 15 server actions best practices" --headless
"""

# Route handler validation
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["**/route.ts", "**/route.js"]
command = """
# Check route handler patterns
echo "🔍 Validating API route handler..."
claude-code -p "Use context7 to verify Next.js 15 route handler patterns for $CLAUDE_FILE_PATHS" --headless
"""

# MCP-powered component creation
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "create_file"
file_paths = ["**/*.tsx"]
command = """
# Determine component type from path
if [[ "$CLAUDE_FILE_PATHS" == *"app/"* ]]; then
  echo "📄 Creating page/layout component"
  claude-code -p "Use context7 for Next.js 15 page/layout conventions" --headless
else
  echo "🧩 Creating reusable component"
  claude-code -p "Use github MCP to find similar component patterns in this repo" --headless
fi
"""
```

### 2. MCP Integration Hooks

```toml
# Advanced MCP-powered hooks

# Prisma/Drizzle schema validation
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["**/schema.prisma", "**/schema/*.ts", "lib/db/*.ts"]
command = """
# Validate database schema changes
if [[ "$CLAUDE_FILE_PATHS" == *"schema.prisma" ]]; then
  echo "🔍 Validating Prisma schema..."
  npx prisma validate
else
  echo "🔍 Checking Drizzle schema..."
  claude-code -p "Use database MCP to validate schema compatibility" --headless
fi
"""

# Next.js API route validation
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["app/api/**/route.ts"]
command = """
# Validate API route exports
echo "🔍 Checking API route handlers..."

# Ensure proper HTTP method exports
if ! grep -E "(GET|POST|PUT|DELETE|PATCH)" "$CLAUDE_FILE_PATHS"; then
  echo "⚠️  Warning: No HTTP method handlers found"
fi

# Get API best practices
claude-code -p "Use context7 for Next.js 15 API route patterns" --headless
"""

# Vercel deployment checks
[[hooks]]
event = "PostToolUse"
run_in_background = true
command = """
# Run Next.js production build check
npx next build --no-lint || echo "⚠️  Build check failed"

# Check bundle size
npx @next/bundle-analyzer || true
"""

# Middleware validation
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["middleware.ts", "middleware.js"]
command = """
# Validate Next.js middleware patterns
echo "🔍 Checking middleware configuration..."
claude-code -p "Use context7 to verify Next.js 15 middleware patterns and edge runtime compatibility" --headless
"""
```

---

## Essential MCP Servers for Next.js {#essential-mcp-servers}

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
- Get latest Next.js 15 App Router patterns
- React Server Components documentation
- Server Actions best practices
- Suspense and streaming patterns
- Next.js-specific TypeScript types

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
      "allowedPaths": ["./app", "./components", "./lib", "./public"]
    },
    "description": "Advanced file operations"
  }
}
```

### 8. Vercel MCP - Deployment & Analytics
```json
{
  "vercel": {
    "command": "npx",
    "args": ["-y", "vercel-mcp-server"],
    "env": {
      "VERCEL_TOKEN": "${VERCEL_TOKEN}"
    },
    "description": "Deployment status, analytics, and edge functions"
  }
}
```

**Use Cases**:
- Check deployment status
- View Core Web Vitals
- Monitor edge function performance
- Preview deployments
- Analytics insights

### 9. Next.js MCP - Framework Intelligence
```json
{
  "nextjs": {
    "command": "npx",
    "args": ["-y", "next-mcp-server"],
    "description": "Next.js-specific development assistance"
  }
}
```

**Use Cases**:
- Route structure analysis
- Build optimization suggestions
- Performance recommendations
- Config validation
- Best practices enforcement

### Complete .mcp.json Configuration for Next.js
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
    },
    "vercel": {
      "command": "npx",
      "args": ["-y", "vercel-mcp-server"],
      "env": {
        "VERCEL_TOKEN": "${VERCEL_TOKEN}"
      }
    },
    "nextjs": {
      "command": "npx",
      "args": ["-y", "next-mcp-server"]
    }
  }
}
```

---

## Headless Claude Code Automation {#headless-automation}

### 1. Basic Headless Usage for Next.js
```bash
# Simple headless command
claude-code -p "Convert this page component to use server actions" --headless

# Check for client/server component issues
claude-code -p "Analyze app/ directory for unnecessary client components" --output-format json --headless

# Optimize data fetching
claude-code -p "Update dashboard page to use parallel data fetching with Promise.all" \
  --context "Use Next.js 15 best practices for server components" \
  --headless

# Generate API route from schema
claude-code -p "Create a Next.js API route for the User model with full CRUD operations" \
  --context "Use app/api/users/route.ts pattern" \
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

### 3. Next.js-Specific Headless Patterns

```bash
#!/bin/bash
# scripts/optimize-nextjs.sh

# Analyze and optimize client components
echo "🔍 Analyzing component usage..."
COMPONENTS=$(claude-code -p "List all components that could be converted to server components" \
  --output-format json --headless)

# Convert each component
echo "$COMPONENTS" | jq -r '.components[]' | while read component; do
  claude-code -p "Convert $component to server component if possible" --headless
  
  # Verify build still works
  npx next build || {
    echo "Build failed after converting: $component"
    git checkout -- .
    exit 1
  }
done

# Optimize images
claude-code -p "Find all img tags and convert to next/image with proper sizing" --headless

# Generate sitemap
claude-code -p "Create app/sitemap.ts based on current routes" --headless
```

### 4. MCP + Headless Workflows

```bash
# Pre-code context gathering for Next.js
claude-code -p "Use context7 to get Next.js 15 server components and data fetching patterns" --headless > .claude/nextjs-patterns.md
claude-code -p "Use vercel MCP to check current Core Web Vitals and performance metrics" --headless > .claude/performance-baseline.md
claude-code -p "Use github to find similar page implementations in this repo" --headless > .claude/similar-pages.md

# Now generate optimized page with full context
claude-code -p "Create a dashboard page using patterns from .claude/ that optimizes for Core Web Vitals"

# Verify deployment readiness
claude-code -p "Use vercel MCP to validate deployment configuration" --headless
```

---

## Complete Integration Example {#complete-example}

### Next.js 15 Project Setup Script
```bash
#!/bin/bash
# setup-claude-nextjs-project.sh

PROJECT_NAME=$1

# Create Next.js project with TypeScript
npx create-next-app@latest $PROJECT_NAME \
  --typescript \
  --tailwind \
  --app \
  --no-src-dir \
  --import-alias "@/*"

cd $PROJECT_NAME

# Create additional directories
mkdir -p {components/{ui,features,providers},lib/{actions,db,utils},hooks,types,.claude/commands}

# Update package.json with additional scripts
npm pkg set scripts.typecheck="tsc --noEmit"
npm pkg set scripts.db:push="prisma db push"
npm pkg set scripts.db:studio="prisma studio"
npm pkg set scripts.test:e2e="playwright test"
npm pkg set scripts.analyze="ANALYZE=true next build"
npm pkg set scripts.validate="npm run typecheck && npm run lint && npm run test"

# Install additional dependencies
npm install -D @types/node prisma @prisma/client zod react-hook-form @hookform/resolvers
npm install -D jest @testing-library/react @testing-library/jest-dom @playwright/test
npm install zustand @tanstack/react-query lucide-react

# Setup Prisma
npx prisma init

# Create middleware.ts
cat > middleware.ts << 'EOF'
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  // Add authentication check here
  return NextResponse.next()
}

export const config = {
  matcher: ['/dashboard/:path*', '/api/:path*']
}
EOF

# Create CLAUDE.md
cat > CLAUDE.md << 'EOF'
# CLAUDE.md - Next.js 15 Project Context

## Project Overview
**Name**: Next.js 15 Claude Code Project
**Framework**: Next.js 15 with App Router
**Deployment**: Vercel

## Technology Stack
- Next.js 15.x (App Router)
- TypeScript 5.x (strict mode)
- Tailwind CSS
- Prisma ORM
- React Query
- Zustand

## Commands
\`\`\`bash
npm run dev       # Start development server
npm run build     # Production build
npm run start     # Start production server
npm run lint      # Run ESLint
npm run typecheck # TypeScript check
npm run test      # Run tests
npm run test:e2e  # Run E2E tests
npm run db:push   # Push schema to database
npm run validate  # Run all checks
\`\`\`

## App Router Structure
- Server Components by default
- Use "use client" only when needed
- Server Actions for mutations
- Parallel data fetching
- Streaming with Suspense

## MCP Servers Available
- context7: Next.js/React docs
- github: Repository intelligence
- database: Schema validation
- vercel: Deployment status
- nextjs: Framework assistance
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
command = """
if grep -q '"use client"' "$CLAUDE_FILE_PATHS"; then
  claude-code -p "Use context7 for React client patterns" --headless
else
  claude-code -p "Use context7 for Next.js 15 server patterns" --headless
fi
"""

[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.ts", "*.tsx"]
command = "npx next lint $CLAUDE_FILE_PATHS && npx prettier --write $CLAUDE_FILE_PATHS"

[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["app/**/route.ts"]
command = "claude-code -p 'Use context7 for Next.js 15 API route patterns' --headless"
EOF

# Create Next.js-specific custom commands
cat > .claude/commands/create-page.md << 'EOF'
---
name: create-page
description: Create a new Next.js page with proper structure
---

Create a new Next.js page component with:
1. Proper server/client component separation
2. Loading and error boundaries
3. Metadata for SEO
4. Proper data fetching patterns
5. TypeScript types
6. Tailwind styling

Page route: $ARGUMENTS
EOF

cat > .claude/commands/create-server-action.md << 'EOF'
---
name: create-server-action
description: Create a new server action with validation
---

Create a new Next.js server action with:
1. "use server" directive
2. Zod schema validation
3. Proper error handling
4. Type-safe return values
5. Database transaction if needed
6. Cache revalidation

Action name: $ARGUMENTS
EOF

# Create sample server action
mkdir -p lib/actions
cat > lib/actions/example.ts << 'EOF'
"use server"

import { z } from 'zod'
import { revalidatePath } from 'next/cache'

const schema = z.object({
  name: z.string().min(1),
  email: z.string().email()
})

export async function actionCreateUser(formData: FormData) {
  const validated = schema.parse({
    name: formData.get('name'),
    email: formData.get('email')
  })
  
  // Add your logic here
  
  revalidatePath('/')
  return { success: true }
}
EOF

# Initialize git
git init
cat > .gitignore << 'EOF'
# Dependencies
node_modules
.pnp
.pnp.js

# Testing
coverage
.nyc_output

# Next.js
.next/
out/
build

# Production
dist

# Misc
.DS_Store
*.pem

# Debug
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Local env files
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# Vercel
.vercel

# Typescript
*.tsbuildinfo
next-env.d.ts

# Claude
.claude/cache
EOF

echo "✅ Next.js 15 project setup complete! Run 'claude-code' to start coding."
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

This manual provides a comprehensive approach to context engineering with Claude Code for Next.js 15 projects. The key insights are:

1. **Server-First Architecture**: Leverage Next.js 15's server components by default, using client components only when necessary
2. **Context Before Code**: Use MCP servers (especially Context7 and Vercel) to provide real-time framework documentation before code generation
3. **Intelligent Automation**: Hooks that understand the difference between server and client components, automatically applying appropriate patterns

Remember: **Context is everything**. The better you provide Next.js-specific context through MCP servers, CLAUDE.md, and hooks, the better Claude Code performs.

### Quick Start Checklist
- [ ] Create Next.js 15 project with App Router
- [ ] Set up CLAUDE.md with Next.js conventions
- [ ] Configure .mcp.json with Context7, Vercel, and Next.js servers
- [ ] Create .claude/settings.toml with server/client-aware hooks
- [ ] Test headless commands for component optimization
- [ ] Implement custom commands for pages and server actions

### Next.js-Specific Tips
- Always start with server components
- Use Server Actions for mutations
- Implement proper loading/error boundaries
- Optimize for Core Web Vitals
- Leverage parallel data fetching
- Use Suspense for better UX

*Last updated: January 2025*