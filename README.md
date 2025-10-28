# Zeff Claude Setup

My personal collection of specialized agents and configurations for Claude Code, optimized for full-stack development with a focus on mobile (React Native) and backend architecture.

## Overview

This setup includes custom agents that provide expert guidance across different domains of software engineering, from mobile app development to backend systems, security, and performance optimization.

## Agents Included

### Engineering Agents

#### 🎨 **frontend-architect**
Create accessible, performant user interfaces with focus on user experience and modern frameworks.
- Accessibility (WCAG 2.1 AA compliance)
- Performance optimization (Core Web Vitals)
- Responsive design patterns
- Component architecture
- React, Vue, Angular best practices

#### 📱 **mobile-architect** (NEW)
Build native mobile experiences with React Native, focusing on performance and platform-specific best practices.
- React Native development with Expo
- iOS (HIG) and Android (Material Design) compliance
- Native module integration
- Offline-first architecture
- 60 FPS performance optimization
- Platform-specific implementations

#### ⚙️ **backend-architect**
Design reliable backend systems with focus on data integrity, security, and fault tolerance.
- RESTful APIs and GraphQL
- Database architecture and optimization
- Security implementation
- System reliability patterns
- Caching and scaling strategies

#### 🏗️ **system-architect**
Design scalable system architecture with focus on maintainability and long-term technical decisions.
- Component boundaries and interfaces
- Scalability strategies
- Dependency management
- Architectural patterns (microservices, CQRS, event sourcing)
- Technology selection guidance

### Quality Agents

#### 🔒 **security-engineer**
Identify security vulnerabilities and ensure compliance with security standards and best practices.
- Vulnerability assessment (OWASP Top 10)
- Threat modeling
- Compliance verification
- Authentication & authorization
- Data protection and encryption

#### ⚡ **performance-engineer**
Optimize system performance through measurement-driven analysis and bottleneck elimination.
- Frontend performance (Core Web Vitals)
- Backend optimization (query, caching)
- Resource optimization
- Benchmarking and profiling
- Critical path analysis

#### 🔧 **refactoring-expert**
Improve code quality and reduce technical debt through systematic refactoring and clean code principles.
- Code smell identification
- Refactoring strategies
- Clean code patterns
- Technical debt reduction

### Specialized Agents

#### 🔍 **deep-research-agent**
Specialist for comprehensive research with adaptive strategies and intelligent exploration.
- Codebase exploration
- Technology research
- Pattern discovery

#### 📋 **requirements-analyst**
Transform ambiguous project ideas into concrete specifications through systematic requirements discovery.
- Requirements gathering
- User story creation
- Specification documentation
- Stakeholder alignment

#### 🎓 **learning-guide**
Teach programming concepts and explain code with focus on understanding through progressive learning.
- Concept explanation
- Code walkthroughs
- Best practices education
- Progressive learning paths

#### 📚 **technical-writer**
Create clear, comprehensive technical documentation tailored to specific audiences.
- API documentation
- Architecture diagrams
- User guides
- Code documentation

#### 🔬 **tech-stack-researcher**
Guide technology choices, architecture decisions, and implementation approaches during planning phases.
- Technology comparisons
- Stack recommendations
- Architectural guidance
- Trade-off analysis

## Installation

### Clone this setup to your Claude directory:

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/zeff-claude-setup.git ~/.claude/zeff-claude-setup

# Or if you want to replace your existing setup
cd ~/.claude
git clone https://github.com/YOUR_USERNAME/zeff-claude-setup.git
```

### Use specific agents:

```bash
# The agents will be automatically available in Claude Code
# Invoke them based on your task context
```

## Usage Examples

### Mobile Development
```
User: "I need to build a React Native app with offline-first architecture"
→ mobile-architect agent activates
→ Provides guidance on React Native, local storage, sync strategies
```

### Backend API Design
```
User: "Design a REST API for user authentication with rate limiting"
→ backend-architect agent activates
→ Provides API specs, security patterns, rate limiting implementation
```

### Performance Optimization
```
User: "My React app is slow, help me optimize it"
→ performance-engineer agent activates
→ Profiles app, identifies bottlenecks, provides optimization strategies
```

### Security Review
```
User: "Review this authentication code for security issues"
→ security-engineer agent activates
→ Performs vulnerability assessment, provides remediation guidance
```

## Project Structure

```
zeff-claude-setup/
├── README.md                              # This file
├── CLAUDE.md                             # Global user instructions
└── .claude/
    └── agents/
        ├── mobile-architect.md           # NEW: React Native specialist
        ├── backend-architect.md          # Backend systems
        ├── frontend-architect.md         # Web UI development
        ├── system-architect.md           # System design
        ├── security-engineer.md          # Security specialist
        ├── performance-engineer.md       # Performance optimization
        ├── refactoring-expert.md         # Code quality
        ├── requirements-analyst.md       # Requirements gathering
        ├── learning-guide.md             # Teaching & explanation
        ├── technical-writer.md           # Documentation
        ├── tech-stack-researcher.md      # Technology guidance
        └── deep-research-agent.md        # Research specialist
```

## Customization

### Adding Your Own Agents

Create a new agent in `.claude/agents/your-agent.md`:

```markdown
---
name: your-agent
description: Brief description of what this agent does
category: engineering|quality|specialized
---

# Your Agent Name

## Triggers
- When to activate this agent

## Behavioral Mindset
- How this agent thinks and approaches problems

## Focus Areas
- Key areas of expertise

## Key Actions
- What actions this agent takes

## Outputs
- What this agent produces

## Boundaries
**Will:**
- Things this agent will do

**Will Not:**
- Things this agent won't do
```

### Global Instructions

Edit `CLAUDE.md` to add project-wide instructions that apply to all agents:

```markdown
## TypeScript Best Practices
- Never use "any" type for type declarations

## Code Style
- Use functional components in React
- Prefer composition over inheritance
```

## Contributing

This is a personal setup, but feel free to fork and adapt it to your needs!

## License

MIT License - Feel free to use and modify as needed.

## Version History

- **v1.0.0** - Initial setup with 12 specialized agents
- Added mobile-architect for React Native development
- Comprehensive coverage of engineering, quality, and specialized domains

---

**Author**: Zeff
**Last Updated**: 2025-10-28
