# Context Engineering for Agentic Coding with Claude Code

> A comprehensive manual for leveraging context engineering principles with Claude Code, TypeScript, Next.js 15, and MCP servers to achieve prompt-only development

## Table of Contents

1. [Overview](#overview)
2. [Context Engineering Principles](#context-engineering-principles)
3. [Next.js 15 TypeScript Project Structure](#nextjs-15-typescript-project-structure)
4. [Claude Code Configuration](#claude-code-configuration)
5. [Essential MCP Servers](#essential-mcp-servers)
6. [Hooks for Intelligent Automation](#hooks-for-intelligent-automation)
7. [Prompt Engineering Workflows](#prompt-engineering-workflows)
8. [Best Practices](#best-practices)
9. [Implementation Examples](#implementation-examples)

## Overview

This manual integrates the sophisticated context engineering concepts from IBM Zurich, Princeton, and other leading research institutions with practical Claude Code configurations for TypeScript development. The goal: **work entirely through prompts without touching code**.

### Core Philosophy

Context engineering goes beyond simple prompt engineering by implementing:
- **Progressive Complexity**: Scale from atomic prompts to neural field dynamics
- **Cognitive Tools**: Structured reasoning patterns based on IBM research
- **Symbolic Processing**: Three-stage abstraction-induction-retrieval from Princeton
- **Field Dynamics**: Emergent behaviors and attractor patterns from Shanghai AI Lab
- **Memory Synergy**: Reasoning-driven consolidation from Singapore-MIT

## Context Engineering Principles

### Progressive Complexity Model

```
Atoms → Molecules → Cells → Organs → Neural Systems → Neural Fields
  │        │         │        │            │                │
Prompts  Few-shot  Memory   Multi-     Cognitive      Context Fields
         Examples  States   Agents      Tools         with Emergence
```

### Applied to Claude Code

1. **Atomic Level**: Single Claude Code prompts with Context7 documentation
2. **Molecular Level**: Combining multiple MCP servers in workflows
3. **Cellular Level**: Persistent memory with MCP Memory server
4. **Organic Level**: Multi-agent coordination through hooks
5. **Neural System**: Cognitive tool patterns in prompts
6. **Neural Field**: Emergent behaviors from integrated systems

## Next.js 15 TypeScript Project Structure

### Optimal Structure for Agentic Coding

```
project-root/
├── .claude/
│   ├── commands/           # Custom command templates
│   └── CLAUDE.md          # Project constitution for Claude
├── .mcp.json              # MCP server configuration
├── .claude.json           # Claude Code configuration
├── .cursorrules          # AI behavior rules
├── src/
│   ├── app/              # Next.js 15 App Router
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   └── api/          # API routes
│   ├── components/       # React components
│   │   ├── ui/          # Presentational components
│   │   └── features/    # Feature-specific components
│   ├── lib/             # Utilities and helpers
│   │   ├── api/         # API client functions
│   │   ├── constants/   # Constants
│   │   └── helpers/     # Helper functions
│   ├── hooks/           # Custom React hooks
│   ├── types/           # TypeScript definitions
│   ├── context/         # React contexts
│   └── styles/          # Global styles
├── public/              # Static assets
└── tests/               # Test files
```

### CLAUDE.md Configuration

```markdown
# Project: [Your Project Name]

## Tech Stack
- Framework: Next.js 15
- Language: TypeScript 5.2+
- Styling: Tailwind CSS 3.4
- State Management: React Context + Hooks
- Data Fetching: React Query
- Testing: Jest + React Testing Library

## Architecture Principles
- Feature-based folder structure
- Atomic design for components
- Test-driven development
- Error boundaries at page level
- Type safety with strict mode

## Development Commands
- `npm run dev`: Start development server
- `npm run build`: Build for production
- `npm run test`: Run tests
- `npm run lint`: Run ESLint
- `npm run typecheck`: Run TypeScript compiler

## Coding Standards
- Use named exports over default exports
- Implement error boundaries at page level
- Write unit tests for utility functions
- Use React Query for data fetching
- Follow atomic design principles

## Context Engineering Patterns
- Use cognitive tools for complex reasoning
- Apply progressive complexity scaling
- Implement field dynamics for persistent behaviors
- Leverage symbolic processing for abstractions
```

## Claude Code Configuration

### Essential .claude.json Settings

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem"]
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    },
    "puppeteer": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-puppeteer"]
    }
  },
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Checking Context7 documentation...' && sleep 1"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit",
        "condition": "success",
        "hooks": [
          {
            "type": "command",
            "command": "npm run typecheck --silent && npm run lint --silent"
          }
        ]
      }
    ]
  }
}
```

## Essential MCP Servers

### 1. Context7 - Documentation Integration

**Purpose**: Provides up-to-date, version-specific documentation directly in prompts

**Usage in prompts**:
```
Create a Next.js 15 API route that handles user authentication with JWT tokens. use context7
```

**Benefits**:
- No hallucinated APIs
- Current documentation
- Version-specific examples

### 2. Filesystem - File Operations

**Purpose**: Secure file operations with configurable access controls

**Key capabilities**:
- Read/write files
- Directory management
- Path navigation
- Access control

### 3. Memory - Persistent Context

**Purpose**: Knowledge graph-based persistent memory system

**Architecture**:
```json
{
  "entities": [
    {
      "type": "component",
      "name": "UserDashboard",
      "relationships": ["uses:UserAPI", "contains:UserStats"]
    }
  ],
  "context": "Main user interface component"
}
```

### 4. Puppeteer - Browser Automation

**Purpose**: Web automation and testing

**Use cases**:
- Visual regression testing
- E2E testing
- Web scraping
- Screenshot generation

## Hooks for Intelligent Automation

### Pre-Tool Use Hooks

```json
{
  "PreToolUse": [
    {
      "matcher": "mcp__context7__.*",
      "hooks": [
        {
          "type": "command",
          "command": "echo '[Context7] Fetching latest documentation...'"
        }
      ]
    },
    {
      "matcher": "Edit",
      "hooks": [
        {
          "type": "command",
          "command": "echo '[Edit] Preparing to modify: $TOOL_INPUT_FILE_PATH'"
        }
      ]
    }
  ]
}
```

### Post-Tool Use Hooks

```json
{
  "PostToolUse": [
    {
      "matcher": "Edit",
      "condition": "success",
      "filter": "*.tsx",
      "hooks": [
        {
          "type": "command",
          "command": "npx prettier --write $TOOL_INPUT_FILE_PATH"
        }
      ]
    },
    {
      "matcher": "Bash",
      "condition": "success",
      "filter": "npm install*",
      "hooks": [
        {
          "type": "command",
          "command": "npm run typecheck"
        }
      ]
    }
  ]
}
```

## Prompt Engineering Workflows

### Cognitive Tool Patterns

Based on IBM research, structure prompts using cognitive tools:

#### 1. Understanding Tool
```
Understand the current authentication system in this Next.js app:
- Identify authentication methods
- Extract security patterns
- Highlight potential vulnerabilities
- Map user flow
use context7
```

#### 2. Extraction Tool
```
Extract all API endpoints from the codebase and categorize them by:
- Authentication requirements
- HTTP methods
- Response types
- Error handling patterns
```

#### 3. Application Tool
```
Apply the repository coding standards to refactor the UserDashboard component:
- Follow atomic design principles
- Implement proper error boundaries
- Add comprehensive TypeScript types
- Include unit tests
use context7
```

### Progressive Complexity Workflow

#### Atomic Level
```
Create a button component with TypeScript. use context7
```

#### Molecular Level
```
Create a form component that:
1. Uses the button component
2. Implements validation
3. Handles async submission
4. Shows loading states
use context7
```

#### Cellular Level
```
Implement a complete authentication flow:
1. Login form with validation
2. API integration
3. Token management
4. Protected route handling
5. Remember previous implementation patterns
use context7 and memory
```

## Best Practices

### 1. Documentation-First Development

Always append `use context7` to prompts involving framework-specific code:
```
Create a Next.js 15 middleware for rate limiting. use context7
```

### 2. Memory Management

Use the memory server for complex, multi-step projects:
```
Remember this project uses:
- Authentication: NextAuth.js
- Database: Prisma with PostgreSQL
- API: tRPC
- Styling: Tailwind CSS

Now create a user profile page that integrates with our existing patterns.
```

### 3. Hook Orchestration

Layer hooks for comprehensive automation:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Analyzing file structure...'"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit",
        "condition": "success",
        "filter": "*.{ts,tsx}",
        "hooks": [
          {
            "type": "command",
            "command": "npm run typecheck --silent"
          },
          {
            "type": "command",
            "command": "npm run lint:fix --silent"
          },
          {
            "type": "command",
            "command": "npm run test:related --silent"
          }
        ]
      }
    ]
  }
}
```

### 4. Error Prevention Strategies

Use pre-tool hooks to prevent errors:

```json
{
  "PreToolUse": [
    {
      "matcher": "Edit",
      "filter": "**/api/**",
      "hooks": [
        {
          "type": "command",
          "command": "echo 'WARNING: Modifying API route. Ensuring type safety...'"
        }
      ]
    }
  ]
}
```

### 5. Test-Driven Prompting

Structure prompts for TDD:
```
Create tests for a user authentication service that:
- Validates email format
- Checks password strength
- Handles login attempts
- Manages JWT tokens

Then implement the service to pass these tests. use context7
```

## Implementation Examples

### Example 1: Complete Feature Implementation

```
Using our established patterns, implement a complete blog feature:

1. Database schema (Prisma)
2. API routes (Next.js API)
3. React components
4. Type definitions
5. Unit tests
6. Integration tests

Follow the project's architectural patterns and coding standards. use context7 and memory
```

### Example 2: Refactoring with Context

```
Refactor the current user dashboard to:
1. Improve performance using React.memo and useMemo
2. Add proper TypeScript types
3. Implement error boundaries
4. Add loading skeletons
5. Follow our atomic design system

Maintain all existing functionality. use context7
```

### Example 3: Debugging with Cognitive Tools

```
Debug the authentication flow using cognitive tools:
1. UNDERSTAND the current implementation
2. EXTRACT error patterns from logs
3. HIGHLIGHT potential race conditions
4. APPLY fixes following security best practices

Use the memory server to track debugging progress. use context7 and memory
```

## Advanced Patterns

### Field Dynamics Implementation

Create prompts that leverage emergent behaviors:

```
Analyze the entire codebase and identify:
1. Common patterns (attractors)
2. Anti-patterns to refactor
3. Missing abstractions
4. Opportunities for consolidation

Create a refactoring plan that maintains system coherence while improving code quality.
```

### Symbolic Processing

Use abstraction-induction-retrieval patterns:

```
Abstract the common patterns from our API routes into reusable utilities:
1. Extract authentication checks
2. Standardize error handling
3. Create response formatters
4. Implement request validation

Generate TypeScript types for all abstractions. use context7
```

## Conclusion

By combining context engineering principles with Claude Code's capabilities, you can achieve true prompt-only development. The key is structuring your project, configuration, and prompts to leverage:

1. **Context7** for always-current documentation
2. **Memory** for persistent project knowledge
3. **Hooks** for automated quality control
4. **Cognitive Tools** for structured reasoning
5. **Progressive Complexity** for scaling solutions

Remember: The goal is not just to avoid writing code, but to work at a higher level of abstraction where you're orchestrating intelligent systems rather than managing implementation details.

---

*This manual integrates research from IBM Zurich, Princeton ICML, Singapore-MIT, Shanghai AI Lab, and Context Engineering to provide a comprehensive framework for agentic coding with Claude Code.*