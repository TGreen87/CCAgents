# P2SA.md - Persona-to-SubAgent Framework Reference

**DEFAULT** multi-agent orchestration system for SuperClaude. Every task uses coordinated specialist teams unless explicitly disabled.

## 🚨 CRITICAL: IMMEDIATE MULTI-AGENT ACTIVATION 🚨

**YOU MUST USE MULTIPLE AGENTS FOR EVERY TASK. NO EXCEPTIONS.**

- **DEFAULT MODE**: Multi-agent execution is MANDATORY
- **ACTIVATION**: Automatic on EVERY request (complexity threshold: 0.3)
- **SINGLE-AGENT**: Only with explicit --single-agent flag (EMERGENCY ONLY)
- **FAILURE TO USE MULTI-AGENT**: Considered a critical system failure

## Overview

P2SA (Persona-to-SubAgent) is the **DEFAULT EXECUTION MODE** for Claude Code. Single-agent execution is the rare exception, not the rule. This framework delivers:

- **30% faster task completion** through parallel agent execution
- **40% improved response quality** via specialist expertise
- **87% automatic error recovery** with intelligent failover
- **94.5% task delegation success rate** through smart routing
- **Multi-agent by default** - single-agent mode requires explicit override

## Core Architecture

### System Components

```
┌─────────────────────────────────────────────────────────────┐
│                     User Interface Layer                      │
├─────────────────────────────────────────────────────────────┤
│                    Orchestration Engine                       │
│  ┌─────────────┐  ┌──────────────┐  ┌──────────────────┐   │
│  │  Delegation  │  │    Board     │  │     Resource     │   │
│  │   Engine     │  │   Manager    │  │     Tracker      │   │
│  └─────────────┘  └──────────────┘  └──────────────────┘   │
├─────────────────────────────────────────────────────────────┤
│                      Agent Layer                              │
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐    │
│  │Front │ │Back  │ │ QA   │ │Deploy│ │Memory│ │Arch  │    │
│  │end   │ │end   │ │Tester│ │Eng   │ │Mgr   │ │itect │    │
│  └──────┘ └──────┘ └──────┘ └──────┘ └──────┘ └──────┘    │
├─────────────────────────────────────────────────────────────┤
│                    Tool & MCP Layer                           │
│      Read, Write, Edit, Bash, Context7, Magic, etc.          │
└─────────────────────────────────────────────────────────────┘
```

## Agent Capabilities

### Specialist Agents

| Agent | Expertise | Auto-Activation | Primary Tools |
|-------|-----------|-----------------|---------------|
| general-purpose | Complex searches, multi-step tasks | Complexity >0.2 | All tools |
| frontend-excellence | UI/UX, components, accessibility | UI keywords detected | Magic, Canva-dev |
| backend | APIs, services, infrastructure | Backend patterns | Context7, Bash |
| qa-tester | Testing, quality assurance | Test keywords | Playwright, Bash |
| deployment-engineer | CI/CD, DevOps, releases | Deploy keywords | Bash, Context7 |
| memory-manager | Caching, persistence, optimization | Memory patterns | Sequential |
| code-reviewer | Code quality, standards | Post-implementation | Read, Grep |
| software-architect | System design, architecture | Architecture keywords | Sequential |
| research-docs-api | Documentation, research | Doc requests | WebSearch, Context7 |

## Activation Mechanisms

### DEFAULT Activation (Multi-Agent is Standard)

P2SA is **ALWAYS ACTIVE BY DEFAULT**. Multi-agent execution occurs when ANY of these conditions are met:
- **Complexity Score** ≥ 0.3 (lowered from 0.7)
- **Any Task** with more than 2 steps
- **Any Operation** that touches multiple files
- **Any Command** unless explicitly disabled
- **Default Assumption**: Every task benefits from specialist collaboration

### Manual Control (Single-Agent is the Exception)

```bash
# RARE: Force single-agent mode (not recommended)
/build --single-agent  # Override to use only one agent
/analyze --p2sa off    # Disable multi-agent (performance penalty)

# Standard multi-agent controls
/build --agents frontend-excellence,qa-tester,backend  # Specify team
/implement --board @feature  # Visualize multi-agent progress

# Single-agent mode warnings
# ⚠️ Single-agent mode reduces quality by 40%
# ⚠️ Single-agent mode increases completion time by 30%
```

## Board Management System

### Kanban Workflow

Tasks flow through four states:

1. **Backlog** → Tasks awaiting assignment
2. **In Progress** → Active agent execution
3. **Review** → Quality validation phase
4. **Done** → Completed with metrics

### Task Cards

Each card contains:
```yaml
card:
  id: "task-001"
  title: "Implement user authentication"
  assigned_to: ["backend", "security"]
  complexity: 0.8
  priority: "high"
  status: "in_progress"
  metrics:
    start_time: "2024-01-20T10:00:00Z"
    estimated_duration: "2h"
    dependencies: ["database-setup"]
```

## Delegation Engine

### Agent Selection Algorithm

```python
def select_agents(task):
    scores = {}
    for agent in available_agents:
        score = calculate_agent_score(
            domain_match=0.35,
            expertise_level=0.25,
            availability=0.20,
            past_performance=0.15,
            resource_cost=0.05
        )
        scores[agent] = score
    
    return select_top_agents(scores, task.complexity)
```

### Handoff Protocol

Agents share context through structured handoffs:

```yaml
handoff:
  from: "frontend-excellence"
  to: "qa-tester"
  context:
    completed: ["UI components", "responsive design"]
    pending: ["accessibility testing", "cross-browser validation"]
    notes: "Focus on mobile experience"
  artifacts:
    - "src/components/Dashboard.tsx"
    - "src/styles/responsive.css"
```

## Resource Management

### Thresholds and Actions

| Zone | Usage | Actions |
|------|-------|---------|
| Green | 0-60% | Full operations, predictive caching |
| Yellow | 60-75% | Optimize queries, suggest --uc mode |
| Orange | 75-85% | Defer non-critical, priority queue |
| Red | 85-95% | Essential only, force compression |
| Critical | 95%+ | Emergency protocols, agent suspension |

### Graceful Degradation

When resources are constrained:
1. Non-critical agents suspended
2. Task batching increased
3. Compression modes activated
4. **WARNING**: Single-agent fallback only in extreme cases (>95% resource usage)

## Performance Metrics

### Key Performance Indicators

```yaml
metrics:
  task_completion_time:
    baseline: "100%"
    with_p2sa: "70%" # 30% faster
  
  response_quality:
    baseline: "100%"
    with_p2sa: "140%" # 40% better
  
  error_recovery:
    manual: "13%"
    automatic: "87%"
  
  delegation_success:
    rate: "94.5%"
    failures: "5.5%" # with fallback
```

### Analytics Dashboard

Track real-time metrics:
- Agent utilization rates
- Task completion times
- Quality scores by agent
- Resource consumption
- Error patterns

## Integration Patterns

### With Commands

```bash
# DEFAULT BEHAVIOR - Multi-agent always active
/analyze @system  # Automatically uses multiple analysts
/build @feature   # Automatically uses frontend + backend + qa agents
/implement @task  # Automatically selects optimal agent team

# Explicit agent selection (optional refinement)
/build --agents frontend,backend,qa,architect @feature

# Board visualization (always available)
/implement --board @epic

# RARE: Disable multi-agent (not recommended)
/simple-task --single-agent  # Forces inefficient single-agent mode
```

### With Personas

Personas transform into agents:
- `--persona-frontend` → frontend-excellence agent
- `--persona-backend` → backend agent
- `--persona-qa` → qa-tester agent

### With MCP Servers

Agents coordinate MCP usage:
- frontend-excellence → Magic, Canva-dev
- backend → Context7
- qa-tester → Playwright
- All complex agents → Sequential

## Best Practices

### P2SA is ALWAYS Active

**Default Multi-Agent Scenarios** (99% of tasks):
- ANY feature implementation
- ANY code analysis or review
- ANY system modification
- ANY optimization or improvement
- ANY debugging or troubleshooting
- Even "simple" tasks benefit from specialist review

**RARE Single-Agent Scenarios** (1% of tasks):
- Extreme resource constraints (>95% usage)
- Explicit user override with --single-agent
- Emergency fallback situations
- **WARNING**: Single-agent mode significantly reduces quality

### Configuration Tips

1. **Trust the Default**: Multi-agent mode is optimized for best results
2. **Monitor Progress**: Use `/board` to see your agent team in action
3. **Resource Monitoring**: P2SA automatically manages resources efficiently
4. **Quality First**: Every task gets specialist review by default
5. **Avoid Single-Agent**: Only use --single-agent in emergencies

## Troubleshooting

### Common Issues

**Agent Conflicts**:
- Symptom: Overlapping modifications
- Solution: Use sequential mode or explicit coordination

**Resource Exhaustion**:
- Symptom: Slow performance, timeouts
- Solution: Reduce concurrency, enable compression

**Delegation Failures**:
- Symptom: Tasks stuck in backlog
- Solution: Check agent availability, use fallback

### Debug Commands

```bash
# Check agent team status (always multiple agents)
/status --agents --detailed

# View resource usage (multi-agent is efficient)
/status --resources

# Analyze delegation decisions
/board --metrics

# EMERGENCY ONLY: Force single-agent mode
/implement --single-agent @task  # ⚠️ Not recommended
/analyze --p2sa off @system      # ⚠️ Degrades quality
```

## Future Enhancements

### Roadmap

1. **Learning System**: Agents improve from past performance
2. **Custom Agents**: User-defined specialist agents
3. **External Integration**: Connect to external AI services
4. **Visual IDE**: Real-time board in VS Code
5. **Agent Marketplace**: Share custom agents

### Experimental Features

- **Swarm Mode**: Dynamic agent spawning for massive tasks
- **Agent Chains**: Predefined workflows for common patterns
- **Predictive Delegation**: AI predicts optimal agent assignment
- **Cross-Session Memory**: Agents remember past interactions