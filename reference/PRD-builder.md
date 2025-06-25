# AI-Guided PRD Builder Template

## 🎯 Purpose of This Document

This template guides AI agents to help users create comprehensive PRDs through assumptive, feature-based questions. The PRDs are specifically designed for AI-driven development, where AI agents will build 100% of the code. Instead of asking technical questions, the AI presents assumptions that users can easily confirm or correct.

## 📖 How This Template Works

1. **User provides idea** - User describes their app concept in simple terms
2. **AI presents assumptions** - AI batches related feature assumptions into conversational questions
3. **User confirms/corrects** - Simple "yes" or "no, actually..." responses guide the process
4. **PRD generation** - AI creates a complete technical PRD optimized for AI agent implementation

---

## 💡 Your App Idea

**Describe your app in 1-3 sentences:**
*[User will provide their app idea here when using this template]*

---

## 🛠️ Tech Stack Context for AI Agents

**The AI agent building this app has access to and should utilize:**

### Deployment Platform
- **CapRover**: Self-hosted PaaS (Platform as a Service)
- **CapRover CLI**: Deploy with `caprover deploy`
- **Docker**: All apps must include a Dockerfile for CapRover
- **One-click deployment**: Apps should be CapRover-ready

### Available Tools & Services

#### MCP Servers for Planning & Building:
- **Context7**: Deep TypeScript-aware reasoning over large file sets
  - Use `mcp__context7__resolve-library-id` to find libraries
  - Use `mcp__context7__get-library-docs` to get up-to-date documentation
  - Best for: Big refactors, generating new pages/routes, scaffolding
- **SequentialThinking**: Forces step-by-step thinking and tool selection
  - Best for: Feature decomposition, task sizing, roadmap planning
- **Puppeteer**: Complete browser automation testing suite (NOT just screenshots!)
  - **Navigation & Load Testing**: `puppeteer_navigate` - test page loads, redirects, performance
  - **User Interactions**: `puppeteer_click`, `puppeteer_fill`, `puppeteer_select`, `puppeteer_hover`
  - **Validation & Verification**: `puppeteer_evaluate` - execute JavaScript to verify DOM state, check errors, measure performance
  - **Visual Testing**: `puppeteer_screenshot` - capture states for regression testing
  - Best for: Complete E2E validation, user journey testing, performance monitoring, error detection
- **SQLite**: Local file-based database wrapper
  - Best for: Schema prototyping, seed scripts, offline testing
- **Fetch**: Simple HTTP client + response inspector
  - Best for: API testing, mocking external services, contract validation

### Key Requirements
- All templates must be Docker-compatible
- Include CapRover deployment configuration
- Consider MCP integration points
- Support automated testing with Puppeteer

---

## 🤖 AI Agent Instructions

**IMPORTANT: This section tells AI agents how to use this document. When a user references this file and provides their app idea, the AI should follow these steps:**

### Step 1: Analyze the User's Idea

First, use `sequentialthinking` to decompose the user's idea and evaluate how detailed it is:

**If the idea is comprehensive** (includes target users, features, scale, and technical preferences):
- Skip the questions entirely
- Summarize their vision: "Based on your detailed description, I understand you want [summary]. Let me generate your PRD and recommend the best templates..."
- Proceed directly to PRD generation

**If the idea is partially detailed** (has some specifics but missing key decisions):
- Ask only the questions needed to fill gaps
- Example: "I can see you want a SaaS product. Just one question: How feature-rich should this be? A) Essential, B) Full-Featured, or C) Premium?"

**If the idea is vague** (just a concept or single sentence):
- Ask the 2-3 essential questions below

### Step 2: Ask Questions (Only If Needed)

#### Question 1: Core Direction (Ask only if not clear)
**Example format:**
"Based on your [app type] idea, which best describes your vision:

**A) Personal Tool** - Just for me or a small group, simple and fast
**B) SaaS Product** - For customers/users, with accounts and subscriptions  
**C) Community Platform** - Public sharing, social features, and growth

Quick answer: A, B, or C?"

#### Question 2: Feature Depth (Ask only if not specified)
**Example format:**
"How feature-rich should this be:

**A) Essentials Only** - Core features, clean and simple
**B) Full-Featured** - All the expected features for this type of app
**C) Premium** - Advanced features, AI, integrations, analytics

Quick answer: A, B, or C?"

#### Question 3: Technical Preferences (Rarely needed)
**Example format:**
"Any technical preferences:

**A) Keep it simple** - Minimal dependencies, easy to maintain
**B) Modern stack** - Latest tools, great developer experience
**C) Enterprise-grade** - Maximum scalability, security, compliance

Quick answer: A, B, or C?"

### Step 3: Generate PRD and Templates

Based on either:
- The detailed idea provided (no questions asked)
- The answers to your questions (for vague ideas)
- A combination of provided details + question answers

Use MCP servers to enhance PRD generation:
1. **sequentialthinking**: Create step-by-step implementation plan
2. **context7**: Find relevant component libraries and patterns
3. **sqlite**: Suggest schema prototypes for quick iteration
4. **fetch**: Define API integration points and mock endpoints
5. **puppeteer**: Design comprehensive validation strategy:
   - User interaction test flows (login, data entry, navigation)
   - DOM verification scripts (check loaded data, error states)
   - Performance benchmarks (page load, API response times)
   - Error scenario testing (invalid inputs, network failures)
   - Accessibility validation (keyboard nav, screen reader support)

Proceed to generate the complete PRD and template recommendations.

---

## 📋 Generated PRD + Template Recommendations

After answering 2-3 quick questions, generate:

### Executive Summary
- **Product Name**: [Name]
- **One-line Description**: [Description]
- **Target Users**: [Who will use this]
- **Core Value**: [Main benefit]

### Feature Specifications

#### Must-Have Features (MVP)
1. **[Feature Name]**
   - What: [Description]
   - Why: [User benefit]
   - Technical: [Brief tech requirement]

2. **[Feature Name]**
   - What: [Description]
   - Why: [User benefit]
   - Technical: [Brief tech requirement]

#### Nice-to-Have Features (Post-MVP)
- [Feature]: [Why it's valuable]
- [Feature]: [Why it's valuable]

### User Journey
1. **First Visit**: [What happens]
2. **Core Action**: [Main user flow]
3. **Return Visit**: [Ongoing experience]

### Technical Architecture for AI Development

#### AI-Optimized Stack
- **Framework**: Next.js 14+ with App Router (best AI agent support)
- **Database**: [PostgreSQL/SQLite based on needs] with Prisma ORM (AI-friendly schemas)
- **Auth**: [NextAuth/Clerk based on complexity] (pre-built AI patterns)
- **Styling**: Tailwind CSS + shadcn/ui (component-based for AI)
  - Use context7 to get latest shadcn/ui component examples
- **Deployment**: CapRover-ready with included Dockerfile
- **Code Structure**: Modular components, clear file naming, single-responsibility functions

#### CapRover Deployment Setup
```dockerfile
# Dockerfile for CapRover (included in all templates)
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN npm ci --only=production
RUN npm run build
EXPOSE 3000
CMD ["npm", "start"]
```

```json
// captain-definition (for CapRover)
{
  "schemaVersion": 2,
  "dockerfileLines": ["FROM node:18-alpine", "..."]
}
```

#### Data Model
```
[Simple representation of main entities]
User -> [Relationship] -> Content
Content -> [Relationship] -> [Other entities]
```

**MCP-Enhanced Data Design:**
- Use `sqlite` to prototype and test schemas locally
- Consider API-first design with `fetch` for external integrations

### 🎯 Template Categories (All CapRover-Ready)

#### Minimal Starters (5-7 AI sessions)
- `create-next-app` - Basic Next.js + Docker
- `create-vite` - Simple SPA + nginx Docker
- `create-remix` - Basic Remix + Docker

#### Full-Stack Starters (8-12 AI sessions)
- `create-t3-app` - TypeScript, Prisma, tRPC + Docker
- `create-blitz-app` - Full-stack with auth + Docker
- `create-redwood-app` - JAMstack framework + Docker

#### Premium/Enterprise Starters (12-20 AI sessions)
- SaaS boilerplates (BoxyHQ, Volca) + MCP integration
- Enterprise starters (Backstage, Medusa) + Puppeteer tests
- Custom architectural templates + full automation

### 📚 Additional Resources

**Template Repositories:**
- [Awesome Next.js](https://github.com/unicodeveloper/awesome-nextjs#boilerplates)
- [Awesome SaaS Boilerplates](https://github.com/smirnov-am/awesome-saas-boilerplates)
- [T3 Stack Examples](https://create.t3.gg/en/examples)

**Component Libraries via Context7:**
- Use context7 to access latest documentation for:
  - React component libraries (shadcn/ui, Material UI, Ant Design)
  - Vue components (Vuetify, Element Plus)
  - Framework-specific components and patterns

### Success Metrics
- **Technical**: Page load < 2s, 99.9% uptime
- **User**: [Specific user action] completion rate > 80%
- **Business**: [Relevant metric for the app type]

### Working with AI Agents

#### Prompt Engineering Strategy
- Start each session with: "Continue building [app name] using the PRD. Current focus: [specific feature]"
- Provide the PRD context at the beginning of each session
- Work on one feature at a time for best results
- Test immediately after each component is built

#### Tech Stack Integration with MCP Servers
- **SequentialThinking**: Use for breaking down complex features into ordered tasks
- **Context7**: AI uses context7 to:
  - Get latest component documentation and examples
  - Find best practices for specific libraries
  - Access up-to-date API references
  - Scaffold initial project structure
- **SQLite**: Prototype data models before committing to production DB
  - Test migrations locally
  - Create seed data for development
- **Fetch**: Mock and test all external API integrations
  - Define contract tests early
  - Create fallback responses for offline development
- **Puppeteer**: AI creates comprehensive frontend validation using ALL Puppeteer capabilities:
  - **User Journey Testing**: Chain navigate → fill → click → evaluate for complete flows
  - **Interaction Validation**: Test every clickable element, form field, dropdown
  - **State Verification**: Use `puppeteer_evaluate` to check DOM changes, data loading
  - **Performance Testing**: Measure load times, render performance, memory usage
  - **Error Handling**: Trigger and validate error states, console errors
  - **Visual Regression**: Screenshot critical states for comparison
  - **Example Test Flow**:
    ```javascript
    // Navigate and verify load
    puppeteer_navigate({ url: "app.com" })
    puppeteer_evaluate({ script: "document.readyState === 'complete'" })
    
    // Test user interaction
    puppeteer_fill({ selector: "#search", value: "test query" })
    puppeteer_click({ selector: "#search-btn" })
    
    // Verify results
    puppeteer_evaluate({ 
      script: "document.querySelectorAll('.result').length > 0" 
    })
    
    // Check performance
    puppeteer_evaluate({ 
      script: "performance.getEntriesByType('navigation')[0].loadEventEnd" 
    })
    ```
- **Docker**: AI always creates CapRover-compatible Dockerfiles

#### AI-Friendly Architecture
- **Component Isolation**: Each feature in its own file/folder
- **Clear Naming**: Descriptive file names like `RecipeCard.tsx`, `useAuth.ts`
- **Type Safety**: TypeScript throughout for better AI understanding
- **Consistent Patterns**: Reuse patterns AI establishes early

#### Session Management
- Session 1-2: Foundation and setup
- Session 3-5: One major feature per session
- Session 6+: Refinements and deployment
- Always commit working code between sessions

#### AI Agent Best Practices
- Let AI see the full PRD before starting
- Share error messages immediately for quick fixes
- Ask AI to explain architectural decisions
- Request AI to create README updates as it builds
- Encourage AI to use context7 for:
  - Finding the right component library for your needs
  - Getting accurate, up-to-date code examples
  - Checking best practices before implementation

---

## 🚀 Template Recommendations

### Based on Your Choices:

#### Primary Templates (Top 3)

**1. [Template Name Based on Answers]**
```bash
# Quick start command
npx create-[template]@latest my-app
# or
git clone https://github.com/[template-repo]
```
- **Why this fits**: Matches your [specific requirements]
- **Includes**: [List key features/packages]
- **CapRover ready**: Dockerfile included
- **Time to MVP**: [X AI sessions]

**2. [Second Best Template]**
```bash
# Quick start command
npx create-[template]@latest my-app
```
- **Why this fits**: Alternative if you want [different aspect]
- **Includes**: [List key features/packages]
- **Time to MVP**: [X AI sessions]

**3. [Third Option Template]**
```bash
# Quick start command
npx create-[template]@latest my-app
```
- **Why this fits**: Good for [specific use case]
- **Includes**: [List key features/packages]
- **Time to MVP**: [X AI sessions]

#### Quick Decision Matrix

| Your Choices | Best Template | Quick Start | Why | CapRover Ready |
|--------------|---------------|-------------|-----|----------------|
| A + A | Next.js Minimal | `npx create-next-app@latest` | Simple, fast, no complexity | ✅ Docker included |
| A + B | Next.js + SQLite | `npm create t3-app@latest` (skip tRPC) | Full features, local data | ✅ Docker + SQLite |
| B + A | Remix Indie Stack | `npx create-remix@latest --template indie` | SaaS-ready, simple features | ✅ Docker + Deploy |
| B + B | T3 Stack | `npm create t3-app@latest` | Modern SaaS starter | ✅ Docker + Prisma |
| B + C | SaaS Starter Kit | `git clone saas-starter-kit` | Premium features included | ✅ Docker + MCP ready |
| C + A | RedwoodJS | `yarn create redwood-app` | Community + simple | ✅ Docker + API |
| C + B | Blitz.js | `npm create blitz-app@latest` | Community features built-in | ✅ Docker + Auth |
| C + C | Enterprise Stack | Custom setup required | Maximum scale ready | ✅ Docker + Full automation |

### First AI Session Instructions

1. **Choose your template** from above
2. **Start your first AI session** with:
   ```
   "I want to build [app name]. Please:
   1. Run: [template command from above]
   2. Set up the project structure
   3. Add CapRover deployment files (Dockerfile, captain-definition)
   4. Implement the first feature: [from PRD]
   
   Here's my PRD: [paste PRD]"
   ```
3. **Test immediately** after AI completes each step
4. **Deploy to CapRover** when ready:
   ```bash
   npm install -g caprover
   caprover deploy
   ```

---

## 📝 Example Interactions

### Example 1: Vague Idea (Needs Questions)

**User**: "I want to build a recipe sharing app"

**AI**: "I'll help you build that! Just 2 quick questions to recommend the best templates:

**Question 1**: Which best describes your recipe app vision?
**A) Personal Tool** - Just for me/family, private recipes
**B) SaaS Product** - For paying customers with accounts
**C) Community Platform** - Public sharing, social features

Quick answer: A, B, or C?"

**User**: "C - community platform"

**AI**: "Great! **Question 2**: How feature-rich should this be?
**A) Essentials Only** - Share recipes, browse, save favorites
**B) Full-Featured** - Meal planning, shopping lists, collections
**C) Premium** - AI suggestions, nutrition tracking, video recipes

Quick answer: A, B, or C?"

**User**: "B - full featured"

### Example 2: Detailed Idea (No Questions Needed)

**User**: "I want to build a community recipe sharing platform where users can create accounts, share their recipes publicly, follow other cooks, create collections, plan weekly meals, and generate shopping lists. It should have social features like comments and ratings, work great on mobile for kitchen use, and eventually support premium features like nutrition tracking. Target audience is home cooks who want to discover and organize recipes."

**AI**: "Based on your detailed description, I understand you want a full-featured community recipe platform with social features, meal planning, and future premium capabilities. Perfect! Let me use our MCP servers to create a comprehensive PRD:

1. Using `sequentialthinking` to break down your requirements into ordered implementation tasks...
2. Using `context7` to find the best UI component libraries for recipe cards and social features...
3. Using `sqlite` to prototype the schema and create sample data...
4. Using `fetch` to plan the integration with nutrition APIs...
5. Using `puppeteer` to create comprehensive validation tests:
   - Recipe creation flow: navigate → fill form → submit → verify
   - Search functionality: enter query → click → check results
   - Social interactions: click like → add comment → verify updates
   - Performance metrics: measure page loads and API response times
   - Error handling: test invalid inputs and network failures...

Let me generate your PRD and recommend the best templates..."

**AI**: "Perfect! Based on your choices (Community Platform + Full Features), here are your best templates:

**🥇 Recommended: Blitz.js Full-Stack App**
```bash
npm create blitz-app@latest recipe-community
```
- Built-in auth, database, and API
- Community features ready to customize
- Real-time updates included
- 10-12 AI sessions to MVP

**🥈 Alternative: T3 Stack with Supabase**
```bash
npm create t3-app@latest recipe-community
```
- Modern TypeScript stack
- Great for social features
- Excellent developer experience
- 8-10 AI sessions to MVP

**🥉 Lighter Option: Next.js + Prisma Starter**
```bash
git clone https://github.com/prisma/prisma-examples/tree/latest/typescript/rest-nextjs-api-routes
```
- More control, less pre-built
- Good if you want custom architecture
- 12-15 AI sessions to MVP

Start your first AI session with: 'Create a recipe community platform using [chosen template]. First, set up the project with CapRover deployment files and implement user authentication.'"

---

## 🚢 CapRover Deployment Guide

**All generated apps will include:**

1. **Dockerfile** - Optimized for your chosen stack
2. **captain-definition** - CapRover configuration
3. **.dockerignore** - Proper exclusions
4. **Environment setup** - For CapRover env vars

**Deploy command (after development):**
```bash
# Install CapRover CLI
npm install -g caprover

# Login to your CapRover instance
caprover login

# Deploy (from project root)
caprover deploy
```

---

## 🤖 AI Implementation Notes

**Remember**: This entire project will be built by AI agents. The PRD should:
- Focus on features and user experience, not implementation details
- Use AI-friendly frameworks and patterns
- Break down work into discrete AI sessions
- Emphasize testing after each component
- Include clear file organization for AI navigation
- Always include CapRover deployment configuration
- Consider MCP server integration opportunities
- **CRITICAL: Use Puppeteer for comprehensive validation, not just screenshots:**
  
### 🧪 Puppeteer Testing Workflow for Every Feature

**Remember**: Every frontend feature MUST be validated with Puppeteer's full capabilities:

#### 1. **Page Load & Navigation Testing**
```javascript
puppeteer_navigate({ url: "your-app.com/feature" })
puppeteer_evaluate({ script: "document.readyState === 'complete'" })
```

#### 2. **User Interaction Testing**
```javascript
// Test all interactive elements
puppeteer_click({ selector: ".button" })
puppeteer_fill({ selector: "#input", value: "test" })
puppeteer_select({ selector: "#dropdown", value: "option1" })
puppeteer_hover({ selector: ".tooltip-trigger" })
```

#### 3. **State Verification**
```javascript
// Verify DOM changes and data loading
puppeteer_evaluate({ 
  script: "document.querySelector('.result').textContent" 
})
puppeteer_evaluate({ 
  script: "document.querySelectorAll('.item').length" 
})
```

#### 4. **Error Validation**
```javascript
// Test error scenarios
puppeteer_fill({ selector: "#email", value: "invalid-email" })
puppeteer_click({ selector: "#submit" })
puppeteer_evaluate({ 
  script: "document.querySelector('.error-message')?.textContent" 
})
```

#### 5. **Performance Monitoring**
```javascript
puppeteer_evaluate({
  script: `
    const metrics = performance.getEntriesByType('navigation')[0];
    return {
      pageLoad: metrics.loadEventEnd - metrics.fetchStart,
      domReady: metrics.domContentLoadedEventEnd,
      firstPaint: performance.getEntriesByName('first-paint')[0]?.startTime
    }
  `
})
```

#### 6. **Visual Documentation**
```javascript
// Capture states for regression testing
puppeteer_screenshot({ name: "feature-before", selector: ".container" })
// ... perform actions ...
puppeteer_screenshot({ name: "feature-after", selector: ".container" })
```

### Testing Checklist for AI Implementation:
- [ ] Navigate to every new route/page
- [ ] Click every button and verify result
- [ ] Fill every form field with valid/invalid data
- [ ] Test all dropdowns and selections
- [ ] Verify all hover states and tooltips
- [ ] Check loading states and spinners
- [ ] Validate error messages and edge cases
- [ ] Measure performance metrics
- [ ] Test responsive behavior at different viewports
- [ ] Verify accessibility (keyboard nav, ARIA)