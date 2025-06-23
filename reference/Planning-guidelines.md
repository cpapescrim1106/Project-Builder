# Programming Planning Guidelines

This document outlines a systematic approach to planning programming tasks before implementation, ensuring thorough preparation and efficient execution.

## 🎯 Planning Principles

### 1. Optimize for AI Context Windows (HIGHEST PRIORITY)
- **Chunk all work into small, independent steps** that fit within AI memory limits
- Each step should be completable in a single AI session
- Plan frequent checkpoints to save progress
- Avoid loading multiple large files simultaneously
- Structure code changes to minimize context needed

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

### Step 1: Requirements Analysis
```
□ Read the full requirement/ticket/PRD
□ Use sequentialthinking to decompose complex requirements into ordered tasks
□ Identify core functionality needed
□ List all user interactions
□ Note technical constraints
□ Identify dependencies on other systems
□ List questions that need clarification
```
✓ Check off each item as completed

### Step 2: Technical Discovery
```
□ Use context7 for deep TypeScript-aware analysis of existing codebase structure
□ Identify similar patterns already implemented
□ Find reusable components/utilities
□ Check for existing libraries/packages
□ Use context7 to find latest component libraries and documentation
□ Use deepdev to understand cross-layer dependencies (DB ⇆ API ⇆ UI)
□ Review relevant documentation
□ Identify potential technical challenges
```
✓ Check off each item as completed

### Step 3: Solution Design
```
□ Choose appropriate architecture pattern
□ Use deepdev to design data models/structures with full-stack context
□ Use sqlite for rapid schema prototyping and iteration
□ Plan API endpoints (if applicable) - use fetch to mock/test early
□ Sketch UI components (if applicable)
□ Define interfaces between components
□ Consider error handling strategies
□ Plan for edge cases
```

### Step 4: Task Breakdown
```
□ Use sequentialthinking to create ordered list of implementation tasks
□ CRITICAL: Ensure each task is small enough for one AI session
□ Plan checkpoint commits after each 2-3 tasks
□ Estimate time for each task
□ Identify dependencies between tasks
□ Define testable completion criteria with comprehensive puppeteer validation:
  - User interaction tests (click, fill, select, hover)
  - DOM state verification (puppeteer_evaluate)
  - Performance benchmarks (load times, render metrics)
  - Error scenario coverage
□ Note which tasks can be parallelized
□ Mark natural break points for context resets
```

### Step 5: Risk Assessment
```
□ Identify technical risks
□ Consider performance implications
□ Review security concerns
□ Plan mitigation strategies
□ Define fallback approaches
```

## 🗂️ Planning Templates

### Feature Planning Template
```markdown
## Feature: [Feature Name]

### Overview
- **Purpose**: [Why this feature exists]
- **Users**: [Who will use this]
- **Success Criteria**: [How we measure success]

### Technical Approach
- **Architecture**: [Pattern/approach chosen] - use context7 to scaffold initial structure
- **Key Components**:
  1. [Component 1]: [Purpose]
  2. [Component 2]: [Purpose]
  3. [Component 3]: [Purpose]

### Data Requirements
- **Models**: [List data models needed] - use deepdev for schema design
- **Storage**: [Database/storage approach] - prototype with sqlite first
- **APIs**: [External APIs needed] - mock with fetch for early testing

### Implementation Tasks (AI-Optimized)
**Track Progress**: Check off tasks as completed ([ ] → [x])

Session 1:
1. [ ] [Task 1 - Small, focused change]
2. [ ] [Task 2 - Related small change]
   - CHECKPOINT: Commit progress

Session 2:
3. [ ] [Task 3 - Next independent piece]
4. [ ] [Task 4 - Related change]
   - CHECKPOINT: Test and commit

Session 3:
5. [ ] [Task 5 - New component/file]
   - CHECKPOINT: Full test cycle
   - [ ] Clean up any temporary test files

### Testing Strategy
- **Unit Tests**: [What to test]
- **Integration Tests**: [Key flows] - use fetch to test API contracts
- **E2E Tests**: [User journeys] - CRITICAL: Use puppeteer for complete validation:
  - Navigation flows (puppeteer_navigate + puppeteer_click)
  - Form submissions (puppeteer_fill + puppeteer_select)
  - Interactive elements (puppeteer_hover + puppeteer_evaluate)
  - Performance metrics (puppeteer_evaluate with performance API)
  - Error handling (trigger errors + puppeteer_evaluate)
  - Visual regression (puppeteer_screenshot before/after)
- **Edge Cases**: [Special scenarios]
- **Cleanup**: Delete any temporary test files after verification

### Risks & Mitigations
- **Risk 1**: [Description] → [Mitigation]
- **Risk 2**: [Description] → [Mitigation]
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

## 🔍 Planning Checklist

### AI Context Window Checklist:
- [ ] Is each task small enough to complete in one AI session?
- [ ] Have I planned checkpoints every 2-3 tasks?
- [ ] Can the AI work on this without loading too many files?
- [ ] Are tasks independent enough to work on separately?
- [ ] Have I avoided tasks that require understanding the entire codebase?

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

### Task Size Reference (AI-Optimized):
- **Trivial** (< 30 min): Config changes, simple UI tweaks - Perfect for AI
- **Small** (30 min - 1 hr): Single component changes - Ideal AI session size
- **Medium** (1-2 hr): Multi-component features - Split into 2-3 AI sessions
- **Large** (2-4 hr): Complex features - Must be broken down into smaller tasks
- **XL** (> 4 hr): Too large for AI - Break down before starting

### AI Session Guidelines:
- **Optimal session**: 30-45 minutes of focused work
- **Max session**: 1 hour before context reset needed
- **Files per session**: 3-5 files maximum
- **Changes per session**: 1-2 features or bug fixes

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

## 🚀 Quick Planning for Common Scenarios

### Adding a New API Endpoint
1. Use deepdev to define request/response schema with full-stack awareness
2. Check authentication requirements
3. Plan validation rules
4. Consider rate limiting
5. Design error responses
6. Use fetch to create mock endpoints and test contracts early
7. Plan integration tests with puppeteer for E2E coverage

### Creating a New UI Component
1. Review design mockups
2. Check context7 for existing component patterns and best practices
3. Identify reusable elements
4. Plan component props/API
5. Consider responsive behavior
6. Plan accessibility features
7. Define test scenarios

### Database Schema Changes
1. Use deepdev to analyze current schema and cross-layer impacts
2. Use sqlite to prototype and test new schema locally first
3. Plan migration strategy
4. Consider backwards compatibility
5. Plan data migration
6. Define rollback procedure
7. Plan performance testing

### Third-party Integration
1. Review API documentation
2. Use fetch to test API endpoints and understand response formats
3. Plan authentication approach
4. Design error handling
5. Plan retry strategies
6. Consider rate limits
7. Design fallback behavior
8. Create mock endpoints with fetch for development/testing

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

### MCP Server Usage Guidelines:

#### Planning Phase MCP Server Matrix:
| Phase | Primary MCPs | Purpose |
|-------|--------------|---------|
| **Ideation/Decomposition** | `sequentialthinking` | Break down fuzzy requirements into ordered checklist |
| **Architecture & Data Design** | `deepdev` → `context7` | Design data models with full-stack awareness, scaffold structure |
| **Prototype & Scaffolding** | `context7` + `sqlite` + `fetch` | Generate code stubs, test schemas, mock APIs |
| **Testing & CI Strategy** | `puppeteer` + `fetch` | Plan E2E tests, define API contracts |

#### Daily Use Cases:
- **context7**: Big refactors, generating new pages/routes, code reviews
- **deepdev**: ORM migrations, tRPC/GraphQL handlers, data-flow debugging
- **sequentialthinking**: Complex "how do I...?" queries, feature decomposition
- **puppeteer**: Complete frontend validation suite:
  - User interaction testing (click, fill, select, hover)
  - DOM state verification (evaluate JavaScript)
  - Performance monitoring (load times, metrics)
  - Error scenario validation
  - Console log monitoring
  - Visual regression testing
  - Accessibility validation
- **sqlite**: Seed scripts, quick data fixtures, schema prototyping
- **fetch**: REST endpoint testing, mocking 3rd-party APIs, contract definition

### Testing Considerations

**IMPORTANT**: Clean up any test files created during testing once they're no longer needed.

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

Remember: Good planning is an investment that pays dividends in faster implementation, fewer bugs, and better code quality. The goal is to think through problems before they become expensive to fix.