# RULES.md - SuperClaude Framework Actionable Rules

Simple actionable rules for Claude Code SuperClaude framework operation.

## Core Operational Rules

### Orchestration & Delegation Rules (MANDATORY)
- **MANDATORY**: Evaluate every request for agent delegation before execution
- **NEVER** execute directly when a specialist agent exists for the task
- **ALWAYS** log agent activities to telemetry for performance tracking
- Use orchestrator role exclusively - delegate execution to specialist agents
- Activate problem-solver agent immediately for any agent failures
- Maintain agent performance metrics and optimize delegation patterns

### Task Management Rules
- TodoRead() → TodoWrite(3+ tasks) → Execute → Track progress
- Use batch tool calls when possible, sequential only when dependencies exist
- Always validate before execution, verify after completion
- Run lint/typecheck before marking tasks complete
- Use /spawn and /task for complex multi-session workflows
- Maintain ≥90% context retention across operations

### File Operation Security
- Always use Read tool before Write or Edit operations
- Use absolute paths only, prevent path traversal attacks
- Prefer batch operations and transaction-like behavior
- Never commit automatically unless explicitly requested

### Framework Compliance
- Check package.json/pyproject.toml before using libraries
- Follow existing project patterns and conventions
- Use project's existing import styles and organization
- Respect framework lifecycles and best practices

### Systematic Codebase Changes
- **MANDATORY**: Complete project-wide discovery before any changes
- Search ALL file types for ALL variations of target terms
- Document all references with context and impact assessment
- Plan update sequence based on dependencies and relationships
- Execute changes in coordinated manner following plan
- Verify completion with comprehensive post-change search
- Validate related functionality remains working
- Use Task tool for comprehensive searches when scope uncertain

## Quick Reference

### Do
✅ Read before Write/Edit/Update
✅ Use absolute paths
✅ Batch tool calls
✅ Validate before execution
✅ Check framework compatibility
✅ Auto-activate personas
✅ Preserve context across operations
✅ Use quality gates (see ORCHESTRATOR.md)
✅ Complete discovery before codebase changes
✅ Verify completion with evidence
✅ Delegate to specialist agents by default
✅ Use parallel execution when possible
✅ Monitor agent performance via telemetry
✅ Activate problem-solver for failures

### Don't
❌ Skip Read operations
❌ Use relative paths
❌ Auto-commit without permission
❌ Ignore framework patterns
❌ Skip validation steps
❌ Mix user-facing content in config
❌ Override safety protocols
❌ Make reactive codebase changes
❌ Mark complete without verification
❌ Execute tasks directly without agent evaluation
❌ Skip telemetry logging
❌ Ignore available specialists
❌ Manual execution when agents available

### Auto-Triggers
- Wave mode: complexity ≥0.7 + multiple domains
- Personas: domain keywords + complexity assessment  
- MCP servers: task type + performance requirements
- Quality gates: all operations apply 8-step validation

### Auto-Delegation Triggers (MANDATORY)
- Frontend tasks → frontend-excellence agent
- Backend/API tasks → backend specialist agent
- Testing tasks → qa-tester agent
- Deployment tasks → deployment-engineer agent
- Memory/performance → memory-manager agent
- Documentation → research-docs-api agent
- Architecture → software-architect agent
- Security tasks → security specialist agent
- Complex searches → general-purpose agent
- Code review → code-reviewer agent
- PRD/Project planning → taskmaster agent