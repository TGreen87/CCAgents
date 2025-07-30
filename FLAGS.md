# FLAGS.md - SuperClaude Flag Reference

Flag system for Claude Code SuperClaude framework with auto-activation and conflict resolution.

## Flag System Architecture

**Priority Order**:
1. Explicit user flags override auto-detection
2. Safety flags override optimization flags
3. Performance flags activate under resource pressure
4. Persona flags based on task patterns
5. MCP server flags with context-sensitive activation
6. Wave flags based on complexity thresholds

## Planning & Analysis Flags

**`--plan`**
- Display execution plan before operations
- Shows tools, outputs, and step sequence

**`--think`**
- Multi-file analysis (~4K tokens)
- Enables Sequential MCP for structured problem-solving
- Auto-activates: Import chains >5 files, cross-module calls >10 references
- Auto-enables `--seq` and suggests `--persona-analyzer`

**`--think-hard`**
- Deep architectural analysis (~10K tokens)
- System-wide analysis with cross-module dependencies
- Auto-activates: System refactoring, bottlenecks >3 modules, security vulnerabilities
- Auto-enables `--seq --c7` and suggests `--persona-architect`

**`--ultrathink`**
- Critical system redesign analysis (~32K tokens)
- Maximum depth analysis for complex problems
- Auto-activates: Legacy modernization, critical vulnerabilities, performance degradation >50%
- Auto-enables `--seq --c7 --all-mcp` for comprehensive analysis

## Compression & Efficiency Flags

**`--uc` / `--ultracompressed`**
- 30-50% token reduction using symbols and structured output
- Auto-activates: Context usage >75% or large-scale operations
- Auto-generated symbol legend, maintains technical accuracy

**`--answer-only`**
- Direct response without task creation or workflow automation
- Explicit use only, no auto-activation

**`--validate`**
- Pre-operation validation and risk assessment
- Auto-activates: Risk score >0.7 or resource usage >75%
- Risk algorithm: complexity*0.3 + vulnerabilities*0.25 + resources*0.2 + failure_prob*0.15 + time*0.1

**`--safe-mode`**
- Maximum validation with conservative execution
- Auto-activates: Resource usage >85% or production environment
- Enables validation checks, forces --uc mode, blocks risky operations

**`--verbose`**
- Maximum detail and explanation
- High token usage for comprehensive output

## MCP Server Control Flags

**`--c7` / `--context7`**
- Enable Context7 for library documentation lookup
- Auto-activates: External library imports, framework questions
- Detection: import/require/from/use statements, framework keywords
- Workflow: resolve-library-id → get-library-docs → implement

**`--seq` / `--sequential`**
- Enable Sequential for complex multi-step analysis
- Auto-activates: Complex debugging, system design, --think flags
- Detection: debug/trace/analyze keywords, nested conditionals, async chains

**`--magic`**
- Enable Magic for UI component generation
- Auto-activates: UI component requests, design system queries
- Detection: component/button/form keywords, JSX patterns, accessibility requirements

**`--play` / `--playwright`**
- Enable Playwright for cross-browser automation and E2E testing
- Detection: test/e2e keywords, performance monitoring, visual testing, cross-browser requirements

**`--all-mcp`**
- Enable all MCP servers simultaneously
- Auto-activates: Problem complexity >0.8, multi-domain indicators
- Higher token usage, use judiciously

**`--no-mcp`**
- Disable all MCP servers, use native tools only
- 40-60% faster execution, WebSearch fallback

**`--no-[server]`**
- Disable specific MCP server (e.g., --no-magic, --no-seq)
- Server-specific fallback strategies, 10-30% faster per disabled server

## Sub-Agent Delegation Flags

**`--delegate [files|folders|auto]`**
- Enable Task tool sub-agent delegation for parallel processing
- **files**: Delegate individual file analysis to sub-agents
- **folders**: Delegate directory-level analysis to sub-agents  
- **auto**: Auto-detect delegation strategy based on scope and complexity
- Auto-activates: >7 directories or >50 files
- 40-70% time savings for suitable operations

**`--concurrency [n]`**
- Control max concurrent sub-agents and tasks (default: 7, range: 1-15)
- Dynamic allocation based on resources and complexity
- Prevents resource exhaustion in complex scenarios

## Orchestration Control Flags

**⚡ IMPORTANT: Multi-agent orchestration is ALWAYS ACTIVE by default for optimal quality and performance**

**`--single-agent`** ⚠️ **EMERGENCY USE ONLY**
- Forces degraded single-agent execution mode
- **WARNING**: Reduces quality by 40%, increases execution time by 60%
- Only use when system resources critically constrained (>95%)
- Automatically triggers warning about performance penalties
- Example: `/analyze --single-agent` # NOT RECOMMENDED

**`--orchestrate`** 🚫 **DEPRECATED**
- Multi-agent orchestration is now ALWAYS ON by default
- This flag has no effect and will be ignored
- To disable orchestration, use `--single-agent` (NOT RECOMMENDED)

**`--agent-transparency`**
- Show real-time agent activities and handoffs
- Displays which agents are working on which tasks
- Includes performance metrics and coordination details
- Useful for understanding multi-agent workflows

**`--telemetry-verbose`**
- Detailed telemetry logging for all agent operations
- Includes timing, resource usage, and decision rationale
- Automatically enabled for complex operations
- Helps optimize agent coordination patterns

**`--no-telemetry`** ⚠️ **NOT RECOMMENDED**
- Disable telemetry collection (reduces optimization capability)
- May impact automatic agent selection quality
- Only use if required by privacy constraints
- Warning: Disables learning and improvement features

**`--preferred-agents [agent1,agent2,...]`**
- Suggest preferred agents while maintaining orchestration
- Does NOT disable automatic agent selection
- System may still activate additional agents as needed
- Example: `--preferred-agents frontend-excellence,qa-tester`

**`--max-agents [n]`**
- Limit maximum concurrent agents (default: 10, range: 1-20)
- Lower values may increase execution time
- System automatically optimizes within constraints
- Example: `--max-agents 5` for resource-limited environments

**`--agent-timeout [s]`**
- Set timeout for individual agent operations (default: 120s)
- Range: 30-600 seconds
- Shorter timeouts may cause premature agent termination
- Example: `--agent-timeout 300` for complex operations

## P2SA (Persona-to-SubAgent) Flags

**`--agents [agent1,agent2,...]`**
- Explicitly specify which agents to use for task execution
- Refines automatic agent selection (doesn't disable multi-agent)
- Example: --agents frontend-excellence,qa-tester,deployment-engineer

**`--p2sa [auto|force|off]`** ⚠️ **DEPRECATED - Multi-agent is now default**
- **auto**: DEFAULT - Multi-agent always active (complexity threshold lowered to 0.3)
- **force**: Redundant - multi-agent is already default
- **off**: ⚠️ NOT RECOMMENDED - Use `--single-agent` instead

**`--board`**
- Enable visual board tracking for agent tasks
- Shows Kanban-style progress: Backlog → In Progress → Review → Done
- Real-time updates with performance metrics

**`--agent-mode [parallel|sequential|hybrid]`**
- Control agent execution strategy
- **parallel**: Execute independent tasks concurrently
- **sequential**: Execute tasks in order with handoffs
- **hybrid**: Mix of parallel and sequential based on dependencies

## TaskMaster Orchestration Flags

**`--taskmaster [auto|init|parse|plan]`**
- Control TaskMaster AI integration
- **auto**: Automatic TaskMaster activation for PRDs (default)
- **init**: Initialize TaskMaster project structure
- **parse**: Parse PRD into structured tasks
- **plan**: Generate comprehensive project plan

**`--task-complexity [1-10]`**
- Override TaskMaster complexity scoring
- Lower values (1-3): Simple tasks, single agent
- Medium values (4-6): Moderate complexity, may need breakdown
- Higher values (7-10): Complex tasks, multi-agent coordination

**`--task-delegation [auto|manual|guided]`**
- Control how TaskMaster delegates to agents
- **auto**: TaskMaster automatically assigns agents
- **manual**: User specifies agent assignments
- **guided**: TaskMaster suggests, user confirms

## Wave Orchestration Flags

**`--wave-mode [auto|force|off]`**
- Control wave orchestration activation
- **auto**: Auto-activates based on complexity >0.8 AND file_count >20 AND operation_types >2
- **force**: Override auto-detection and force wave mode for borderline cases
- **off**: Disable wave mode, use Sub-Agent delegation instead
- 30-50% better results through compound intelligence and progressive enhancement

**`--wave-strategy [progressive|systematic|adaptive|enterprise]`**
- Select wave orchestration strategy
- **progressive**: Iterative enhancement for incremental improvements
- **systematic**: Comprehensive methodical analysis for complex problems
- **adaptive**: Dynamic configuration based on varying complexity
- **enterprise**: Large-scale orchestration for >100 files with >0.7 complexity
- Auto-selects based on project characteristics and operation type

**`--wave-delegation [files|folders|tasks]`**
- Control how Wave system delegates work to Sub-Agent
- **files**: Sub-Agent delegates individual file analysis across waves
- **folders**: Sub-Agent delegates directory-level analysis across waves
- **tasks**: Sub-Agent delegates by task type (security, performance, quality, architecture)
- Integrates with `--delegate` flag for coordinated multi-phase execution

## Scope & Focus Flags

**`--scope [level]`**
- file: Single file analysis
- module: Module/directory level
- project: Entire project scope
- system: System-wide analysis

**`--focus [domain]`**
- performance: Performance optimization
- security: Security analysis and hardening
- quality: Code quality and maintainability
- architecture: System design and structure
- accessibility: UI/UX accessibility compliance
- testing: Test coverage and quality

## Iterative Improvement Flags

**`--loop`**
- Enable iterative improvement mode for commands
- Auto-activates: Quality improvement requests, refinement operations, polish tasks
- Compatible commands: /improve, /refine, /enhance, /fix, /cleanup, /analyze
- Default: 3 iterations with automatic validation

**`--iterations [n]`**
- Control number of improvement cycles (default: 3, range: 1-10)
- Overrides intelligent default based on operation complexity

**`--interactive`**
- Enable user confirmation between iterations
- Pauses for review and approval before each cycle
- Allows manual guidance and course correction

## Persona Activation Flags

**Available Personas**:
- `--persona-architect`: Systems architecture specialist
- `--persona-frontend`: UX specialist, accessibility advocate
- `--persona-backend`: Reliability engineer, API specialist
- `--persona-analyzer`: Root cause specialist
- `--persona-security`: Threat modeler, vulnerability specialist
- `--persona-mentor`: Knowledge transfer specialist
- `--persona-refactorer`: Code quality specialist
- `--persona-performance`: Optimization specialist
- `--persona-qa`: Quality advocate, testing specialist
- `--persona-devops`: Infrastructure specialist
- `--persona-scribe=lang`: Professional writer, documentation specialist

## Introspection & Transparency Flags

**`--introspect` / `--introspection`**
- Deep transparency mode exposing thinking process
- Auto-activates: SuperClaude framework work, complex debugging
- Transparency markers: 🤔 Thinking, 🎯 Decision, ⚡ Action, 📊 Check, 💡 Learning
- Conversational reflection with shared uncertainties

## Flag Integration Patterns

### MCP Server Auto-Activation

**Auto-Activation Logic**:
- **Context7**: External library imports, framework questions, documentation requests
- **Sequential**: Complex debugging, system design, any --think flags  
- **Magic**: UI component requests, design system queries, frontend persona
- **Playwright**: Testing workflows, performance monitoring, QA persona

### Flag Precedence

1. **P2SA Multi-Agent DEFAULT** > all other flags (unless --single-agent emergency override)
2. Safety flags (--safe-mode) > optimization flags
3. Explicit flags > auto-activation
4. Thinking depth: --ultrathink > --think-hard > --think
5. --no-mcp overrides all individual MCP flags
6. Scope: system > project > module > file
7. Last specified persona takes precedence
8. Wave mode: --wave-mode off > --wave-mode force > --wave-mode auto
9. Sub-Agent delegation: explicit --delegate > auto-detection
10. Loop mode: explicit --loop > auto-detection based on refinement keywords
11. --uc auto-activation overrides verbose flags

### Context-Based Auto-Activation

**Multi-Agent Orchestration**: ALWAYS ACTIVE by default (complexity threshold lowered to 0.3)
**Telemetry Auto-Activation**: Enabled for all operations to optimize agent selection
**Agent Transparency**: Auto-enabled for complexity >0.8 or >3 agents active
**Wave Auto-Activation**: complexity ≥0.7 AND files >20 AND operation_types >2
**Sub-Agent Auto-Activation**: >7 directories OR >50 files OR complexity >0.8
**Loop Auto-Activation**: polish, refine, enhance, improve keywords detected