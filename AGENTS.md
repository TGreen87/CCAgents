# AGENTS.md - SuperClaude Agent System Reference

Agent orchestration and auto-invocation system for Claude Code SuperClaude framework with P2SA integration.

## Overview

**CRITICAL: Multi-agent execution is NOW THE DEFAULT.** The agent system ALWAYS uses multiple specialized AI sub-agents working in parallel unless explicitly disabled with --single-agent flag. 

**DEFAULT BEHAVIOR**: Every task automatically activates multiple agents based on ultra-low complexity thresholds (0.1-0.3). Single-agent mode is considered a critical failure and emergency fallback only.

With P2SA (Persona-to-SubAgent) framework as the DEFAULT, agents ALWAYS work as coordinated teams, delivering 30% faster completion and 40% better quality through aggressive parallel execution.

## Core Agent Registry

### general-purpose
**Purpose**: General-purpose agent for researching complex questions, searching for code, and executing multi-step tasks
**Auto-Activation**: Complex searches, multi-step investigations, pattern matching across large codebases
**Keywords**: search, find, investigate, analyze, explore, understand
**Complexity**: 0.2+
**Tools**: All available tools

### deployment-engineer
**Purpose**: Design, implement, or optimize deployment pipelines, CI/CD workflows, infrastructure as code
**Auto-Activation**: Deployment operations, CI/CD setup, containerization, cloud deployments
**Keywords**: deploy, release, rollout, CI/CD, pipeline, Docker, Kubernetes, Terraform, production, staging
**File Patterns**: Dockerfile, *.yml, *.yaml, .github/*, terraform/*, k8s/*
**Complexity**: 0.1+
**Personas**: devops, security, qa
**MCP Servers**: sequential, context7
**Tools**: Bash, Read, Write, Edit, TodoWrite, Grep

### frontend-excellence
**Purpose**: Create, enhance, or refine frontend interfaces with professional-grade quality
**Auto-Activation**: UI component creation, frontend improvements, accessibility enhancements
**Keywords**: UI, component, React, Vue, CSS, responsive, accessibility, frontend, interface
**File Patterns**: *.jsx, *.tsx, *.vue, *.css, *.scss, components/*
**Complexity**: 0.2+
**Personas**: frontend, qa
**MCP Servers**: magic, context7, canva-dev
**Tools**: Write, Edit, MultiEdit, Read, WebFetch

### research-docs-api
**Purpose**: Retrieve current API documentation, best practices, technical information
**Auto-Activation**: Documentation lookups, API research, best practices queries
**Keywords**: documentation, API, docs, reference, guide, tutorial, how-to
**Complexity**: 0.1+
**MCP Servers**: context7, sequential
**Tools**: WebSearch, WebFetch, Read

### qa-tester
**Purpose**: Comprehensive quality assurance testing and test suite development
**Auto-Activation**: Test creation, quality validation, coverage analysis
**Keywords**: test, testing, QA, quality, coverage, unit test, integration test, e2e
**File Patterns**: *test*, *spec*, __tests__/*, tests/*
**Complexity**: 0.2+
**Personas**: qa
**MCP Servers**: playwright, sequential
**Tools**: Bash, Write, Edit, Read

### code-reviewer
**Purpose**: Review code for quality, readability, and best practices
**Auto-Activation**: After code changes, before commits, PR reviews
**Keywords**: review, check, validate, quality, standards
**Complexity**: 0.2+
**Personas**: refactorer, qa
**Tools**: Read, Grep, Git operations
**Telemetry Integration**: 
You actively report code review activities to the orchestration telemetry system:
- Code reviews performed and duration
- Issues identified by severity (critical/high/medium/low)
- Code quality metrics and patterns detected
- Review acceptance/rejection rates
- Improvement suggestions implemented

This helps the orchestration system track code quality trends and optimize review processes.

### software-architect
**Purpose**: Design system architecture, evaluate architectural decisions
**Auto-Activation**: System design, architectural planning, scalability assessment
**Keywords**: architecture, design, system, scalability, patterns, structure
**Complexity**: 0.3+
**Personas**: architect
**MCP Servers**: sequential, context7
**Tools**: Read, Write, Sequential thinking

### memory-manager
**Purpose**: Manage memory across Claude Code sessions, implement RAG systems
**Auto-Activation**: Memory optimization, caching strategies, session persistence
**Keywords**: memory, cache, persist, store, retrieve, RAG, vector, session
**File Patterns**: *memory*, *cache*, *store*, .claude/*
**Complexity**: 0.3+
**Personas**: performance, backend
**MCP Servers**: sequential
**Tools**: Read, Write, Edit, Bash

### taskmaster
**Purpose**: AI-powered project orchestration and task management system
**Auto-Activation**: PRD parsing, project planning, task coordination, multi-agent orchestration
**Keywords**: prd, project, task, plan, roadmap, sprint, epic, story, milestone, orchestrate, coordinate
**File Patterns**: *.prd, *requirements*.txt, *spec*.md, .taskmaster/*, prd.txt
**Complexity**: 0.1+
**Personas**: architect, analyzer, scribe
**MCP Servers**: sequential, context7
**Tools**: Bash (task-master CLI), Read, Write, TodoWrite
**Special**: Uses locally installed TaskMaster AI via 'task-master' command

## Auto-Invocation Rules

### Pattern Matching Algorithm
```yaml
invocation_score = 
  keyword_match * 0.3 +
  file_pattern_match * 0.2 +
  complexity_score * 0.25 +
  context_relevance * 0.15 +
  user_history * 0.1

threshold: 0.3 for auto-invocation  # Lowered for aggressive multi-agent default
```

### Context-Based Triggers

**Development Context**:
- New feature implementation → frontend-excellence or deployment-engineer
- Bug fixing → code-reviewer + qa-tester
- Performance issues → memory-manager
- Architecture decisions → software-architect

**Operation Context**:
- Pre-commit → code-reviewer
- Pre-deployment → deployment-engineer + qa-tester
- Documentation → research-docs-api
- System analysis → general-purpose + software-architect

## Agent Workflows

### Sequential Agent Execution
```yaml
workflow: code_quality_pipeline
  steps:
    1. code-reviewer: analyze changes
    2. qa-tester: validate functionality
    3. deployment-engineer: prepare release
  handoff: each agent passes findings to next
```

### Parallel Agent Coordination
```yaml
workflow: system_optimization
  parallel:
    - memory-manager: analyze memory usage
    - software-architect: review architecture
    - qa-tester: performance testing
  merge: combine findings for holistic view
```

### Hierarchical Agent Delegation
```yaml
workflow: complex_feature
  primary: general-purpose
  delegates:
    - frontend-excellence: UI components
    - deployment-engineer: infrastructure
    - qa-tester: test coverage
```

### TaskMaster Orchestration
```yaml
workflow: project_implementation
  orchestrator: taskmaster
  phases:
    1. planning:
       - parse PRD into structured tasks
       - analyze complexity and dependencies
       - identify agent assignments
    2. execution:
       - taskmaster delegates to specialized agents
       - frontend-excellence: UI tasks
       - backend: API tasks
       - qa-tester: testing tasks
       - deployment-engineer: release tasks
    3. coordination:
       - track progress across all agents
       - resolve dependencies
       - ensure integration points
```

## Agent Communication Protocol

### Information Sharing
- Agents share context through structured data formats
- Use TodoWrite for task coordination
- Maintain session memory for continuity

### Handoff Mechanisms
```yaml
handoff_protocol:
  from_agent: 
    - summarize findings
    - highlight key decisions
    - note unresolved issues
  to_agent:
    - acknowledge context
    - build on previous work
    - maintain consistency
```

## Quality Gates for Agents

### Pre-Invocation Validation
- Verify agent availability
- Check resource constraints
- Validate prerequisite conditions

### Post-Execution Validation
- Verify task completion
- Check output quality
- Ensure no regressions

## Emergency Protocols

### Agent Failure Handling
```yaml
failure_response:
  primary_failure:
    - attempt fallback agent
    - degrade gracefully
    - alert user
  
  cascade_prevention:
    - isolate failures
    - maintain partial results
    - provide recovery options
```

### Resource Management
```yaml
resource_limits:
  token_budget: agent-specific limits
  time_limits: operation timeouts
  concurrency: max parallel agents
```

## Integration with SuperClaude

### Command Integration
Agents auto-activate with commands:
- `/build` → frontend-excellence, deployment-engineer
- `/test` → qa-tester
- `/analyze` → general-purpose, software-architect
- `/deploy` → deployment-engineer
- `/improve` → code-reviewer, appropriate specialist

### Persona Synergy
Agents work with personas:
- frontend-excellence + frontend persona
- deployment-engineer + devops persona
- memory-manager + performance persona
- software-architect + architect persona

### MCP Server Coordination
Agents leverage appropriate MCP servers:
- frontend-excellence → magic, canva-dev
- research-docs-api → context7
- qa-tester → playwright
- All complex agents → sequential

## Agent Performance Metrics

### Success Metrics
```yaml
agent_metrics:
  invocation_accuracy: % correct auto-invocations
  task_completion: % successfully completed tasks
  handoff_success: % smooth transitions
  user_satisfaction: feedback scores
```

### Optimization Targets
- Reduce false positive invocations <5%
- Improve task completion rate >90%
- Minimize handoff friction
- Maximize value delivered per invocation