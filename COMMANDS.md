# COMMANDS.md - SuperClaude Command Execution Framework

Command execution framework for Claude Code SuperClaude integration.

## Command System Architecture

### Core Command Structure
```yaml
---
command: "/{command-name}"
category: "Primary classification"
purpose: "Operational objective"
wave-enabled: true|false
performance-profile: "optimization|standard|complex"
orchestration:
  default_agents: [primary-agent, supporting-agent1, supporting-agent2]
  strategy: "single|sequential|parallel"
  telemetry: enabled
  fallback_chain: [fallback-agent1, fallback-agent2]
  auto_invoke: true
---
```

### Command Processing Pipeline
1. **Input Parsing**: `$ARGUMENTS` with `@<path>`, `!<command>`, `--<flags>`
2. **Context Resolution**: Auto-persona activation and MCP server selection
3. **Wave Eligibility**: Complexity assessment and wave mode determination
4. **Execution Strategy**: Tool orchestration and resource allocation
5. **Quality Gates**: Validation checkpoints and error handling

### Integration Layers
- **Claude Code**: Native slash command compatibility
- **Persona System**: Auto-activation based on command context
- **MCP Servers**: Context7, Sequential, Magic, Playwright integration
- **Wave System**: Multi-stage orchestration for complex operations

## Wave System Integration

**Wave Orchestration Engine**: Multi-stage command execution with compound intelligence. Auto-activates on complexity ≥0.7 + files >20 + operation_types >2.

**Wave-Enabled Commands**:
- **Tier 1**: `/analyze`, `/build`, `/implement`, `/improve`
- **Tier 2**: `/design`, `/task`

### Development Commands

**`/build $ARGUMENTS`**
```yaml
---
command: "/build"
category: "Development & Deployment"
purpose: "Project builder with framework detection"
wave-enabled: true
performance-profile: "optimization"
orchestration:
  default_agents: [frontend-excellence, backend, deployment-engineer]
  strategy: "parallel"
  telemetry: enabled
  fallback_chain: [general-purpose, software-architect]
  auto_invoke: true
---
```
- **Auto-Persona**: Frontend, Backend, Architect, Scribe
- **MCP Integration**: Magic (UI builds), Context7 (patterns), Sequential (logic)
- **Tool Orchestration**: [Read, Grep, Glob, Bash, TodoWrite, Edit, MultiEdit]
- **Arguments**: `[target]`, `@<path>`, `!<command>`, `--<flags>`

**`/implement $ARGUMENTS`**
```yaml
---
command: "/implement"
category: "Development & Implementation"
purpose: "Feature and code implementation with intelligent persona activation"
wave-enabled: true
performance-profile: "standard"
orchestration:
  default_agents: [general-purpose, frontend-excellence, backend]
  strategy: "sequential"
  telemetry: enabled
  fallback_chain: [software-architect, deployment-engineer]
  auto_invoke: true
---
```
- **Auto-Persona**: Frontend, Backend, Architect, Security (context-dependent)
- **MCP Integration**: Magic (UI components), Context7 (patterns), Sequential (complex logic)
- **Tool Orchestration**: [Read, Write, Edit, MultiEdit, Bash, Glob, TodoWrite, Task]
- **Arguments**: `[feature-description]`, `--type component|api|service|feature`, `--framework <name>`, `--<flags>`


### Analysis Commands

**`/analyze $ARGUMENTS`**
```yaml
---
command: "/analyze"
category: "Analysis & Investigation"
purpose: "Multi-dimensional code and system analysis"
wave-enabled: true
performance-profile: "complex"
orchestration:
  default_agents: [general-purpose, software-architect, code-reviewer]
  strategy: "parallel"
  telemetry: enabled
  fallback_chain: [qa-tester, memory-manager]
  auto_invoke: true
---
```
- **Auto-Persona**: Analyzer, Architect, Security
- **MCP Integration**: Sequential (primary), Context7 (patterns), Magic (UI analysis)
- **Tool Orchestration**: [Read, Grep, Glob, Bash, TodoWrite]
- **Arguments**: `[target]`, `@<path>`, `!<command>`, `--<flags>`

**`/troubleshoot $ARGUMENTS`**
```yaml
---
command: "/troubleshoot"
category: "Analysis & Investigation"
purpose: "Problem investigation and root cause analysis"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [general-purpose, qa-tester, software-architect]
  strategy: "sequential"
  telemetry: enabled
  fallback_chain: [code-reviewer, deployment-engineer]
  auto_invoke: true
---
```
- **Auto-Persona**: Analyzer, QA
- **MCP Integration**: Sequential, Playwright
- **Arguments**: `[symptoms]`, `[flags]`

**`/explain $ARGUMENTS`**
```yaml
---
command: "/explain"
category: "Documentation & Education"
purpose: "Educational explanations and knowledge transfer"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [research-docs-api, general-purpose]
  strategy: "single"
  telemetry: enabled
  fallback_chain: [software-architect, code-reviewer]
  auto_invoke: true
---
```
- **Auto-Persona**: Mentor, Scribe
- **MCP Integration**: Context7, Sequential
- **Arguments**: `[topic]`, `[flags]`


### Quality Commands

**`/improve $ARGUMENTS`**
```yaml
---
command: "/improve"
category: "Quality & Enhancement"
purpose: "Evidence-based code enhancement"
wave-enabled: true
performance-profile: "optimization"
orchestration:
  default_agents: [code-reviewer, software-architect, qa-tester]
  strategy: "sequential"
  telemetry: enabled
  fallback_chain: [general-purpose, frontend-excellence]
  auto_invoke: true
---
```
- **Auto-Persona**: Refactorer, Performance, Architect, QA
- **MCP Integration**: Sequential (logic), Context7 (patterns), Magic (UI improvements)
- **Tool Orchestration**: [Read, Grep, Glob, Edit, MultiEdit, Bash]
- **Arguments**: `[target]`, `@<path>`, `!<command>`, `--<flags>`


**`/cleanup $ARGUMENTS`**
```yaml
---
command: "/cleanup"
category: "Quality & Enhancement"
purpose: "Project cleanup and technical debt reduction"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [code-reviewer, general-purpose]
  strategy: "sequential"
  telemetry: enabled
  fallback_chain: [software-architect, qa-tester]
  auto_invoke: true
---
```
- **Auto-Persona**: Refactorer
- **MCP Integration**: Sequential
- **Arguments**: `[target]`, `[flags]`

### Additional Commands

**`/document $ARGUMENTS`**
```yaml
---
command: "/document"
category: "Documentation"
purpose: "Documentation generation and knowledge transfer"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [research-docs-api, general-purpose]
  strategy: "sequential"
  telemetry: enabled
  fallback_chain: [software-architect, code-reviewer]
  auto_invoke: true
---
```
- **Auto-Persona**: Scribe, Mentor
- **MCP Integration**: Context7, Sequential
- **Arguments**: `[target]`, `[flags]`

**`/estimate $ARGUMENTS`**
```yaml
---
command: "/estimate"
category: "Planning"
purpose: "Evidence-based estimation and planning"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [software-architect, general-purpose]
  strategy: "sequential"
  telemetry: enabled
  fallback_chain: [taskmaster, deployment-engineer]
  auto_invoke: true
---
```
- **Auto-Persona**: Analyzer, Architect
- **MCP Integration**: Sequential, Context7
- **Arguments**: `[target]`, `[flags]`

**`/task $ARGUMENTS`**
```yaml
---
command: "/task"
category: "Planning"
purpose: "Long-term project management and orchestration"
wave-enabled: true
performance-profile: "complex"
orchestration:
  default_agents: [taskmaster, software-architect, general-purpose]
  strategy: "sequential"
  telemetry: enabled
  fallback_chain: [deployment-engineer, qa-tester]
  auto_invoke: true
---
```
- **Auto-Persona**: Architect, Analyzer
- **MCP Integration**: Sequential
- **Arguments**: `[operation]`, `[flags]`

**`/test $ARGUMENTS`**
```yaml
---
command: "/test"
category: "Testing"
purpose: "Testing workflows and quality assurance"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [qa-tester, general-purpose, code-reviewer]
  strategy: "parallel"
  telemetry: enabled
  fallback_chain: [software-architect, deployment-engineer]
  auto_invoke: true
---
```
- **Auto-Persona**: QA
- **MCP Integration**: Playwright, Sequential
- **Arguments**: `[type]`, `[flags]`

**`/git $ARGUMENTS`**
```yaml
---
command: "/git"
category: "Version-Control"
purpose: "Git workflow assistance and version control"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [deployment-engineer, general-purpose]
  strategy: "single"
  telemetry: enabled
  fallback_chain: [code-reviewer, software-architect]
  auto_invoke: true
---
```
- **Auto-Persona**: DevOps, Scribe, QA
- **MCP Integration**: Sequential
- **Arguments**: `[operation]`, `[flags]`

**`/design $ARGUMENTS`**
```yaml
---
command: "/design"
category: "Development"
purpose: "Design orchestration and system architecture"
wave-enabled: true
performance-profile: "complex"
orchestration:
  default_agents: [software-architect, frontend-excellence, general-purpose]
  strategy: "parallel"
  telemetry: enabled
  fallback_chain: [backend, deployment-engineer]
  auto_invoke: true
---
```
- **Auto-Persona**: Architect, Frontend
- **MCP Integration**: Magic, Sequential, Context7
- **Arguments**: `[domain]`, `[flags]`

### Meta & Orchestration Commands

**`/index $ARGUMENTS`**
```yaml
---
command: "/index"
category: "Meta"
purpose: "Command catalog browsing and discovery"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [general-purpose, research-docs-api]
  strategy: "single"
  telemetry: enabled
  fallback_chain: [software-architect]
  auto_invoke: true
---
```
- **Auto-Persona**: Mentor, Analyzer
- **MCP Integration**: Sequential
- **Arguments**: `[query]`, `[flags]`

**`/load $ARGUMENTS`**
```yaml
---
command: "/load"
category: "Meta"
purpose: "Project context loading and initialization"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [general-purpose, software-architect, code-reviewer]
  strategy: "parallel"
  telemetry: enabled
  fallback_chain: [frontend-excellence, backend]
  auto_invoke: true
---
```
- **Auto-Persona**: Analyzer, Architect, Scribe
- **MCP Integration**: All servers
- **Arguments**: `[path]`, `[flags]`

**Iterative Operations** - Use `--loop` flag with improvement commands for iterative refinement

**`/spawn $ARGUMENTS`**
```yaml
---
command: "/spawn"
category: "Meta"
purpose: "Task orchestration and multi-agent coordination"
wave-enabled: false
performance-profile: "complex"
orchestration:
  default_agents: [taskmaster, general-purpose, software-architect]
  strategy: "parallel"
  telemetry: enabled
  fallback_chain: [deployment-engineer, qa-tester]
  auto_invoke: true
---
```
- **Auto-Persona**: Analyzer, Architect, DevOps
- **MCP Integration**: All servers
- **Arguments**: `[mode]`, `[flags]`

## Command Execution Matrix

### Performance Profiles
```yaml
optimization: "High-performance with caching and parallel execution"
standard: "Balanced performance with moderate resource usage"
complex: "Resource-intensive with comprehensive analysis"
```

### Command Categories
- **Development**: build, implement, design
- **Planning**: workflow, estimate, task
- **Analysis**: analyze, troubleshoot, explain
- **Quality**: improve, cleanup
- **Testing**: test
- **Documentation**: document
- **Version-Control**: git
- **Deployment**: deploy
- **Optimization**: optimize
- **Meta**: index, load, spawn

### Wave-Enabled Commands
7 commands: `/analyze`, `/build`, `/design`, `/implement`, `/improve`, `/task`, `/workflow`

### Deployment Commands

**`/deploy $ARGUMENTS`**
```yaml
---
command: "/deploy"
category: "Deployment & Infrastructure"
purpose: "Deploy applications, configure CI/CD, manage releases"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [deployment-engineer, qa-tester, general-purpose]
  strategy: "sequential"
  telemetry: enabled
  fallback_chain: [software-architect, backend]
  auto_invoke: true
---
```
- **Auto-Persona**: DevOps, Security, QA
- **Auto-Agent**: deployment-engineer
- **MCP Integration**: Sequential, Context7
- **Tool Orchestration**: [Bash, Read, Write, Edit, TodoWrite]
- **Arguments**: `[target]`, `--env production|staging`, `--rollback`, `--dry-run`

### Optimization Commands

**`/optimize $ARGUMENTS`**
```yaml
---
command: "/optimize"
category: "Performance & Memory"
purpose: "Optimize performance, memory usage, and resource allocation"
wave-enabled: true
performance-profile: "optimization"
orchestration:
  default_agents: [memory-manager, general-purpose, software-architect]
  strategy: "sequential"
  telemetry: enabled
  fallback_chain: [backend, code-reviewer]
  auto_invoke: true
---
```
- **Auto-Persona**: Performance, Backend, Analyzer
- **Auto-Agent**: memory-manager (when --memory flag present)
- **MCP Integration**: Sequential, Playwright
- **Tool Orchestration**: [Read, Grep, Bash, Sequential, TodoWrite]
- **Arguments**: `[target]`, `--memory`, `--performance`, `--cache`, `--<flags>`

### Project Orchestration Commands

**`/taskmaster $ARGUMENTS`**
```yaml
---
command: "/taskmaster"
category: "Project Management & Orchestration"
purpose: "AI-powered project planning and task orchestration using TaskMaster AI"
wave-enabled: true
performance-profile: "complex"
orchestration:
  default_agents: [taskmaster, software-architect, general-purpose]
  strategy: "sequential"
  telemetry: enabled
  fallback_chain: [deployment-engineer, qa-tester, frontend-excellence]
  auto_invoke: true
---
```
- **Auto-Agent**: taskmaster (primary orchestrator)
- **Supporting Agents**: All agents based on task delegation
- **MCP Integration**: Sequential (planning), Context7 (best practices)
- **Tool Orchestration**: [Bash (task-master CLI), Read, Write, TodoWrite]
- **Arguments**: 
  - `init` - Initialize TaskMaster project
  - `parse-prd [file]` - Parse requirements into tasks
  - `plan` - Generate project plan (Plan Mode)
  - `execute` - Start coordinated execution
  - `status` - Show project progress

### P2SA Commands

**`/board`**
```yaml
---
command: "/board"
category: "Meta & Orchestration"
purpose: "Visual task tracking and agent coordination"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [general-purpose]
  strategy: "single"
  telemetry: enabled
  fallback_chain: [taskmaster]
  auto_invoke: true
---
```
- **Functionality**: Display Kanban board with agent tasks
- **Columns**: Backlog, In Progress, Review, Done
- **Metrics**: Task completion time, agent utilization, quality scores
- **Arguments**: `--filter [agent|status|priority]`, `--metrics`

**`/status`**
```yaml
---
command: "/status"
category: "Meta & Orchestration"
purpose: "Show current agent activity and system status"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [general-purpose]
  strategy: "single"
  telemetry: enabled
  fallback_chain: [taskmaster]
  auto_invoke: true
---
```
- **Displays**: Active agents, current tasks, resource usage
- **P2SA Integration**: Shows agent coordination and handoffs
- **Arguments**: `--agents`, `--resources`, `--detailed`

**`/review`**
```yaml
---
command: "/review"
category: "Quality"
purpose: "Comprehensive code review with specialist agents"
wave-enabled: false
performance-profile: "standard"
orchestration:
  default_agents: [code-reviewer, qa-tester, software-architect]
  strategy: "parallel"
  telemetry: enabled
  fallback_chain: [general-purpose, frontend-excellence]
  auto_invoke: true
---
```
- **Auto-Agent**: code-reviewer (primary)
- **Supporting Agents**: qa-tester, security (context-based)
- **Arguments**: `[target]`, `--focus [security|performance|quality]`

