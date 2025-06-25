# CLI Programming Agent Planning Guidelines

This document provides systematic planning strategies for CLI programming agents, optimized for efficient task decomposition, parallel execution, and intelligent tool usage. Focus on actionable commands and measurable outcomes.

## 🎯 Planning Principles

### 1. Optimize for CLI Agent Efficiency (HIGHEST PRIORITY)
- **Execute tasks in parallel when possible** - use Task tool for concurrent operations
- **Chunk work into independent, measurable steps** that fit within context limits
- **Plan frequent commits** - run `git add . && git commit -m "checkpoint: [description]"` after 2-3 completed tasks
- **Use Task tool for complex searches** - delegate file discovery, pattern matching, and codebase analysis
- **Batch CLI commands** - combine related operations: `npm run lint && npm run typecheck && npm test`

### 2. Understand Before You Code
- Read and comprehend requirements fully
- Identify all stakeholders and dependencies
- Clarify ambiguities before starting

### 3. Break Down Complex Tasks
- Decompose large features into smaller, testable units
- Create logical groupings of related functionality
- Define clear boundaries between components
- **Each unit should be small enough for AI to handle in one session**

### 4. Consider the Full Stack
- Frontend implications
- Backend requirements
- Database changes
- Infrastructure needs
- Security considerations

## 📋 The Planning Process

**IMPORTANT**: Check off each item (□ → ✓) as you complete it to track progress and ensure nothing is missed.

### Step 1: Requirements Analysis & Initial Discovery
**CLI Actions:**
```bash
# Parallel file discovery (use Task tool for complex searches)
Task: "Search for similar features in codebase"
Task: "Find existing API patterns and endpoints" 
Task: "Identify relevant configuration files"

# Quick project structure analysis
find . -name "*.json" -o -name "*.yml" -o -name "*.config.*" | head -20
ls -la src/ components/ pages/ 2>/dev/null | head -20
```

**MCP Commands:**
```javascript
// Break down complex requirements
mcp__sequential-thinking__sequentialthinking({
  thought: "Analyze user requirement and decompose into ordered, testable tasks",
  totalThoughts: 5
})

// Discover existing patterns
mcp__context7__resolve-library-id({ libraryName: "[relevant framework]" })
```

**Checklist:**
```
□ Run parallel discovery tasks using Task tool
□ Use sequentialthinking to decompose complex requirements
□ Execute `git log --oneline --grep="[similar feature]" | head -10` to find related commits
□ Run `grep -r "[key terms]" src/ --include="*.js" --include="*.ts" | head -20`
□ Identify core functionality and user interactions
□ Document technical constraints and dependencies
□ List clarification questions
```

### Step 2: Technical Discovery & Codebase Analysis
**Parallel CLI Discovery (use Task tool for efficiency):**
```bash
# Task 1: Package analysis
Task: "Analyze package.json dependencies and scripts"
# Command: cat package.json | jq '.dependencies, .devDependencies, .scripts'

# Task 2: Component discovery  
Task: "Find all components and their patterns"
# Command: find src/ -name "*.tsx" -o -name "*.jsx" | xargs grep -l "export.*component\|export default" | head -20

# Task 3: API endpoint discovery
Task: "Locate API routes and endpoint patterns"
# Command: grep -r "app\.(get\|post\|put\|delete)\|router\|endpoint" --include="*.js" --include="*.ts" src/ api/ | head -15
```

**MCP Deep Analysis:**
```javascript
// TypeScript-aware codebase analysis
mcp__context7__resolve-library-id({ libraryName: "[current framework]" })
mcp__context7__get-library-docs({ 
  context7CompatibleLibraryID: "/[resolved-id]",
  topic: "component patterns",
  tokens: 5000
})

// IDE diagnostics for current state
mcp__ide__getDiagnostics()
```

**CLI Validation Commands:**
```bash
# Check project health in parallel
npm run lint &
npm run typecheck &
npm run test:quick &
wait  # Wait for parallel commands to complete

# Analyze bundle and dependencies
npx depcheck  # Find unused dependencies
npx bundle-analyzer --no-open  # Check bundle size
```

**Checklist:**
```
□ Launch parallel discovery tasks using Task tool
□ Use context7 for framework-specific analysis
□ Run IDE diagnostics to identify existing issues
□ Execute health check commands in parallel
□ Identify reusable patterns and components
□ Document technical challenges and constraints
```

### Step 3: Solution Design & Rapid Prototyping
**Architecture Commands:**
```bash
# Create solution branch for experimentation
git checkout -b feature/[solution-name]
git push -u origin feature/[solution-name]

# Set up rapid prototyping environment
mkdir -p prototypes/[feature-name]/{schemas,mocks,components}
```

**MCP Prototyping Workflow:**
```javascript
// Rapid schema design and testing
mcp__sqlite__create_table({
  query: "CREATE TABLE [entity] (id INTEGER PRIMARY KEY, ...)"
})
mcp__sqlite__write_query({
  query: "INSERT INTO [entity] VALUES (...)"
})

// API contract definition and mocking
mcp__fetch__fetch({
  url: "https://jsonplaceholder.typicode.com/posts",  // Mock similar API
  raw: false
})

// Component scaffolding
mcp__context7__resolve-library-id({ libraryName: "[component library]" })
mcp__magicui__getUIComponents()  // If using MagicUI
```

**CLI Prototyping Commands:**
```bash
# Quick component generation
npx create-component [ComponentName] --typescript --test

# API endpoint scaffolding
curl -X POST http://localhost:3000/api/[endpoint] -H "Content-Type: application/json" -d '{"test": "data"}'

# Database migration dry run
npm run db:migrate:dry-run
```

**Checklist:**
```
□ Create feature branch with `git checkout -b feature/[name]`
□ Use sqlite for rapid schema prototyping
□ Use fetch to mock and test API contracts early
□ Scaffold components using context7 and available libraries
□ Define clear interfaces and error handling
□ Test edge cases with curl/fetch commands
```

### Step 4: Task Breakdown & Parallel Execution Planning
**Task Decomposition with MCP:**
```javascript
// Break complex features into CLI-executable tasks
mcp__sequential-thinking__sequentialthinking({
  thought: "Create ordered, independent tasks that can be executed in parallel where possible",
  totalThoughts: 8
})
```

**Parallel Task Identification:**
```bash
# Identify independent tasks that can run concurrently
echo "PARALLEL TASKS:"
echo "- Frontend component development (Task A)"
echo "- Backend API implementation (Task B)" 
echo "- Database schema updates (Task C)"
echo "- Test suite creation (Task D)"

echo "SEQUENTIAL DEPENDENCIES:"
echo "- Schema (C) -> API (B) -> Frontend (A) integration"
echo "- Components (A) -> E2E tests (D)"
```

**Task Execution Commands:**
```bash
# Checkpoint commit template
git add . && git commit -m "checkpoint: [task-description] - [what-completed]"

# Parallel development setup
npm run dev &          # Development server
npm run test:watch &   # Test watcher  
npm run lint:watch &   # Linting watcher
echo "All watchers started in parallel"
```

**Puppeteer Test Planning:**
```javascript
// Define comprehensive E2E test criteria
mcp__puppeteer__puppeteer_navigate({ url: "http://localhost:3000/[feature]" })
mcp__puppeteer__puppeteer_screenshot({ name: "baseline-[feature]", selector: "body" })

// Performance benchmarking setup
mcp__puppeteer__puppeteer_evaluate({
  script: `
    const start = performance.now();
    // Trigger feature functionality
    const end = performance.now();
    return { executionTime: end - start };
  `
})
```

**Checklist:**
```
□ Use sequentialthinking to create ordered, measurable tasks
□ Identify tasks that can be executed in parallel using Task tool
□ Plan checkpoint commits: `git commit -m "checkpoint: [description]"`
□ Set up parallel development environment (dev server, watchers)
□ Define puppeteer test criteria for each major interaction
□ Create performance benchmarks with specific metrics
□ Mark Task tool delegation points for complex searches/analysis
```

### Step 5: Risk Assessment & Mitigation Commands
**Automated Risk Detection:**
```bash
# Security vulnerability scanning
npm audit --audit-level=moderate
npx audit-ci --moderate &

# Performance impact analysis
npx bundle-analyzer --no-open &
npx lighthouse-ci autorun --collect.numberOfRuns=3 &

wait  # Wait for parallel scans
```

**MCP Risk Analysis:**
```javascript
// Code quality and security insights
mcp__ide__getDiagnostics({ uri: "src/[target-files]" })

// Performance baseline establishment
mcp__puppeteer__puppeteer_evaluate({
  script: `
    const metrics = performance.getEntriesByType('navigation')[0];
    return {
      pageLoadTime: metrics.loadEventEnd - metrics.fetchStart,
      domReady: metrics.domContentLoadedEventEnd,
      firstPaint: performance.getEntriesByType('paint')[0]?.startTime
    };
  `
})
```

**Mitigation Command Templates:**
```bash
# Rollback preparation
git tag pre-[feature-name]-$(date +%Y%m%d)
git push origin pre-[feature-name]-$(date +%Y%m%d)

# Feature flag setup
echo "FEATURE_[NAME]_ENABLED=false" >> .env.local

# Monitoring setup
curl -X POST [monitoring-endpoint] -d '{"metric":"[feature-name]-deploy","value":1}'
```

**Checklist:**
```
□ Run parallel security and performance scans
□ Use IDE diagnostics to identify code quality risks
□ Establish performance baselines with puppeteer
□ Create rollback tags: `git tag pre-[feature]-$(date +%Y%m%d)`
□ Set up feature flags for safe deployment
□ Plan monitoring and alerting commands
```

## 🗂️ CLI-Optimized Planning Templates

**Note**: Use Task tool for complex analysis steps to maximize parallel execution efficiency.

### CLI-Optimized Feature Planning Template
```markdown
## Feature: [Feature Name]

### Overview & Measurement
- **Purpose**: [Why this feature exists]
- **Users**: [Who will use this]  
- **Success Metrics**: [Measurable outcomes with CLI validation commands]
  ```bash
  # Example success validation
  curl -s http://localhost:3000/api/metrics | jq '.feature_usage'
  npm run test:e2e -- --grep="[Feature Name]"
  ```

### Technical Approach & CLI Setup
- **Architecture**: [Pattern chosen]
  ```bash
  # Initial scaffolding commands
  mkdir -p src/features/[feature-name]/{components,hooks,utils,tests}
  git checkout -b feature/[feature-name]
  ```
- **MCP Scaffolding**:
  ```javascript
  mcp__context7__resolve-library-id({ libraryName: "[framework]" })
  mcp__context7__get-library-docs({ context7CompatibleLibraryID: "/[id]", topic: "component patterns" })
  ```

### Data & API Strategy
- **Schema Prototyping**:
  ```javascript
  mcp__sqlite__create_table({ query: "CREATE TABLE [entity] (...)" })
  mcp__sqlite__write_query({ query: "INSERT INTO [entity] VALUES (...)" })
  ```
- **API Mocking**:
  ```javascript
  mcp__fetch__fetch({ url: "[mock-endpoint]", raw: false })
  ```
- **CLI Validation**:
  ```bash
  curl -X POST http://localhost:3000/api/[endpoint] -H "Content-Type: application/json" -d '{"test":"data"}'
  ```

### Parallel Implementation Tasks
**Execute in parallel using Task tool where possible**

**Phase 1 - Foundation (Parallel Execution):**
```bash
# Start parallel development environment
npm run dev &
npm run test:watch &
npm run lint:watch &
```

**Task Group A (Independent - can run in parallel):**
1. [ ] Backend API endpoints
   ```bash
   Task: "Implement API routes for [feature]"
   # Commands: touch src/api/[feature].ts && npm run test:api
   ```
2. [ ] Database schema & migrations
   ```bash
   Task: "Create and test database schema"
   # Commands: npm run db:create-migration && npm run db:migrate:up
   ```

**Task Group B (Frontend - can run parallel to Group A):**
3. [ ] UI Components
   ```bash
   Task: "Build React components for [feature]"
   # Commands: npx create-component [Name] && npm run test:component
   ```
4. [ ] State management/hooks
   ```bash
   Task: "Implement custom hooks and state logic"
   # Commands: touch src/hooks/use[Feature].ts && npm run test:hooks
   ```

**Phase 2 - Integration (Sequential after Phase 1):**
5. [ ] Frontend-Backend integration
   ```bash
   npm run test:integration
   git add . && git commit -m "checkpoint: integration complete"
   ```
6. [ ] E2E testing with Puppeteer
   ```javascript
   mcp__puppeteer__puppeteer_navigate({ url: "http://localhost:3000/[feature]" })
   mcp__puppeteer__puppeteer_click({ selector: "[data-testid='[action]']" })
   mcp__puppeteer__puppeteer_screenshot({ name: "[feature]-complete" })
   ```

**Checkpoint Commands:**
```bash
# After each phase
git add .
git commit -m "checkpoint: [phase] - [what completed]"
npm run lint && npm run typecheck && npm run test
```

### Comprehensive Testing Strategy
**Parallel Test Execution Setup:**
```bash
# Run all test types in parallel
npm run test:unit &
npm run test:integration &
npm run test:e2e &
wait  # Wait for all tests to complete
```

**Unit Tests with CLI Validation:**
- **What to test**: [Core business logic, utilities, pure functions]
  ```bash
  npm run test:unit -- --coverage --watch=false
  npx jest --testPathPattern="[feature]" --coverage
  ```

**Integration Tests with MCP:**
- **API Contract Testing**:
  ```javascript
  mcp__fetch__fetch({ 
    url: "http://localhost:3000/api/[endpoint]",
    raw: false 
  })
  ```
- **Database Integration**:
  ```javascript
  mcp__sqlite__read_query({ query: "SELECT * FROM [table] WHERE [condition]" })
  ```

**E2E Tests - Complete Puppeteer Workflow:**
```javascript
// 1. Navigation & Load Testing
mcp__puppeteer__puppeteer_navigate({ url: "http://localhost:3000/[feature]" })
mcp__puppeteer__puppeteer_evaluate({ 
  script: "document.readyState === 'complete'" 
})

// 2. User Interaction Testing  
mcp__puppeteer__puppeteer_fill({ selector: "[data-testid='input']", value: "test-data" })
mcp__puppeteer__puppeteer_click({ selector: "[data-testid='submit']" })
mcp__puppeteer__puppeteer_hover({ selector: ".tooltip-trigger" })

// 3. State Validation
mcp__puppeteer__puppeteer_evaluate({ 
  script: "document.querySelector('.success-message')?.textContent" 
})

// 4. Performance Benchmarking
mcp__puppeteer__puppeteer_evaluate({
  script: `
    const metrics = performance.getEntriesByType('navigation')[0];
    return {
      pageLoadTime: metrics.loadEventEnd - metrics.fetchStart,
      domReady: metrics.domContentLoadedEventEnd,
      firstPaint: performance.getEntriesByType('paint')[0]?.startTime
    };
  `
})

// 5. Error Scenario Testing
mcp__puppeteer__puppeteer_fill({ selector: "[data-testid='input']", value: "invalid-data" })
mcp__puppeteer__puppeteer_click({ selector: "[data-testid='submit']" })
mcp__puppeteer__puppeteer_evaluate({ 
  script: "document.querySelector('.error-message')?.textContent" 
})

// 6. Visual Regression
mcp__puppeteer__puppeteer_screenshot({ name: "[feature]-baseline", selector: "body" })
mcp__puppeteer__puppeteer_screenshot({ name: "[feature]-after-interaction", selector: ".main-content" })
```

**Edge Case Testing Commands:**
```bash
# Network failure simulation
curl -X POST http://localhost:3000/api/[endpoint] --max-time 1

# Large data set testing
for i in {1..100}; do curl -X POST http://localhost:3000/api/[endpoint] -d "{\"id\":$i}"; done

# Concurrent user simulation
ab -n 100 -c 10 http://localhost:3000/[feature]
```

**Test Cleanup Commands:**
```bash
# Remove temporary test files
find . -name "*.test.tmp" -delete
rm -rf test-output/ screenshots-temp/
git clean -fd  # Remove untracked files
```

### Risks & CLI Mitigation Commands
- **Performance Risk**: [Description]
  ```bash
  # Mitigation commands
  npx lighthouse-ci autorun --collect.numberOfRuns=3
  npx bundle-analyzer --no-open
  npm run test:performance
  ```
- **Security Risk**: [Description]
  ```bash
  # Mitigation commands  
  npm audit --audit-level=moderate
  npx audit-ci --moderate
  npm run test:security
  ```
- **Integration Risk**: [Description]
  ```bash
  # Mitigation commands
  npm run test:integration
  curl -f http://localhost:3000/health
  npm run test:contract
  ```
```

### Bug Fix Planning Template
```markdown
## Bug: [Bug Description]

### Investigation
- **Symptoms**: [What users experience]
- **Root Cause**: [Technical cause]
- **Affected Areas**: [Code/features impacted]

### Fix Approach
- **Solution**: [How to fix]
- **Changes Required**:
  1. [File/Component]: [Change needed]
  2. [File/Component]: [Change needed]

### Testing
- **Reproduction Steps**: [How to verify bug]
- **Validation**: [How to confirm fix]
- **Regression Tests**: [What else to check]
```

### Refactoring Planning Template
```markdown
## Refactoring: [Area/Component]

### Motivation
- **Current Issues**: [Problems with existing code]
- **Benefits**: [Why refactor now]

### Scope
- **Included**: [What will be refactored]
- **Excluded**: [What stays the same]
- **Dependencies**: [What else might be affected]

### Approach
- **Pattern**: [New pattern/structure]
- **Migration Strategy**: [How to transition]
- **Backwards Compatibility**: [What to maintain]

### Steps
1. [ ] [Preparation step]
2. [ ] [Refactoring step]
3. [ ] [Testing step]
4. [ ] [Migration step]
```

## 🔍 CLI Agent Planning Checklist

**Parallel Execution Priority**: Always look for opportunities to use Task tool for concurrent operations.

### CLI Agent Efficiency Checklist:
- [ ] Can this task be executed in parallel using Task tool?
- [ ] Is each task small enough to complete with clear CLI commands?
- [ ] Have I planned checkpoint commits: `git commit -m "checkpoint: [description]"`?
- [ ] Can the agent work on this without loading excessive files?
- [ ] Are tasks independent enough to delegate to sub-agents?
- [ ] Have I identified opportunities for batched CLI operations?
- [ ] Can complex searches be delegated using Task tool?

### Before Starting Any Task:
- [ ] Do I understand what success looks like?
- [ ] Have I identified all dependencies?
- [ ] Do I know the existing code patterns to follow?
- [ ] Have I considered error cases?
- [ ] Is my approach testable?
- [ ] Have I estimated the effort required?

### For New Features:
- [ ] User flow documented?
- [ ] Data model designed?
- [ ] API contracts defined?
- [ ] UI mockups reviewed?
- [ ] Performance requirements clear?
- [ ] Security requirements addressed?

### For Bug Fixes:
- [ ] Root cause identified?
- [ ] Reproduction steps documented?
- [ ] Fix approach validated?
- [ ] Regression impact assessed?
- [ ] Test cases defined?

### For Refactoring:
- [ ] Clear improvement goals?
- [ ] Backwards compatibility plan?
- [ ] Migration strategy defined?
- [ ] Testing approach comprehensive?
- [ ] Rollback plan available?

## 📊 Estimation Guidelines

### Task Size Reference (CLI-Optimized):
- **Trivial** (< 30 min): Config changes, CLI script creation - Execute immediately
  ```bash
  # Example: Update package.json scripts
  npm pkg set scripts.custom="[command]"
  ```
- **Small** (30 min - 1 hr): Single component/API endpoint - Ideal for direct execution
  ```bash
  # Example: Create new component
  npx create-component [Name] --typescript
  npm run test:component -- [Name]
  ```
- **Medium** (1-2 hr): Multi-component features - Use parallel Task execution
  ```bash
  # Example: Feature with frontend + backend
  Task: "Implement backend API"
  Task: "Create frontend components" 
  Task: "Write integration tests"
  ```
- **Large** (2-4 hr): Complex features - Must decompose with sequential thinking
  ```javascript
  mcp__sequential-thinking__sequentialthinking({
    thought: "Break down [complex feature] into independent, parallel tasks"
  })
  ```
- **XL** (> 4 hr): Architectural changes - Requires multiple planning sessions

### CLI Agent Session Guidelines:
- **Optimal operations**: Direct CLI commands with clear outcomes
- **Parallel execution**: Use Task tool for independent operations
- **File handling**: Use Task tool for complex file searches/analysis
- **Command batching**: Group related CLI operations together
  ```bash
  # Example: Batch related commands
  npm run lint && npm run typecheck && npm run test && git add . && git commit -m "feature: [description]"
  ```

### Estimation Factors:
- **Complexity**: Algorithm difficulty, business logic
- **Integration**: Number of systems involved
- **Testing**: Test coverage required
- **Documentation**: Docs to write/update
- **Review**: Code review cycles expected

### Buffer Recommendations:
- Add 20% for unfamiliar code areas
- Add 30% for external dependencies
- Add 40% for concurrent team changes
- Add 50% for critical path items

## 🚀 CLI-Optimized Quick Planning for Common Scenarios

### Adding a New API Endpoint
**Parallel Execution Plan:**
```bash
# Task A: Schema definition (independent)
Task: "Define OpenAPI schema for [endpoint]"
# Commands: touch api/schemas/[endpoint].yml && npx swagger-codegen validate api/schemas/[endpoint].yml

# Task B: Authentication check (independent)
Task: "Verify auth middleware integration"
# Commands: grep -r "auth" src/middleware/ && npm run test:auth

# Task C: Validation setup (independent)
Task: "Create request validation schemas"
# Commands: npm install joi && touch src/validation/[endpoint].js
```

**Sequential Implementation:**
```bash
# 1. Mock and test early
curl -X POST http://localhost:3000/api/[endpoint] -d '{"test":"data"}'

# 2. MCP contract testing
mcp__fetch__fetch({ url: "http://localhost:3000/api/[endpoint]", raw: false })

# 3. Integration validation
npm run test:api -- --grep="[endpoint]"

# 4. E2E coverage with puppeteer
mcp__puppeteer__puppeteer_navigate({ url: "http://localhost:3000/test-[endpoint]" })
mcp__puppeteer__puppeteer_fill({ selector: "[data-testid='api-input']", value: "test-data" })
mcp__puppeteer__puppeteer_click({ selector: "[data-testid='api-submit']" })
```

### Creating a New UI Component
**Parallel Discovery & Setup:**
```bash
# Task A: Pattern analysis
Task: "Find similar components in codebase"
# Commands: find src/ -name "*.tsx" | xargs grep -l "interface.*Props" | head -10

# Task B: Design system check
Task: "Identify reusable design tokens"
# Commands: grep -r "theme\|styled\|css-in-js" src/ --include="*.ts" --include="*.tsx"

# Task C: Accessibility research
Task: "Review ARIA patterns for component type"
# Commands: curl -s https://www.w3.org/WAI/ARIA/apg/ | grep -i "[component-type]"
```

**MCP-Enhanced Development:**
```javascript
// Get latest component patterns
mcp__context7__resolve-library-id({ libraryName: "[current-ui-library]" })
mcp__context7__get-library-docs({ 
  context7CompatibleLibraryID: "/[id]", 
  topic: "[component-type] patterns",
  tokens: 3000
})

// Get modern UI components if needed
mcp__magicui__getUIComponents()
mcp__magicui__getButtons()  // For button components
mcp__magicui__getAnimations()  // For animated components
```

**CLI Development Flow:**
```bash
# 1. Scaffold component
npx create-component [ComponentName] --typescript --stories --test

# 2. Parallel development
npm run storybook &  # Component development environment
npm run test:watch -- [ComponentName] &  # Test feedback
npm run dev &  # App integration testing

# 3. Responsive testing
npx puppeteer-cli screenshot http://localhost:6006/?path=/story/[component] --viewport 375x667  # Mobile
npx puppeteer-cli screenshot http://localhost:6006/?path=/story/[component] --viewport 1920x1080  # Desktop

# 4. Accessibility validation
npx axe-cli http://localhost:6006/?path=/story/[component]
```

### Database Schema Changes
**Risk-Free Prototyping with MCP:**
```javascript
// 1. Rapid schema prototyping
mcp__sqlite__create_table({ 
  query: "CREATE TABLE [new_table] (id INTEGER PRIMARY KEY, [fields]...)" 
})

// 2. Test data insertion
mcp__sqlite__write_query({ 
  query: "INSERT INTO [new_table] VALUES ([test_data])" 
})

// 3. Query testing
mcp__sqlite__read_query({ 
  query: "SELECT * FROM [new_table] JOIN [existing_table] ON [condition]" 
})
```

**Parallel Analysis & Planning:**
```bash
# Task A: Impact analysis
Task: "Analyze current schema dependencies"
# Commands: grep -r "[table_name]" src/ --include="*.sql" --include="*.js" --include="*.ts"

# Task B: Performance baseline
Task: "Establish current query performance"
# Commands: npm run db:benchmark && curl http://localhost:3000/api/performance-metrics

# Task C: Migration strategy
Task: "Plan zero-downtime migration approach"
# Commands: npm run db:migration:plan -- --dry-run
```

**Safe Execution Pipeline:**
```bash
# 1. Create migration branch
git checkout -b migration/[schema-change]

# 2. Generate migration files
npm run db:create-migration -- --name="[descriptive-name]"

# 3. Test migration (dry run)
npm run db:migrate:up -- --dry-run
npm run db:migrate:down -- --dry-run

# 4. Performance testing
npm run test:db-performance -- --migration=[migration-id]

# 5. Backup strategy
pg_dump [database] > backup-pre-migration-$(date +%Y%m%d).sql
```

### Third-party Integration
**Parallel API Research & Testing:**
```bash
# Task A: Documentation analysis
Task: "Extract API endpoints and rate limits from documentation"
# Commands: curl -s [api-docs-url] | grep -E "(GET|POST|PUT|DELETE|rate.*limit)"

# Task B: Authentication testing
Task: "Test auth flow and token management"
# Commands: curl -X POST [auth-endpoint] -d '{"credentials":"test"}'

# Task C: Response format analysis
Task: "Analyze response schemas and error formats"
# Commands: curl -s [api-endpoint] | jq '.' > sample-response.json
```

**MCP Development & Testing:**
```javascript
// 1. Live API testing
mcp__fetch__fetch({ 
  url: "[third-party-endpoint]",
  raw: false
})

// 2. Error handling testing
mcp__fetch__fetch({ 
  url: "[endpoint-with-invalid-params]",
  raw: true  // Get full error response
})

// 3. Rate limit testing
// Execute multiple fetch calls to test limits
for (let i = 0; i < 10; i++) {
  mcp__fetch__fetch({ url: "[rate-limited-endpoint]" })
}
```

**Integration Development Pipeline:**
```bash
# 1. Mock server setup
npm install json-server
echo '{"[resource]":[{"id":1,"data":"mock"}]}' > db.json
json-server --watch db.json --port 3001 &

# 2. Integration testing
curl -X GET http://localhost:3001/[resource]
curl -X POST http://localhost:3001/[resource] -d '{"test":"data"}'

# 3. Error scenario testing
curl -X GET http://localhost:3001/nonexistent  # 404 testing
curl --max-time 1 http://localhost:3001/[slow-endpoint]  # Timeout testing

# 4. Monitoring setup
echo "THIRD_PARTY_API_MONITOR=true" >> .env
npm run monitor:api-health
```

## 💡 Best Practices

### Communication
- Document assumptions clearly
- Ask questions early
- Share planning with team
- Update plans as you learn

### Flexibility
- Plans are guides, not contracts
- Adjust as you discover issues
- Track deviations for learning
- Communicate changes promptly

### Learning
- Review plan vs actual after completion
- Document lessons learned
- Update templates based on experience
- Share insights with team

## 🎓 Planning Anti-patterns to Avoid

### Over-planning
- Spending more time planning than doing
- Creating unnecessarily detailed plans
- Planning for unlikely scenarios
- Paralysis by analysis

### Under-planning
- Jumping straight to code
- Ignoring dependencies
- Missing edge cases
- No consideration for testing

### Common Mistakes
- Not validating assumptions
- Ignoring existing patterns
- Planning in isolation
- Not considering maintenance
- Forgetting about deployment

## 📚 Additional Resources

### When to Use Each Planning Level:
- **Light Planning**: Familiar tasks, small changes
- **Standard Planning**: New features, moderate complexity
- **Deep Planning**: Architecture changes, critical features
- **Formal Planning**: New systems, major refactors

### Tools for Planning:
- Diagrams: Architecture, flow charts, sequence diagrams
- Prototypes: Proof of concepts, UI mockups
- Spikes: Technical investigations
- Documentation: ADRs, design docs
- Collaboration: Pair planning, team reviews
- Component Libraries: Use context7 to get latest component documentation and examples

### MCP Server Usage Guidelines for CLI Agents:

#### Planning Phase MCP Server Matrix:
| Phase | Primary MCPs | CLI Integration | Parallel Execution |
|-------|--------------|-----------------|--------------------|
| **Ideation/Decomposition** | `sequential-thinking` | Break down into CLI-executable tasks | Use for complex requirement analysis |
| **Discovery & Analysis** | `context7` + Task tool | Scaffold structure, analyze patterns | Delegate searches to Task tool |
| **Rapid Prototyping** | `context7` + `sqlite` + `fetch` | Generate stubs, test schemas, mock APIs | Run schema + API + UI tasks in parallel |
| **Testing & Validation** | `puppeteer` + `fetch` + `ide` | Complete E2E workflows, API contracts | Execute test suites concurrently |
| **Code Quality** | `ide` + `sequential-thinking` | Diagnostics, optimization planning | Parallel linting, type checking, testing |

#### Task Tool Delegation Patterns:
```bash
# When to use Task tool (delegate complex searches):
Task: "Find all components using [pattern] in codebase"
Task: "Analyze API endpoint patterns and authentication"
Task: "Search for similar implementations of [feature]"
Task: "Identify all files that need updates for [change]"

# When to execute directly (simple, clear commands):
npm run lint
git status
curl -X GET http://localhost:3000/api/health
```

#### Daily CLI Integration Patterns:

**context7**: Architecture & Code Generation
```bash
# Use for: Big refactors, new pages/routes, component patterns
mcp__context7__resolve-library-id({ libraryName: "[framework]" })
# Follow with: mkdir -p [generated-structure] && npm run scaffold
```

**sequential-thinking**: Complex Problem Decomposition
```bash
# Use for: Multi-step features, "how do I...?" queries
mcp__sequential-thinking__sequentialthinking({ thought: "[complex-problem]" })
# Follow with: Task tool delegation for parallel execution
```

**puppeteer**: Complete Frontend Validation
```javascript
// CLI-integrated E2E workflow:
// 1. Start dev environment
// npm run dev & npm run test:server &

// 2. Execute comprehensive testing
mcp__puppeteer__puppeteer_navigate({ url: "http://localhost:3000" })
mcp__puppeteer__puppeteer_click({ selector: "[data-testid='feature']" })
mcp__puppeteer__puppeteer_evaluate({ script: "window.performance.now()" })
mcp__puppeteer__puppeteer_screenshot({ name: "validation-complete" })

// 3. Validate with CLI
// npm run test:e2e:validate && echo "✅ E2E validation complete"
```

**sqlite**: Rapid Data Prototyping
```bash
# Use for: Schema design, test data, rapid iteration
# Before: mcp__sqlite__create_table({ query: "..." })
# After: npm run db:export-schema > prototype-schema.sql
```

**fetch**: API Development & Testing
```bash
# Use for: API exploration, contract testing, mocking
# Before: mcp__fetch__fetch({ url: "[endpoint]" })
# After: curl -X GET [endpoint] | jq '.' > api-response.json
```

**Task Tool**: Complex Analysis & Search Operations
```bash
# Use for operations that require multiple file reads/searches:
Task: "Find all usage patterns of [component] across codebase"
Task: "Analyze performance bottlenecks in [area]"
Task: "Identify security vulnerabilities in dependencies"
# Benefit: Parallel execution, reduced context usage
```

### Testing Considerations

**IMPORTANT**: Always clean up test artifacts and use CLI commands for verification.

```bash
# Cleanup commands to run after testing
rm -rf test-output/ *.tmp screenshots-temp/
git clean -fd  # Remove untracked files
npm run test:cleanup  # Run project-specific cleanup
echo "✅ Test cleanup complete"
```

#### Comprehensive Testing with Puppeteer MCP

**CRITICAL**: Puppeteer MCP is not just for screenshots - it's your complete browser automation testing suite for validating ALL frontend functionality.

##### Core Puppeteer MCP Commands for Testing:

1. **puppeteer_navigate** - Navigate to URLs and test page loads
   - Test different environments (staging, production)
   - Verify redirects and URL changes
   - Test page load performance

2. **puppeteer_click** - Simulate user clicks for interaction testing
   - Button functionality validation
   - Navigation flow testing
   - Modal/popup triggers
   - Interactive element verification

3. **puppeteer_fill** - Test form inputs and data entry
   - Form validation testing
   - Input field behavior
   - Search functionality
   - Login/authentication flows

4. **puppeteer_select** - Test dropdown menus
   - Option filtering
   - Dynamic content loading
   - Form dependencies

5. **puppeteer_hover** - Test hover states
   - Tooltip verification
   - Hover menu functionality
   - CSS transitions
   - Interactive UI elements

6. **puppeteer_evaluate** - Execute JavaScript for deep validation
   ```javascript
   // Verify DOM content
   puppeteer_evaluate({ script: "document.querySelector('.user-name').textContent" })
   
   // Check console errors
   puppeteer_evaluate({ script: "window.errors || []" })
   
   // Measure performance
   puppeteer_evaluate({ 
     script: "performance.getEntriesByType('navigation')[0].loadEventEnd" 
   })
   ```

7. **puppeteer_screenshot** - Visual validation and regression testing
   - Before/after comparisons
   - Error state documentation
   - Responsive design verification
   - UI component validation

##### Complete E2E Testing Workflow Example:

```javascript
// 1. Test Page Load
puppeteer_navigate({ url: "https://app.example.com" })

// 2. Verify Initial State
puppeteer_evaluate({ 
  script: "document.readyState === 'complete'" 
})

// 3. Test User Login Flow
puppeteer_fill({ selector: "#email", value: "test@example.com" })
puppeteer_fill({ selector: "#password", value: "password123" })
puppeteer_click({ selector: "#login-button" })

// 4. Validate Login Success
puppeteer_evaluate({ 
  script: "window.location.pathname === '/dashboard'" 
})

// 5. Test Interactive Features
puppeteer_hover({ selector: ".user-menu" })
puppeteer_click({ selector: ".dropdown-item" })

// 6. Verify Data Loading
puppeteer_evaluate({ 
  script: "document.querySelectorAll('.data-row').length > 0" 
})

// 7. Test Error Handling
puppeteer_click({ selector: ".invalid-action" })
puppeteer_evaluate({ 
  script: "document.querySelector('.error-message')?.textContent" 
})

// 8. Performance Validation
puppeteer_evaluate({
  script: `
    const metrics = performance.getEntriesByType('navigation')[0];
    return {
      pageLoadTime: metrics.loadEventEnd - metrics.fetchStart,
      domReady: metrics.domContentLoadedEventEnd
    }
  `
})

// 9. Visual Documentation
puppeteer_screenshot({ name: "test-complete", selector: "body" })
```

##### Testing Best Practices:

1. **Always validate functionality, not just visuals**
   - Use `puppeteer_evaluate` to check DOM state
   - Verify JavaScript execution and console errors
   - Test API responses and data loading

2. **Test user journeys end-to-end**
   - Chain multiple actions to simulate real workflows
   - Test error paths and edge cases
   - Validate state persistence across pages

3. **Performance testing is crucial**
   - Measure page load times
   - Check for memory leaks
   - Monitor console errors and warnings

4. **Responsive testing**
   - Test different viewport sizes
   - Validate mobile interactions
   - Check touch events and gestures

5. **Accessibility validation**
   - Test keyboard navigation
   - Verify ARIA attributes
   - Check focus management

#### Isolated Environment Testing
When planning tests, especially for AI-generated code, consider using Docker for isolated testing environments if you need to:
- Test without affecting your local setup
- Verify code works in clean environments
- Test across different versions/platforms
- Automatic cleanup with `--rm` flag

Example: `docker run --rm -v $(pwd):/app node:18 npm test`

## 📋 Quick Reference: CLI Agent Command Patterns

### Essential CLI Workflows
```bash
# Parallel development setup
npm run dev & npm run test:watch & npm run lint:watch &

# Comprehensive validation pipeline
npm run lint && npm run typecheck && npm run test && npm run build

# Checkpoint commit pattern
git add . && git commit -m "checkpoint: [description] - [completed-items]"

# Parallel testing execution
npm run test:unit & npm run test:integration & npm run test:e2e & wait

# Project health check
npm audit --audit-level=moderate && npx depcheck && npm outdated
```

### Task Tool Delegation Triggers
```bash
# Use Task tool when you need:
Task: "Search for [pattern] across entire codebase"
Task: "Analyze dependencies and usage patterns"
Task: "Find similar implementations of [feature]"
Task: "Identify all files requiring updates for [change]"

# Execute directly for simple operations:
npm run [script]
git status
curl -X GET [endpoint]
cat package.json | jq '.scripts'
```

### MCP Quick Commands
```javascript
// Problem decomposition
mcp__sequential-thinking__sequentialthinking({ thought: "[complex-problem]", totalThoughts: 5 })

// Architecture scaffolding
mcp__context7__resolve-library-id({ libraryName: "[framework]" })

// Rapid prototyping
mcp__sqlite__create_table({ query: "CREATE TABLE [entity] (...)" })
mcp__fetch__fetch({ url: "[api-endpoint]", raw: false })

// Complete E2E validation
mcp__puppeteer__puppeteer_navigate({ url: "http://localhost:3000" })
mcp__puppeteer__puppeteer_click({ selector: "[data-testid='action']" })
mcp__puppeteer__puppeteer_screenshot({ name: "validation-[step]" })

// Code quality insights
mcp__ide__getDiagnostics()
```

### Parallel Execution Patterns
```bash
# Phase 1: Independent tasks (run in parallel)
Task: "Backend API development"
Task: "Frontend component creation"
Task: "Database schema design"
Task: "Test suite planning"

# Phase 2: Integration (sequential after Phase 1)
npm run test:integration
git add . && git commit -m "checkpoint: integration complete"

# Phase 3: Validation (parallel testing)
npm run test:unit & npm run test:e2e & npm run test:performance & wait
```

Remember: Efficient CLI agents maximize parallel execution, delegate complex searches to Task tool, and maintain clear checkpoint commits. Plan for measurable outcomes with specific CLI validation commands.