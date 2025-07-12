# Claude Code Context Engineering Manual
*A 20-minute guide to mastering context engineering with Claude Code for TypeScript projects and creative tasks*

## Table of Contents
1. [Quick Start: What is Claude Code?](#quick-start)
2. [TypeScript Development Best Practices](#typescript-development)
3. [Context Engineering Fundamentals](#context-engineering-fundamentals)
4. [Document Analysis & Creative Writing](#document-analysis)
5. [Advanced Techniques](#advanced-techniques)
6. [Quick Reference](#quick-reference)

---

## Quick Start: What is Claude Code? {#quick-start}

Claude Code is Anthropic's command-line tool for agentic coding that transforms how developers interact with AI. Think of it as having a senior engineer pair programming with you 24/7.

### Key Features
- **No special prompting required** - Just ask questions naturally
- **Automatic project comprehension** - Understands your entire codebase
- **Git integration** - Handles 90%+ of version control tasks
- **Real-time context visualization** - See exactly how much context is being used
- **Extended thinking modes** - From quick answers to deep analysis

### Installation & Setup (2 minutes)
```bash
# Install Claude Code
npm install -g @anthropic/claude-code

# Initialize in your project
claude-code init

# Create CLAUDE.md in your project root
touch CLAUDE.md
```

---

## TypeScript Development Best Practices {#typescript-development}

### 1. Project Configuration with CLAUDE.md

Create a `CLAUDE.md` file in your project root. This acts as Claude's persistent memory:

```markdown
# CLAUDE.md

## Project Overview
This is a TypeScript monorepo using:
- Node.js 20.x
- TypeScript 5.x
- Yarn workspaces
- ESLint + Prettier

## Common Commands
- `yarn build` - Build all packages
- `yarn test` - Run all tests
- `yarn test:watch` - Run tests in watch mode
- `yarn lint` - Run ESLint
- `yarn typecheck` - Run TypeScript compiler

## Architecture
- `/packages/core` - Core business logic
- `/packages/api` - REST API implementation
- `/packages/web` - React frontend

## Coding Standards
- Use functional components with hooks
- Prefer composition over inheritance
- All async functions must have error handling
- Use strict TypeScript settings
```

### 2. Effective Prompting Strategies

#### Basic Request
```
"Add error handling to the user service"
```

#### Better: Specific Context
```
"Add comprehensive error handling to the user service in packages/api/src/services/UserService.ts. Include:
- Custom error classes for different failure types
- Proper logging with correlation IDs
- Return appropriate HTTP status codes
- Add unit tests for error scenarios"
```

#### Best: With Thinking Mode
```
"think hard about the error handling strategy for our user service. Consider:
- What types of errors can occur (validation, database, external API)
- How to maintain consistency across services
- How to make errors debuggable in production
Then implement comprehensive error handling in UserService.ts"
```

### 3. TypeScript-Specific Tips

**Use Tab Completion**: Navigate files quickly
```
# Type partial path and press Tab
"Update the types in src/ty<TAB>"
# Autocompletes to: src/types/
```

**Leverage Type Information**
```
"Analyze the type dependencies in our API package and suggest improvements for better type safety"
```

**Monorepo Management**
```
"Update all packages to use the new Logger type from @myapp/core"
```

---

## Context Engineering Fundamentals {#context-engineering-fundamentals}

### Understanding Context Windows

Claude's context window is like RAM - limited but powerful when used wisely:

```
┌─────────────────────────────────────┐
│     Context Window (200K tokens)     │
├─────────────────────────────────────┤
│ CLAUDE.md         (2K tokens)   5%  │
│ Current Files     (10K tokens)  10% │
│ Conversation      (20K tokens)  15% │
│ Available Space   (168K tokens) 70% │
└─────────────────────────────────────┘
```

### Context Management Commands

**Monitor Usage**: Watch the percentage indicator in your terminal

**Compact Context**: Intelligently summarize without losing critical information
```bash
/compact
```

**Clear Context**: Start fresh between unrelated tasks
```bash
/clear
```

### The Context Engineering Pyramid

```
        ┌───────┐
       │Systems │ <- Entire architectures
      ┌─────────┐
     │  Agents  │ <- Multi-step workflows  
    ┌───────────┐
   │   Memory   │ <- Persistent state
  ┌─────────────┐
 │   Examples   │ <- Few-shot patterns
┌───────────────┐
│    Prompts    │ <- Basic instructions
└───────────────┘
```

### Best Practices for Context Optimization

1. **Strategic Chunking**: Break large tasks into context-friendly pieces
   ```
   ❌ "Refactor the entire application architecture"
   ✅ "Let's refactor in phases: 1) Extract interfaces, 2) Update implementations, 3) Add tests"
   ```

2. **Document Ordering**: Place large documents at the top
   ```xml
   <document>
     <document_content>
       [Your 50-page transcript here]
     </document_content>
     <source>meeting_transcript_2025_01.txt</source>
   </document>
   
   <query>
   Summarize the key decisions from this meeting
   </query>
   ```

3. **Proactive Compaction**: Compact at natural breakpoints
   ```
   After completing feature A:
   /compact
   "Now let's work on feature B"
   ```

---

## Document Analysis & Creative Writing {#document-analysis}

### Analyzing Large Documents

Claude excels at processing extensive documents in a single context:

#### 1. Transcript Analysis Pattern
```xml
<transcript>
  <metadata>
    <date>2025-01-15</date>
    <participants>Alice, Bob, Charlie</participants>
    <duration>2 hours</duration>
  </metadata>
  <content>
    [Full transcript text]
  </content>
</transcript>

<analysis_request>
think hard about this transcript and provide:
1. Executive summary (3-5 bullet points)
2. Key decisions made
3. Action items by participant
4. Areas of disagreement or concern
5. Follow-up questions that remain unanswered
</analysis_request>
```

#### 2. Multi-Document Synthesis
```xml
<documents>
  <document id="1" type="planning">
    <title>Q1 Product Roadmap</title>
    <content>[roadmap content]</content>
  </document>
  
  <document id="2" type="transcript">
    <title>Engineering Stand-up</title>
    <content>[transcript content]</content>
  </document>
  
  <document id="3" type="requirements">
    <title>Customer Feature Requests</title>
    <content>[requirements content]</content>
  </document>
</documents>

<synthesis_task>
Analyze these documents and create a unified implementation plan that:
- Aligns with the Q1 roadmap
- Addresses engineering concerns from the standup
- Incorporates high-priority customer requests
- Identifies conflicts and proposes resolutions
</synthesis_task>
```

### Creative Writing with Context

#### Script Writing Pattern
```markdown
# Creative Brief
<context>
  <genre>Sci-fi thriller</genre>
  <setting>Mars colony, 2145</setting>
  <themes>Isolation, survival, corporate espionage</themes>
  <tone>Tense, psychological</tone>
</context>

<character_profiles>
  <protagonist>
    Name: Dr. Sarah Chen
    Background: Exobiologist, discovered anomaly
    Motivation: Protect colony, uncover truth
    Flaw: Trusts too easily
  </protagonist>
  [Additional characters...]
</character_profiles>

<plot_outline>
  [Your plot points]
</plot_outline>

<request>
Write Act 1, Scene 3 where Sarah discovers the corporate conspiracy. 
Include:
- Subtle foreshadowing of the betrayal
- Technical dialogue that feels authentic
- Building tension through environmental details
- Character development through action, not exposition
</request>
```

#### Research-Based Content Creation
```
/claude "I've uploaded 5 research papers on quantum computing. Create a comprehensive blog post series that:
1. Explains key concepts for a technical but non-specialist audience
2. Includes real-world applications
3. Addresses common misconceptions
4. Provides code examples where appropriate
think harder about how to make complex topics accessible without oversimplifying"
```

---

## Advanced Techniques {#advanced-techniques}

### 1. Multi-Agent Workflows

Use multiple Claude instances for complex tasks:

```bash
# Terminal 1: Implementation Agent
claude-code
"Implement the new authentication system based on requirements.md"

# Terminal 2: Review Agent
claude-code
"Review the authentication implementation for security vulnerabilities and best practices"

# Terminal 3: Test Agent
claude-code
"Write comprehensive tests for the authentication system, including edge cases"
```

### 2. Custom Commands

Create reusable workflows in `.claude/commands/`:

```markdown
# .claude/commands/analyze-pr.md
---
name: analyze-pr
description: Comprehensive PR analysis
---

Perform a thorough analysis of the current git changes:

1. Review code quality and adherence to project standards
2. Check for potential bugs or edge cases
3. Verify test coverage for new code
4. Assess performance implications
5. Ensure documentation is updated
6. Generate a PR description with:
   - Summary of changes
   - Technical details
   - Testing approach
   - Potential impacts

think hard about subtle issues that might not be immediately obvious.
```

### 3. Context Engineering Patterns

#### The Cascade Pattern
Start broad, then narrow focus:
```
1. "Analyze our entire authentication flow"
2. "Focus on the token refresh mechanism"
3. "Implement improvements to the refresh logic"
4. "Add tests for the edge cases we discussed"
```

#### The Synthesis Pattern
Combine multiple contexts:
```
"Using the meeting transcript, technical spec, and user feedback documents I've provided, synthesize a implementation plan that addresses all concerns"
```

#### The Iteration Pattern
Refine through cycles:
```
1. "Create initial implementation"
2. "Review and identify improvements"
3. "Refactor based on review"
4. "Optimize for performance"
```

### 4. Extended Thinking Modes

Use progressively deeper analysis:

- `think` - Quick analysis (30 seconds)
- `think hard` - Thorough evaluation (2-3 minutes)  
- `think harder` - Deep consideration (5-10 minutes)
- `ultrathink` - Maximum depth (10-30 minutes)

Example:
```
"ultrathink about our system architecture and propose a migration path to microservices that minimizes risk and disruption"
```

---

## Quick Reference {#quick-reference}

### Essential Commands
| Command | Purpose | When to Use |
|---------|---------|-------------|
| `/compact` | Intelligently summarize context | Every 2-3 hours or at major milestones |
| `/clear` | Reset context completely | Between unrelated tasks |
| `/think` | Trigger extended thinking | Complex problems requiring analysis |
| `/pr` | Create pull request | After completing features |

### Context Optimization Checklist
- [ ] CLAUDE.md file created and comprehensive
- [ ] Large documents placed at prompt top
- [ ] Using XML tags for structure
- [ ] Monitoring context percentage
- [ ] Compacting at natural breakpoints
- [ ] Clear task decomposition
- [ ] Specific, detailed instructions

### TypeScript Project Setup
```bash
# 1. Initialize Claude Code
claude-code init

# 2. Create comprehensive CLAUDE.md
cat > CLAUDE.md << 'EOF'
# Project: [Your Project Name]

## Tech Stack
- TypeScript 5.x
- Node.js 20.x
- [Your frameworks]

## Commands
- Build: `npm run build`
- Test: `npm test`
- Dev: `npm run dev`

## Architecture
[Your architecture description]

## Conventions
[Your coding standards]
EOF

# 3. Create custom commands
mkdir -p .claude/commands
echo "Your custom command templates" > .claude/commands/your-command.md

# 4. Start coding with Claude
claude-code
```

### Pro Tips
1. **Version Control**: Commit CLAUDE.md to share team knowledge
2. **Screenshots**: Cmd+Ctrl+Shift+4 (Mac) → Ctrl+V to paste
3. **File Navigation**: Use Tab completion everywhere
4. **Subagents**: Launch parallel Claude instances for complex tasks
5. **Think Modes**: Match thinking depth to problem complexity

---

## Conclusion

Context engineering with Claude Code is about thoughtful information architecture. By managing context strategically, providing clear structure, and leveraging Claude's capabilities, you can achieve remarkable productivity gains in both technical development and creative tasks.

Remember: **Context is everything.** The better you engineer it, the better Claude performs.

*Happy coding with Claude! 🚀*