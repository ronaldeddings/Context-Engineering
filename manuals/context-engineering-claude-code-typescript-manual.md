# Context Engineering with Claude Code & TypeScript: A Comprehensive Manual

## Quick Start (3-Minute Overview)

### Core Concept
Context Engineering transforms AI coding from simple prompts to sophisticated cognitive architectures. Think of it as evolving from "write me a function" to orchestrating complex reasoning systems that understand, plan, and execute with human-like intelligence.

### Essential Setup
1. **Install Claude Code**: `npm install -g @anthropic-ai/claude-code`
2. **Add Context7 MCP**: `claude mcp add context7 -- npx -y @upstash/context7-mcp@latest`
3. **Create CLAUDE.md**: Project-wide AI instructions
4. **Configure Hooks**: Automated quality enforcement

### Key Principle
```
atoms → molecules → cells → organs → neural systems → neural fields
Simple prompts → Complex reasoning → Emergent intelligence
```

---

## 1. TypeScript Project Structure for Agentic Coding

### Optimal Monorepo Structure
```
my-project/
├── .claude/
│   ├── settings.toml          # Project hooks configuration
│   ├── settings.local.toml    # Personal settings (git ignored)
│   └── commands/              # Custom slash commands
├── packages/
│   ├── frontend/              # UI components & pages
│   ├── backend/               # API & business logic
│   └── shared/                # Common types & utilities
├── CLAUDE.md                  # AI context instructions
├── tsconfig.json              # Root TypeScript config
├── .pre-commit-config.yaml    # Pre-commit hooks
└── package.json               # Root dependencies
```

### CLAUDE.md Template for TypeScript Projects
```markdown
# Project Context for Claude Code

## Tech Stack
- Framework: Next.js 15
- Runtime: Node.js 20 + TypeScript 5.3
- Styling: Tailwind CSS 3.4
- State: Zustand/Redux Toolkit
- Testing: Vitest + Playwright

## Project Structure
- `packages/frontend/`: Next.js app with App Router
- `packages/backend/`: Express/Fastify API
- `packages/shared/`: Shared TypeScript types and utilities

## Cognitive Architecture Patterns
This project implements Context Engineering principles:
- **Atomic Operations**: Single-purpose functions in `/lib/atoms/`
- **Molecular Compositions**: Combined operations in `/lib/molecules/`
- **Cognitive Tools**: Structured reasoning in `/lib/cognitive/`
- **Field Dynamics**: Emergent behaviors in `/lib/fields/`

## Commands
- `npm run dev`: Start all services
- `npm run typecheck`: Check TypeScript types
- `npm run lint`: Run ESLint
- `npm run test`: Run test suite
- `npm run build`: Production build

## Code Conventions
- Prefer functional components with hooks
- Use arrow functions for components
- Implement cognitive patterns where applicable
- Strong TypeScript types always
- Test-driven development required

## Context Engineering Guidelines
When implementing features:
1. Start with atomic operations
2. Compose into molecular patterns
3. Apply cognitive tools for complex logic
4. Allow field dynamics for emergence
```

### TypeScript Configuration for AI
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "paths": {
      "@atoms/*": ["./packages/shared/lib/atoms/*"],
      "@molecules/*": ["./packages/shared/lib/molecules/*"],
      "@cognitive/*": ["./packages/shared/lib/cognitive/*"],
      "@fields/*": ["./packages/shared/lib/fields/*"]
    }
  }
}
```

---

## 2. Claude Code Hooks Configuration

### Essential Hooks for TypeScript Development

#### `.claude/settings.toml`
```toml
# Type checking before commits
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "bash"
command_contains = ["git commit"]
command = "npm run typecheck"
run_in_background = false

# Format TypeScript files after editing
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.ts", "*.tsx"]
command = "npx prettier --write $CLAUDE_FILE_PATHS && npx eslint --fix $CLAUDE_FILE_PATHS"

# Run tests after code changes
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.test.ts", "*.spec.ts", "*.test.tsx"]
command = "npm run test -- $CLAUDE_FILE_PATHS"

# Invoke Context7 for documentation needs
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "web_search"
query_contains = ["typescript", "react", "next.js"]
command = "echo 'Consider using Context7 MCP for up-to-date docs'"

# Build verification
[[hooks]]
event = "Stop"
command = "npm run build:check"
run_in_background = true

# Security scan on dependency changes
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["package.json", "package-lock.json"]
command = "npm audit"
```

### Headless Mode for CI/CD

#### GitHub Actions Integration
```yaml
name: AI-Powered Code Review
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  ai-review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
      - name: AI Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          npx @anthropic-ai/claude-code -p "Review this PR for:
          1. TypeScript type safety issues
          2. Context engineering pattern violations
          3. Performance concerns
          4. Security vulnerabilities
          Use cognitive tools to analyze complexity" \
          --output-format stream-json
```

#### Pre-commit Hook
```yaml
# .pre-commit-config.yaml
repos:
  - repo: local
    hooks:
      - id: claude-code-review
        name: AI Code Review
        entry: npx @anthropic-ai/claude-code -p "Check for type safety and context patterns"
        language: system
        files: '\.(ts|tsx)$'
        pass_filenames: true
```

---

## 3. MCP Servers for TypeScript Development

### Essential MCP Servers

#### 1. **Context7** - Up-to-date Documentation
```bash
claude mcp add context7 -- npx -y @upstash/context7-mcp@latest
```
Use by appending "use context7" to prompts for current library docs.

#### 2. **TypeScript Server** - Type Analysis
```bash
claude mcp add typescript-server -- npx -y @typescript/mcp-server@latest
```
Provides deep TypeScript analysis and refactoring capabilities.

#### 3. **Database MCP** - Data Operations
```bash
claude mcp add postgres -- npx -y @modelcontextprotocol/server-postgres
```
Direct database access for schema-aware operations.

#### 4. **Testing MCP** - Test Generation
```bash
claude mcp add vitest-server -- npx -y @vitest/mcp-server@latest
```
Intelligent test generation based on code structure.

### Custom MCP Server Template
```typescript
// packages/mcp-servers/cognitive-tools/index.ts
import { McpServer } from '@modelcontextprotocol/server-core';

export class CognitiveToolsServer extends McpServer {
  constructor() {
    super({
      name: 'cognitive-tools',
      version: '1.0.0',
      description: 'Context engineering cognitive tools'
    });

    // Register cognitive operations
    this.registerTool({
      name: 'analyze_complexity',
      description: 'Analyze code complexity using field theory',
      inputSchema: {
        type: 'object',
        properties: {
          filePath: { type: 'string' },
          depth: { type: 'number', default: 3 }
        }
      },
      handler: this.analyzeComplexity.bind(this)
    });

    this.registerTool({
      name: 'generate_cognitive_pattern',
      description: 'Generate cognitive tool pattern',
      inputSchema: {
        type: 'object',
        properties: {
          intent: { type: 'string' },
          level: { 
            type: 'string', 
            enum: ['atom', 'molecule', 'cell', 'organ', 'neural', 'field'] 
          }
        }
      },
      handler: this.generatePattern.bind(this)
    });
  }

  async analyzeComplexity({ filePath, depth }) {
    // Implement field theory analysis
    const fieldAnalysis = await this.performFieldAnalysis(filePath, depth);
    return {
      attractors: fieldAnalysis.attractors,
      resonance: fieldAnalysis.resonance,
      complexity: fieldAnalysis.emergentComplexity
    };
  }

  async generatePattern({ intent, level }) {
    // Generate cognitive pattern based on level
    const pattern = await this.createCognitivePattern(intent, level);
    return {
      pattern,
      implementation: this.generateTypeScriptImplementation(pattern)
    };
  }
}
```

---

## 4. Context Engineering Implementation Patterns

### Atomic Operations (Level 1)
```typescript
// packages/shared/lib/atoms/validation.ts
export const validateEmail = (email: string): boolean => {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
};

export const validatePhone = (phone: string): boolean => {
  return /^\+?[\d\s-()]+$/.test(phone);
};
```

### Molecular Compositions (Level 2)
```typescript
// packages/shared/lib/molecules/user-validation.ts
import { validateEmail, validatePhone } from '@atoms/validation';

export interface UserValidation {
  email: boolean;
  phone: boolean;
  overall: boolean;
}

export const validateUser = (user: {
  email: string;
  phone: string;
}): UserValidation => {
  const emailValid = validateEmail(user.email);
  const phoneValid = validatePhone(user.phone);
  
  return {
    email: emailValid,
    phone: phoneValid,
    overall: emailValid && phoneValid
  };
};
```

### Cognitive Tools (Level 3)
```typescript
// packages/shared/lib/cognitive/user-processor.ts
export class UserCognitiveProcessor {
  private readonly validators = new Map<string, Function>();
  private readonly memory = new Map<string, any>();

  constructor() {
    this.initializeValidators();
  }

  async processUser(userData: any): Promise<ProcessingResult> {
    // Stage 1: Abstraction
    const abstract = this.abstractUserData(userData);
    
    // Stage 2: Induction
    const patterns = await this.inducePatterns(abstract);
    
    // Stage 3: Retrieval
    const result = this.retrieveSolution(patterns);
    
    // Update memory
    this.updateMemory(userData, result);
    
    return result;
  }

  private abstractUserData(data: any): AbstractRepresentation {
    // Convert concrete data to abstract symbols
    return {
      symbols: this.extractSymbols(data),
      relations: this.extractRelations(data)
    };
  }

  private async inducePatterns(abstract: AbstractRepresentation): Promise<Pattern[]> {
    // Apply pattern recognition
    const patterns = [];
    
    // Check against known patterns in memory
    for (const [key, pattern] of this.memory) {
      const similarity = this.calculateSimilarity(abstract, pattern);
      if (similarity > 0.8) {
        patterns.push(pattern);
      }
    }
    
    return patterns;
  }
}
```

### Field Dynamics (Level 4)
```typescript
// packages/shared/lib/fields/semantic-field.ts
export class SemanticField {
  private attractors: Attractor[] = [];
  private fieldStrength: number = 0;
  private resonance: number = 0;

  addAttractor(attractor: Attractor): void {
    this.attractors.push(attractor);
    this.recalculateField();
  }

  private recalculateField(): void {
    // Calculate field dynamics based on attractors
    this.fieldStrength = this.attractors.reduce((sum, a) => sum + a.strength, 0);
    this.resonance = this.calculateResonance();
  }

  private calculateResonance(): number {
    // Implement resonance calculation based on attractor alignment
    let totalAlignment = 0;
    
    for (let i = 0; i < this.attractors.length; i++) {
      for (let j = i + 1; j < this.attractors.length; j++) {
        totalAlignment += this.calculateAlignment(
          this.attractors[i], 
          this.attractors[j]
        );
      }
    }
    
    return totalAlignment / (this.attractors.length * (this.attractors.length - 1) / 2);
  }

  async evolve(): Promise<FieldState> {
    // Allow field to evolve based on dynamics
    const newAttractors = await this.emergentPatterns();
    
    return {
      attractors: [...this.attractors, ...newAttractors],
      strength: this.fieldStrength,
      resonance: this.resonance,
      emergentProperties: this.detectEmergentProperties()
    };
  }
}
```

---

## 5. Best Practices for Context Engineering

### 1. **Progressive Complexity**
- Start with atomic operations
- Compose into molecules only when needed
- Use cognitive tools for complex reasoning
- Apply field dynamics for emergent behaviors

### 2. **Type Safety First**
```typescript
// Always define strong types
export interface CognitiveOperation<T, R> {
  intent: string;
  process: (input: T) => Promise<R>;
  verify: (result: R) => boolean;
  memory?: Map<string, any>;
}
```

### 3. **Hooks for Everything**
- Pre-commit: Type checking, linting
- Post-edit: Formatting, validation
- Pre-tool: Context enhancement
- Stop: Build verification

### 4. **MCP Server Strategy**
- Context7 for documentation
- Custom servers for domain logic
- TypeScript server for refactoring
- Testing server for TDD

### 5. **Memory Management**
```typescript
// Implement selective memory consolidation
export class CognitiveMemory {
  private shortTerm = new Map();
  private longTerm = new Map();
  
  consolidate(): void {
    // Only move relevant patterns to long-term
    for (const [key, value] of this.shortTerm) {
      if (this.isRelevant(value)) {
        this.longTerm.set(key, this.compress(value));
      }
    }
    this.shortTerm.clear();
  }
}
```

---

## 6. Workflow Patterns

### Explore-Plan-Code-Commit Pattern
```bash
# 1. Explore
claude "Analyze the user authentication system use context7"

# 2. Plan
claude "Create a plan to add OAuth2 integration"

# 3. Code
claude "Implement the OAuth2 integration following the plan"

# 4. Commit
claude "Commit changes with comprehensive message"
```

### Multi-Agent Development
```bash
# Terminal 1: Frontend Agent
cd frontend-worktree
claude "Implement the OAuth UI components"

# Terminal 2: Backend Agent
cd backend-worktree
claude "Implement OAuth2 endpoints"

# Terminal 3: Testing Agent
cd testing-worktree
claude "Create comprehensive OAuth tests"
```

### Continuous Context Enhancement
```typescript
// .claude/commands/enhance-context.md
---
name: enhance-context
description: Enhance context with cognitive patterns
---

Analyze the current code context and:
1. Identify atomic operations that could be composed
2. Suggest molecular patterns for repeated logic
3. Recommend cognitive tools for complex operations
4. Detect potential field dynamics
5. Update CLAUDE.md with new patterns

use context7 for latest TypeScript patterns
```

---

## 7. Troubleshooting & Tips

### Common Issues
1. **Stale Documentation**: Always append "use context7" for current docs
2. **Type Errors**: Configure strict TypeScript in hooks
3. **Memory Overflow**: Implement selective consolidation
4. **Emergent Bugs**: Use field analysis tools

### Performance Optimization
- Batch operations at the same complexity level
- Use parallel agents for independent tasks
- Implement caching in MCP servers
- Profile cognitive operations

### Security Considerations
- Never commit API keys (use env vars)
- Sanitize all MCP server inputs
- Implement rate limiting
- Audit cognitive memory regularly

---

## Conclusion

Context Engineering with Claude Code transforms TypeScript development from simple code generation to sophisticated cognitive systems. By implementing progressive complexity, leveraging hooks and MCP servers, and following these patterns, you create AI-augmented development environments that think, reason, and evolve alongside your codebase.

Remember: Start simple (atoms), compose thoughtfully (molecules), reason systematically (cognitive tools), and allow emergence (fields). The goal isn't just to write code faster, but to create systems that exhibit intelligent behavior and continuous improvement.