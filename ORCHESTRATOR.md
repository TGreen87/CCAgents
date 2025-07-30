# ORCHESTRATOR.md - SuperClaude Agent-First Orchestration System

**Primary Directive**: Every request must be evaluated for agent delegation. Claude Code orchestrates; agents execute.

## 🎯 Agent-First Detection Engine

Analyzes ALL requests to identify optimal agent delegation strategies. Default behavior is multi-agent orchestration unless explicitly overridden.

### Agent Delegation Decision Flow

```yaml
request_analysis:
  1_immediate_delegation_check:
    if: "request matches agent expertise"
    then: "delegate to specialist agent"
    confidence_threshold: 0.7
    
  2_multi_agent_evaluation:
    if: "request spans multiple domains"
    then: "coordinate multiple agents"
    max_agents: 10
    
  3_fallback_strategy:
    if: "no specialist matches"
    then: "delegate to general-purpose"
    never: "execute directly unless --single-agent"
```

### Pre-Operation Validation Checks

**Agent Availability Check** (NEW):
- Verify required agents are available
- Check agent health status from telemetry
- Confirm resource availability for delegation
- Prepare fallback chain if primary unavailable

**Resource Validation**:
- Token usage prediction based on operation complexity and scope
- Memory and processing requirements estimation
- File system permissions and available space verification
- MCP server availability and response time checks

**Compatibility Validation**:
- Flag combination conflict detection (e.g., `--no-mcp` with `--seq`)
- Persona + command compatibility verification
- Tool availability for requested operations
- Project structure requirements validation

**Risk Assessment**:
- Operation complexity scoring (0.0-1.0 scale)
- Failure probability based on historical patterns
- Resource exhaustion likelihood prediction
- Cascading failure potential analysis

**Validation Logic**: Resource availability, flag compatibility, risk assessment, outcome prediction, and safety recommendations. Operations with risk scores >0.8 trigger safe mode suggestions.

**Resource Management Thresholds**:
- **Green Zone** (0-60%): Full operations, predictive monitoring active
- **Yellow Zone** (60-75%): Resource optimization, caching, suggest --uc mode
- **Orange Zone** (75-85%): Warning alerts, defer non-critical operations  
- **Red Zone** (85-95%): Force efficiency modes, block resource-intensive operations
- **Critical Zone** (95%+): Emergency protocols, essential operations only

### Pattern Recognition Rules

#### Complexity Detection
```yaml
simple:
  indicators:
    - single file operations
    - basic CRUD tasks
    - straightforward queries
    - < 3 step workflows
  token_budget: 5K
  time_estimate: < 5 min

moderate:
  indicators:
    - multi-file operations
    - analysis tasks
    - refactoring requests
    - 3-10 step workflows
  token_budget: 15K
  time_estimate: 5-30 min

complex:
  indicators:
    - system-wide changes
    - architectural decisions
    - performance optimization
    - > 10 step workflows
  token_budget: 30K+
  time_estimate: > 30 min
```

#### Domain Identification with Agent Mapping
```yaml
frontend:
  keywords: [UI, component, React, Vue, CSS, responsive, accessibility, implement component, build UI]
  file_patterns: ["*.jsx", "*.tsx", "*.vue", "*.css", "*.scss"]
  typical_operations: [create, implement, style, optimize, test]
  primary_agent: frontend-excellence
  supporting_agents: [qa-tester, performance-profiler]

backend:
  keywords: [API, database, server, endpoint, authentication, performance, implement API, build service]
  file_patterns: ["*.js", "*.ts", "*.py", "*.go", "controllers/*", "models/*"]
  typical_operations: [implement, optimize, secure, scale]
  primary_agent: api-designer
  supporting_agents: [database-engineer, security-auditor]

infrastructure:
  keywords: [deploy, Docker, CI/CD, monitoring, scaling, configuration]
  file_patterns: ["Dockerfile", "*.yml", "*.yaml", ".github/*", "terraform/*"]
  typical_operations: [setup, configure, automate, monitor]
  primary_agent: deployment-engineer
  supporting_agents: [security-auditor, performance-profiler]

security:
  keywords: [vulnerability, authentication, encryption, audit, compliance]
  file_patterns: ["*auth*", "*security*", "*.pem", "*.key"]
  typical_operations: [scan, harden, audit, fix]
  primary_agent: security-auditor
  supporting_agents: [api-designer, database-engineer]

documentation:
  keywords: [document, README, wiki, guide, manual, instructions, commit, release, changelog]
  file_patterns: ["*.md", "*.rst", "*.txt", "docs/*", "README*", "CHANGELOG*"]
  typical_operations: [write, document, explain, translate, localize]
  primary_agent: documentation-specialist
  supporting_agents: [api-designer, test-architect]

data_analysis:
  keywords: [analyze, data, metrics, statistics, visualize, trend, pattern, report]
  file_patterns: ["*.csv", "*.json", "*.log", "*.metrics", "data/*", "analytics/*"]
  typical_operations: [analyze, visualize, report, predict]
  primary_agent: data-analyst
  supporting_agents: [performance-profiler, database-engineer]

testing:
  keywords: [test, testing, QA, coverage, automation, quality, verification]
  file_patterns: ["*test*", "*spec*", "__tests__/*", "tests/*", "cypress/*", "playwright/*"]
  typical_operations: [test, validate, automate, coverage]
  primary_agent: test-architect
  supporting_agents: [qa-tester, security-auditor]

performance:
  keywords: [performance, slow, optimize, bottleneck, profile, benchmark, latency]
  file_patterns: ["*profile*", "*benchmark*", "performance/*", "metrics/*"]
  typical_operations: [profile, optimize, benchmark, analyze]
  primary_agent: performance-profiler
  supporting_agents: [database-engineer, data-analyst]

problem_solving:
  keywords: [debug, fix, troubleshoot, resolve, investigate, error, failure]
  file_patterns: ["*.*"]  # Any file when debugging
  typical_operations: [diagnose, fix, recover, prevent]
  primary_agent: problem-solver
  supporting_agents: [ALL - coordinates any needed specialist]

general_tasks:
  keywords: [research, investigate, explore, implement, create, build]
  file_patterns: ["*.*"]  # Any file type
  typical_operations: [research, implement, integrate]
  primary_agent: general-purpose
  supporting_agents: [domain-specific as needed]
```

#### Operation Type Classification
```yaml
analysis:
  verbs: [analyze, review, explain, understand, investigate, troubleshoot]
  outputs: [insights, recommendations, reports]
  typical_tools: [Grep, Read, Sequential]

creation:
  verbs: [create, build, implement, generate, design]
  outputs: [new files, features, components]
  typical_tools: [Write, Magic, Context7]

implementation:
  verbs: [implement, develop, code, construct, realize]
  outputs: [working features, functional code, integrated components]
  typical_tools: [Write, Edit, MultiEdit, Magic, Context7, Sequential]

modification:
  verbs: [update, refactor, improve, optimize, fix]
  outputs: [edited files, improvements]
  typical_tools: [Edit, MultiEdit, Sequential]

debugging:
  verbs: [debug, fix, troubleshoot, resolve, investigate]
  outputs: [fixes, root causes, solutions]
  typical_tools: [Grep, Sequential, Playwright]

iterative:
  verbs: [improve, refine, enhance, correct, polish, fix, iterate, loop]
  outputs: [progressive improvements, refined results, enhanced quality]
  typical_tools: [Sequential, Read, Edit, MultiEdit, TodoWrite]

deployment:
  verbs: [deploy, release, rollout, ship, launch, publish, promote]
  outputs: [deployed services, released features, production builds]
  typical_tools: [Bash, deployment-engineer agent, Context7]
  auto_agents: [deployment-engineer]

memory_optimization:
  verbs: [optimize, cache, persist, store, retrieve, manage memory]
  outputs: [optimized memory usage, caching strategies, persistence layers]
  typical_tools: [memory-manager agent, Sequential, Read, Write]
  auto_agents: [memory-manager]

wave_operations:
  verbs: [comprehensively, systematically, thoroughly, progressively, iteratively]
  modifiers: [improve, optimize, refactor, modernize, enhance, audit, transform]
  outputs: [comprehensive improvements, systematic enhancements, progressive transformations]
  typical_tools: [Sequential, Task, Read, Edit, MultiEdit, Context7]
  wave_patterns: [review-plan-implement-validate, assess-design-execute-verify, analyze-strategize-transform-optimize]
```

### Intent Extraction Algorithm
```
1. Parse user request for keywords and patterns
2. Match against domain/operation matrices
3. Score complexity based on scope and steps
4. Evaluate wave opportunity scoring
5. Estimate resource requirements
6. Generate routing recommendation (traditional vs wave mode)
7. Apply auto-detection triggers for wave activation
```

**Enhanced Wave Detection Algorithm**:
- **Flag Overrides**: `--single-wave` disables, `--force-waves`/`--wave-mode` enables
- **Scoring Factors**: Complexity (0.2-0.4), scale (0.2-0.3), operations (0.2), domains (0.1), flag modifiers (0.05-0.1)
- **Thresholds**: Default 0.7, customizable via `--wave-threshold`, enterprise strategy lowers file thresholds
- **Decision Logic**: Sum all indicators, trigger waves when total ≥ threshold

## 🚦 Routing Intelligence

Dynamic decision trees that map detected patterns to optimal tool combinations, persona activation, and orchestration strategies.

### P2SA (Persona-to-SubAgent) Framework

**Multi-Agent Orchestration System** transforming personas into coordinated specialist teams.

**Core Components**:
- **Delegation Engine**: Analyzes tasks and assigns to optimal agents
- **Board Manager**: Visual task tracking with Kanban-style workflow
- **Agent Coordinator**: Manages inter-agent communication and handoffs
- **Resource Tracker**: Monitors system resources with graceful degradation

**P2SA Activation Logic**:
```yaml
p2sa_activation:
  automatic: # DEFAULT - ALWAYS ACTIVE
    - complexity >= 0.3 OR domains >= 1  # Much lower thresholds
    - ANY multi-step operation (>2 steps)
    - ANY multi-file operation
    - DEFAULT unless explicitly disabled
  modes:
    multi_agent: "DEFAULT - Full P2SA orchestration"
    hybrid: "Lead persona with supporting agents"
    single_agent: "RARE - Requires explicit --single-agent flag"
```

**Agent Selection Algorithm**:
```yaml
agent_score = 
  domain_match * 0.35 +
  expertise_level * 0.25 +
  availability * 0.20 +
  past_performance * 0.15 +
  resource_cost * 0.05
```

**Board States**:
- **Backlog**: Tasks awaiting assignment
- **In Progress**: Active agent execution
- **Review**: Quality validation phase
- **Done**: Completed with metrics

### Wave Orchestration Engine
Multi-stage command execution with compound intelligence. Automatic complexity assessment or explicit flag control. Now integrated with P2SA for agent-based wave execution.

**Wave Control Matrix**:
```yaml
wave-activation:
  automatic: "complexity >= 0.7"
  explicit: "--wave-mode, --force-waves"
  override: "--single-wave, --wave-dry-run"
  p2sa_default: "Multi-agent execution for ALL wave operations"
  
wave-strategies:
  progressive: "Incremental enhancement"
  systematic: "Methodical analysis"
  adaptive: "Dynamic configuration"
```

**Wave-Enabled Commands**:
- **Tier 1**: `/analyze`, `/build`, `/implement`, `/improve`
- **Tier 2**: `/design`, `/task`

### Master Routing Table (Agent-First)

| Pattern | Complexity | Primary Agent | Supporting Agents | Strategy | Confidence |
|---------|------------|---------------|-------------------|----------|------------|
| "analyze architecture" | complex | software-architect | data-analyst, documentation-specialist | parallel analysis | 95% |
| "create component" | simple | frontend-excellence | - | single agent | 90% |
| "implement feature" | moderate | [domain-specific] | qa-tester, documentation-specialist | sequential | 88% |
| "implement API" | moderate | api-designer | database-engineer, security-auditor | parallel design | 92% |
| "implement UI component" | simple | frontend-excellence | qa-tester | sequential | 94% |
| "implement authentication" | complex | security-auditor | api-designer, database-engineer | security-first | 90% |
| "fix bug" | moderate | problem-solver | [domain-specific], qa-tester | diagnostic-first | 95% |
| "optimize performance" | complex | performance-profiler | database-engineer, data-analyst | measure-first | 90% |
| "security audit" | complex | security-auditor | api-designer, database-engineer | comprehensive | 95% |
| "write documentation" | moderate | documentation-specialist | [domain-specific] | collaborative | 95% |
| "analyze data" | moderate | data-analyst | performance-profiler | data-driven | 93% |
| "design database" | moderate | database-engineer | api-designer, security-auditor | schema-first | 91% |
| "create tests" | moderate | test-architect | qa-tester, [domain-specific] | coverage-first | 92% |
| "improve code quality" | moderate | code-reviewer | test-architect, performance-profiler | iterative | 89% |
| "research solution" | simple | research-docs-api | general-purpose | exploratory | 87% |
| "handle errors" | complex | problem-solver | ALL available agents | recovery-focused | 96% |
| "deploy application" | complex | deployment-engineer | qa-tester, security-auditor | staged rollout | 95% |
| "setup CI/CD" | moderate | deployment-engineer | test-architect, security-auditor | automation-first | 90% |
| "profile application" | complex | performance-profiler | database-engineer, frontend-excellence | holistic | 91% |
| "design system" | complex | software-architect | ALL domain specialists | collaborative design | 94% |
| "migrate system" | complex | problem-solver | database-engineer, deployment-engineer | phased approach | 93% |
| "full system review" | complex | software-architect | ALL specialists | comprehensive | 95% |
| "implement end-to-end" | complex | general-purpose | ALL relevant specialists | orchestrated | 93% |
| "performance campaign" | complex | performance-profiler | data-analyst, database-engineer | data-driven optimization | 94% |
| "general task" | any | general-purpose | [context-specific] | adaptive | 85% |

### Decision Trees

#### Agent Selection Logic

**Primary Agent Selection**:
```yaml
selection_criteria:
  1_domain_match:
    weight: 0.40
    logic: "Match keywords and patterns to agent expertise"
    
  2_complexity_fit:
    weight: 0.25
    logic: "Match task complexity to agent capabilities"
    
  3_file_pattern:
    weight: 0.20
    logic: "Match file types to agent specialization"
    
  4_operation_type:
    weight: 0.15
    logic: "Match operation to agent strengths"

fallback_chain:
  1: "Domain specialist"
  2: "General-purpose"
  3: "Problem-solver" # For complex/unknown cases
```

**Multi-Agent Coordination**:
- **Parallel**: Independent tasks → concurrent execution
- **Sequential**: Dependent tasks → ordered execution
- **Hierarchical**: Complex tasks → coordinator + specialists
- **Adaptive**: Dynamic based on task evolution

**Delegation & Wave Evaluation**:
- **Delegation Score >0.6**: Add Task tool, auto-enable delegation flags based on scope
- **Wave Score >0.7**: Add Sequential for coordination, auto-enable wave strategies based on requirements

**Auto-Flag Assignment**:
- Directory count >7 → `--delegate --parallel-dirs`
- Focus areas >2 → `--multi-agent --parallel-focus`  
- High complexity + critical quality → `--wave-mode --wave-validation`
- Multiple operation types → `--wave-mode --adaptive-waves`

#### Task Delegation Intelligence

**Sub-Agent Delegation Decision Matrix**:

**Delegation Scoring Factors**:
- **Complexity >0.6**: +0.3 score
- **Parallelizable Operations**: +0.4 (scaled by opportunities/5, max 1.0)
- **High Token Requirements >15K**: +0.2 score  
- **Multi-domain Operations >2**: +0.1 per domain

**Wave Opportunity Scoring**:
- **High Complexity >0.8**: +0.4 score
- **Multiple Operation Types >2**: +0.3 score
- **Critical Quality Requirements**: +0.2 score
- **Large File Count >50**: +0.1 score
- **Iterative Indicators**: +0.2 (scaled by indicators/3)
- **Enterprise Scale**: +0.15 score

**Strategy Recommendations**:
- **Wave Score >0.7**: Use wave strategies
- **Directories >7**: `parallel_dirs`
- **Focus Areas >2**: `parallel_focus`  
- **High Complexity**: `adaptive_delegation`
- **Default**: `single_agent`

**Wave Strategy Selection**:
- **Security Focus**: `wave_validation`
- **Performance Focus**: `progressive_waves`
- **Critical Operations**: `wave_validation`
- **Multiple Operations**: `adaptive_waves`
- **Enterprise Scale**: `enterprise_waves`
- **Default**: `systematic_waves`

**Auto-Delegation Triggers**:
```yaml
directory_threshold:
  condition: directory_count > 7
  action: auto_enable --delegate --parallel-dirs
  confidence: 95%

file_threshold:
  condition: file_count > 50 AND complexity > 0.6
  action: auto_enable --delegate --sub-agents [calculated]
  confidence: 90%

multi_domain:
  condition: domains.length > 3
  action: auto_enable --delegate --parallel-focus
  confidence: 85%

complex_analysis:
  condition: complexity > 0.8 AND scope = comprehensive
  action: auto_enable --delegate --focus-agents
  confidence: 90%

token_optimization:
  condition: estimated_tokens > 20000
  action: auto_enable --delegate --aggregate-results
  confidence: 80%
```

**Wave Auto-Delegation Triggers**:
- Complex improvement: complexity > 0.8 AND files > 20 AND operation_types > 2 → --wave-count 5 (95%)
- Multi-domain analysis: domains > 3 AND tokens > 15K → --adaptive-waves (90%)
- Critical operations: production_deploy OR security_audit → --wave-validation (95%)
- Enterprise scale: files > 100 AND complexity > 0.7 AND domains > 2 → --enterprise-waves (85%)
- Large refactoring: large_scope AND structural_changes AND complexity > 0.8 → --systematic-waves --wave-validation (93%)

**Delegation Routing Table**:

| Operation | Complexity | Auto-Delegates | Performance Gain |
|-----------|------------|----------------|------------------|
| `/load @monorepo/` | moderate | --delegate --parallel-dirs | 65% |
| `/analyze --comprehensive` | high | --multi-agent --parallel-focus | 70% |
| Comprehensive system improvement | high | --wave-mode --progressive-waves | 80% |
| Enterprise security audit | high | --wave-mode --wave-validation | 85% |
| Large-scale refactoring | high | --wave-mode --systematic-waves | 75% |

**Sub-Agent Specialization Matrix**:
- **Quality**: qa persona, complexity/maintainability focus, Read/Grep/Sequential tools
- **Security**: security persona, vulnerabilities/compliance focus, Grep/Sequential/Context7 tools
- **Performance**: performance persona, bottlenecks/optimization focus, Read/Sequential/Playwright tools
- **Architecture**: architect persona, patterns/structure focus, Read/Sequential/Context7 tools
- **API**: backend persona, endpoints/contracts focus, Grep/Context7/Sequential tools

**Wave-Specific Specialization Matrix**:
- **Review**: analyzer persona, current_state/quality_assessment focus, Read/Grep/Sequential tools
- **Planning**: architect persona, strategy/design focus, Sequential/Context7/Write tools
- **Implementation**: intelligent persona, code_modification/feature_creation focus, Edit/MultiEdit/Task tools
- **Validation**: qa persona, testing/validation focus, Sequential/Playwright/Context7 tools
- **Optimization**: performance persona, performance_tuning/resource_optimization focus, Read/Sequential/Grep tools

#### Persona Auto-Activation System

**Multi-Factor Activation Scoring**:
- **Keyword Matching**: Base score from domain-specific terms (30%)
- **Context Analysis**: Project phase, urgency, complexity assessment (40%)
- **User History**: Past preferences and successful outcomes (20%)
- **Performance Metrics**: Current system state and bottlenecks (10%)

**Intelligent Activation Rules**:

**Performance Issues** → `--persona-performance` + `--focus performance`
- **Trigger Conditions**: Response time >500ms, error rate >1%, high resource usage
- **Confidence Threshold**: 85% for automatic activation

**Security Concerns** → `--persona-security` + `--focus security`
- **Trigger Conditions**: Vulnerability detection, auth failures, compliance gaps
- **Confidence Threshold**: 90% for automatic activation

**UI/UX Tasks** → `--persona-frontend` + `--magic`
- **Trigger Conditions**: Component creation, responsive design, accessibility
- **Confidence Threshold**: 80% for automatic activation

**Complex Debugging** → `--persona-analyzer` + `--think` + `--seq`
- **Trigger Conditions**: Multi-component failures, root cause investigation
- **Confidence Threshold**: 75% for automatic activation

**Documentation Tasks** → `--persona-scribe=en`
- **Trigger Conditions**: README, wiki, guides, commit messages, API docs
- **Confidence Threshold**: 70% for automatic activation

#### Flag Auto-Activation Patterns

**Context-Based Auto-Activation**:
- Performance issues → --persona-performance + --focus performance + --think
- Security concerns → --persona-security + --focus security + --validate
- UI/UX tasks → --persona-frontend + --magic + --c7
- Complex debugging → --think + --seq + --persona-analyzer
- Large codebase → --uc when context >75% + --delegate auto
- Testing operations → --persona-qa + --play + --validate
- DevOps operations → --persona-devops + --safe-mode + --validate
- Refactoring → --persona-refactorer + --wave-strategy systematic + --validate
- Iterative improvement → --loop for polish, refine, enhance keywords

**Wave Auto-Activation**:
- Complex multi-domain → --wave-mode auto when complexity >0.8 AND files >20 AND types >2
- Enterprise scale → --wave-strategy enterprise when files >100 AND complexity >0.7 AND domains >2
- Critical operations → Wave validation enabled by default for production deployments
- Legacy modernization → --wave-strategy enterprise --wave-delegation tasks
- Performance optimization → --wave-strategy progressive --wave-delegation files
- Large refactoring → --wave-strategy systematic --wave-delegation folders

**Sub-Agent Auto-Activation**:
- File analysis → --delegate files when >50 files detected
- Directory analysis → --delegate folders when >7 directories detected
- Mixed scope → --delegate auto for complex project structures
- High concurrency → --concurrency auto-adjusted based on system resources

**Loop Auto-Activation**:
- Quality improvement → --loop for polish, refine, enhance, improve keywords
- Iterative requests → --loop when "iteratively", "step by step", "incrementally" detected
- Refinement operations → --loop for cleanup, fix, correct operations on existing code

#### Flag Precedence Rules
1. Safety flags (--safe-mode) > optimization flags
2. Explicit flags > auto-activation
3. Thinking depth: --ultrathink > --think-hard > --think
4. --no-mcp overrides all individual MCP flags
5. Scope: system > project > module > file
6. Last specified persona takes precedence
7. Wave mode: --wave-mode off > --wave-mode force > --wave-mode auto
8. Sub-Agent delegation: explicit --delegate > auto-detection
9. Loop mode: explicit --loop > auto-detection based on refinement keywords
10. --uc auto-activation overrides verbose flags

### Confidence Scoring
Based on pattern match strength (40%), historical success rate (30%), context completeness (20%), resource availability (10%).

## Quality Gates & Agent Validation Framework

### Agent-Driven Quality Assurance
```yaml
agent_quality_gates:
  pre_delegation:
    - agent_availability: "Verify agents are responsive"
    - capability_match: "Confirm agent can handle task"
    - resource_check: "Ensure sufficient tokens/memory"
    - telemetry_health: "Check recent agent performance"
    
  during_execution:
    - progress_monitoring: "Track agent task completion"
    - quality_checkpoints: "Validate intermediate outputs"
    - resource_tracking: "Monitor token/time usage"
    - error_detection: "Catch and handle agent failures"
    
  post_execution:
    - output_validation: "Verify completeness and correctness"
    - synthesis_quality: "Ensure seamless integration"
    - telemetry_logging: "Record performance metrics"
    - feedback_loop: "Update agent performance data"

specialist_validation:
  code_quality: "code-reviewer agent validates all code"
  security: "security-auditor reviews sensitive operations"
  performance: "performance-profiler validates optimizations"
  testing: "test-architect ensures coverage"
  documentation: "documentation-specialist reviews docs"
  api_contracts: "api-designer validates interfaces"
  data_integrity: "database-engineer validates schema"
  
failure_recovery:
  primary_failure: "problem-solver agent takes over"
  cascade_prevention: "isolate failing agents"
  fallback_activation: "general-purpose as backup"
  telemetry_analysis: "data-analyst reviews failures"
```

### Task Completion Criteria
```yaml
completion_requirements:
  validation: "all 8 steps pass, evidence provided, metrics documented"
  ai_integration: "MCP coordination, persona integration, tool orchestration, ≥90% context retention"
  performance: "response time targets, resource limits, success thresholds, token efficiency"
  quality: "code quality standards, security compliance, performance assessment, integration testing"

evidence_requirements:
  quantitative: "performance/quality/security metrics, coverage percentages, response times"
  qualitative: "code quality improvements, security enhancements, UX improvements"
  documentation: "change rationale, test results, performance benchmarks, security scans"
```

## ⚡ Performance Optimization

Resource management, operation batching, and intelligent optimization for sub-100ms performance targets.

**Token Management**: Intelligent resource allocation based on unified Resource Management Thresholds (see Detection Engine section)

**Operation Batching**:
- **Tool Coordination**: Parallel operations when no dependencies
- **Context Sharing**: Reuse analysis results across related routing decisions
- **Cache Strategy**: Store successful routing patterns for session reuse
- **Task Delegation**: Intelligent sub-agent spawning for parallel processing
- **Resource Distribution**: Dynamic token allocation across sub-agents

**Resource Allocation**:
- **Detection Engine**: 1-2K tokens for pattern analysis
- **Decision Trees**: 500-1K tokens for routing logic
- **MCP Coordination**: Variable based on servers activated


## 🔗 Integration Intelligence

Smart MCP server selection and orchestration.

### MCP Server Selection Matrix
**Reference**: See MCP.md for detailed server capabilities, workflows, and integration patterns.

**Quick Selection Guide**:
- **Context7**: Library docs, framework patterns
- **Sequential**: Complex analysis, multi-step reasoning
- **Magic**: UI components, design systems
- **Playwright**: E2E testing, performance metrics

### Intelligent Server Coordination
**Reference**: See MCP.md for complete server orchestration patterns and fallback strategies.

**Core Coordination Logic**: Multi-server operations, fallback chains, resource optimization

### Persona Integration
**Reference**: See PERSONAS.md for detailed persona specifications and MCP server preferences.

## 🚨 Emergency Protocols

Handling resource constraints and failures gracefully.

### Resource Management
Threshold-based resource management follows the unified Resource Management Thresholds (see Detection Engine section above).

### Graceful Degradation
- **Level 1**: Reduce verbosity, skip optional enhancements, use cached results
- **Level 2**: Disable advanced features, simplify operations, batch aggressively
- **Level 3**: Essential operations only, maximum compression, queue non-critical

### Error Recovery Patterns
- **MCP Timeout**: Use fallback server
- **Token Limit**: Activate compression
- **Tool Failure**: Try alternative tool
- **Parse Error**: Request clarification




## 🔧 Configuration

### Orchestrator Settings
```yaml
orchestrator_config:
  # Performance
  enable_caching: true
  cache_ttl: 3600
  parallel_operations: true
  max_parallel: 3
  
  # Intelligence
  learning_enabled: true
  confidence_threshold: 0.7
  pattern_detection: aggressive
  
  # Resource Management
  token_reserve: 10%
  emergency_threshold: 90%
  compression_threshold: 75%
  
  # Wave Mode Settings
  wave_mode:
    enable_auto_detection: true
    wave_score_threshold: 0.7
    max_waves_per_operation: 5
    adaptive_wave_sizing: true
    wave_validation_required: true
```

### Custom Routing Rules
Users can add custom routing patterns via YAML configuration files.

## Agent Orchestration Best Practices

### Always Delegate First
```yaml
default_behavior:
  1_analyze_request: "Identify domain and complexity"
  2_select_agents: "Choose primary and supporting agents"
  3_delegate_task: "Use Task tool for delegation"
  4_monitor_progress: "Track via telemetry"
  5_synthesize_output: "Combine agent results"
  
never:
  - "Execute directly without agent evaluation"
  - "Ignore specialist when available"
  - "Skip telemetry logging"
```

### Optimal Agent Combinations
```yaml
proven_patterns:
  full_feature:
    agents: [api-designer, frontend-excellence, database-engineer, test-architect]
    strategy: "parallel design, sequential implementation"
    
  performance_optimization:
    agents: [performance-profiler, database-engineer, data-analyst]
    strategy: "measure → analyze → optimize → validate"
    
  security_hardening:
    agents: [security-auditor, api-designer, deployment-engineer]
    strategy: "audit → design → implement → verify"
    
  system_debugging:
    agents: [problem-solver, ALL specialists as needed]
    strategy: "diagnose → coordinate → fix → prevent"
```

### Telemetry-Driven Improvements
- Monitor agent performance metrics
- Identify bottlenecks and failures
- Adjust routing based on success rates
- Continuously optimize agent selection
