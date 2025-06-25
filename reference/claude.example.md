# CLI Agent Planning Overview
Core planning methodology for CLI programming agents - see @docs/planning-guidelines.md for complete framework

# Quick Planning Checklist
- Analyze user intent and break into subtasks
- Validate file operations before execution  
- Test commands in safe environment first
- Follow error handling patterns in guidelines

# Essential Planning Principles
1. **Chain safely**: cmd1 && cmd2 — run the next command only if the previous succeeds
2. **Run in parallel**: cmd1 & cmd2 & wait — launch independent tasks concurrently, then sync
3. **No fake data**: If blocked, stop and ask; never fall back to mock or invented outputs
4. **Stay lean**: Share only essential files/info; keep replies brief to conserve context
5. **Read first**: Ingest all referenced docs/specs fully before writing code
6. **Follow the plan**: Build a to-do list, execute step-by-step, update only with user approval
7. **Use Task tool**: Delegate complex searches and analysis to maximize parallel efficiency

# Command Patterns
```bash
# Safe chaining - next command runs only if previous succeeds
npm run lint && npm run test && npm run build && echo "✅ All checks passed"

# Parallel execution - run independent tasks concurrently
npm run dev & npm run test:watch & npm run lint:watch & wait

# Validation before action
[ -f package.json ] && npm install || echo "❌ No package.json found"

# Checkpoint commits after successful operations
npm test && git add . && git commit -m "checkpoint: tests passing"

# Batch operations with error handling
for file in src/*.js; do
  npx eslint "$file" && echo "✅ $file" || echo "❌ $file failed linting"
done
```

# Full Documentation
- Complete planning framework: @docs/planning-guidelines.md