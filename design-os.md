

# Installation Guide

Get Design OS up and running in minutes.



## 1Clone the Repository

Copy

```
git clone https://github.com/buildermethods/design-os.git my-project-design
cd my-project-design
```

Replace `my-project-design` with whatever you want to name your design workspace.

## 2Remove the Original Remote

Copy

```
git remote remove origin
```

Now you have a clean local instance ready to use.

## 3Install Dependencies

Copy

```
npm install
```

## 4Start the Dev Server

Copy

```
npm run dev
```

Open [http://localhost:3000](http://localhost:3000/) in your browser.

## 5Open Claude Code

In the same project directory, start Claude Code:

Copy

```
claude
```

You're ready to start designing. Run `/product-vision` to begin defining your product.

## Optional: Save as Your Own Template

If you want to reuse Design OS for future projects:

1. Push to your own GitHub repository:

Copy

```
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
git push -u origin main
```

1. Go to your repository on GitHub, click **Settings**, and check **Template repository**.

Now you can create new instances using GitHub's "Use this template" button.





# Requirements

What you need to run Design OS and use your exported components.



## Running Design OS

Design OS runs locally on your machine. You'll need:

- **Node.js** (v18 or higher)
- **npm** (comes with Node.js)
- **An AI coding agent** — Claude Code is recommended as Design OS comes with custom slash commands for Claude Code. If you prefer a different tool (Cursor, Codex, Gemini, etc.) you can simply reference the commands in those tools using @ followed by the path to the `.claude/commands/design-os/[command-name].md` file.

## Installing Your Exported Components

When you export your designs, you get production-ready React components. Your target codebase needs:

### Required

- **React** (v18 or higher)
- **Tailwind CSS** (v4) — Components use Tailwind utility classes for styling

### Backend

Your backend can be anything—Rails, Laravel, Next.js API routes, Python, Go, whatever. Design OS only handles the frontend design layer.





# Workflow Overview

Design OS uses slash commands to guide you through the design process.

Each command is a conversation—the AI asks questions, you provide direction, and together you shape your product.

## The Design Workflow

Design OS follows a structured sequence. Each step builds on the previous one.

### Phase 1: Product Planning

Before designing any screens, establish the foundation:

1. **Product Vision** — Define what you're building and why
2. **Product Roadmap** — Break your product into sections
3. **Data Model** — Define the core entities in your system
4. **Design Tokens** — Choose colors and typography
5. **Application Shell** — Design navigation and layout



### Phase 2: Section Design

Once the foundation is set, work through each section:

1. **Shape the Section** — Define scope and requirements
2. **Create Sample Data** — Generate realistic data and types
3. **Design the Screen** — Build the actual React components
4. **Capture Screenshots** — Document the design (optional)

Repeat for each section in your roadmap.



### Phase 3: Export

When all sections are designed:

1. **Export** — Generate the complete handoff package



## Quick Reference

Command

Purpose

```
/product-vision
```

Define product name, description, problems, features

```
/product-roadmap
```

Break product into sections

```
/data-model
```

Define core entities and relationships

```
/design-tokens
```

Choose colors and typography

```
/design-shell
```

Design navigation and layout

```
/shape-section
```

Define a section's scope and requirements

```
/sample-data
```

Generate sample data and TypeScript types

```
/design-screen
```

Create screen design components

```
/screenshot-design
```

Capture screenshots

```
/export-product
```

Generate the complete handoff package

## Tips

- **Follow the sequence** — Each step builds on the previous. Don't skip ahead.
- **Be specific** — The more detail you provide, the better the output.
- **Iterate** — Each command is a conversation. Refine until you're happy.
- **Restart the dev server** — After creating new components, restart to see changes.





# Product Planning

Phase 1: Establish the foundation for your product before designing any screens.



## 1Product Vision

Copy

```
/product-vision
```

Define your product's core identity. This is a conversational process where you'll establish:

- **Product name** — A clear, memorable name
- **Description** — 1-3 sentences capturing the essence
- **Problems & solutions** — What pain points you're addressing and how
- **Key features** — The main capabilities that make this possible

Share any notes, ideas, or rough thoughts you have about what you're building. The AI will ask clarifying questions and help you refine everything into a clear product overview.

**Creates:** `product/product-overview.md`

## 2Product Roadmap

Copy

```
/product-roadmap
```

Break your product into 3-5 sections. Each section represents:

- A navigation item in your app's UI
- A self-contained feature area that can be designed and built independently
- A logical phase of your development roadmap

Based on your product overview, the AI will propose sections and discuss ordering. Sections are sequenced by development priority—the first section is your core functionality, with each subsequent section building on it.

**Creates:** `product/product-roadmap.md`

## 3Data Model

Copy

```
/data-model
```

Define the core entities in your product—the "nouns" of your system. What are the main things users create, view, and manage?

The focus is conceptual, not technical:

- **Entity names** — User, Project, Invoice, Task, etc.
- **Plain-language descriptions** — What each entity represents
- **Relationships** — How entities connect to each other

Don't worry about database schemas or field types. Keep it minimal. The implementation agent will extend the model with additional details as needed.

**Creates:** `product/data-model/data-model.md`

## 4Design Tokens

Copy

```
/design-tokens
```

Choose your visual identity:

### Colors

Select from Tailwind's built-in color palette:

- **Primary** — Main accent for buttons, links, key actions (e.g., `blue`, `indigo`, `emerald`, `lime`)
- **Secondary** — Complementary accent for tags, highlights (e.g., `violet`, `amber`, `teal`)
- **Neutral** — Backgrounds, text, borders (e.g., `slate`, `gray`, `zinc`, `stone`)

### Typography

Select from Google Fonts:

- **Heading** — For titles and section headers (e.g., `DM Sans`, `Inter`, `Space Grotesk`)
- **Body** — For paragraphs and UI text (e.g., `Inter`, `Source Sans 3`, `Nunito Sans`)
- **Mono** — For code and technical content (e.g., `JetBrains Mono`, `Fira Code`)

The AI will suggest options based on your product type and help you find a combination that fits.

**Creates:** `product/design-system/colors.json`, `product/design-system/typography.json`

## 5Application Shell

Copy

```
/design-shell
```

Design the persistent navigation and layout that wraps all your sections. Choose from common patterns:

- **Sidebar Navigation** — Vertical nav on the left, content on the right. Best for dashboard-style tools, admin panels, apps with many sections.
- **Top Navigation** — Horizontal nav at top, content below. Best for simpler apps, marketing-style products, fewer sections.
- **Minimal Header** — Just logo + user menu. Best for single-purpose tools, wizard-style flows.

You'll also define:

- User menu placement and contents
- Responsive behavior (how it adapts on mobile)
- Any additional nav items (Settings, Help, etc.)

The shell is implemented as React components that will wrap your section screen designs.

**Creates:**

- `product/shell/spec.md` — Shell specification
- `src/shell/components/AppShell.tsx` — Main shell wrapper
- `src/shell/components/MainNav.tsx` — Navigation component
- `src/shell/components/UserMenu.tsx` — User menu component
- `src/shell/ShellPreview.tsx` — Preview wrapper for Design OS







# Designing Sections

Phase 2: Work through each section in your roadmap, completing these steps for each one.



After completing Product Planning, you're ready to design individual sections.

## 1Shape the Section

Copy

```
/shape-section
```

Define what the section does. If you have multiple sections, you'll be asked which one to work on.

This is a conversational process to establish:

- **Overview** — What this section is for (2-3 sentences)
- **User flows** — The main actions and step-by-step interactions
- **UI requirements** — Specific layouts, patterns, or components needed
- **Scope boundaries** — What's intentionally excluded

Share any notes or ideas you have. The AI will ask clarifying questions about user actions, information to display, and UI patterns. Focus on experience and interface requirements—no backend or database details.

You'll also be asked whether this section should display inside the application shell (most sections do) or as a standalone page (for things like landing pages or embedded widgets).

**Creates:** `product/sections/[section-id]/spec.md`

## 2Create Sample Data

Copy

```
/sample-data
```

Generate realistic sample data based on the spec. This data populates your screen designs and makes them feel real.

The AI analyzes your section spec and proposes a data structure:

- **Entities** — Based on your global data model (if defined) or inferred from the spec
- **Relationships** — How the data connects
- **Sample records** — 5-10 realistic entries with varied content

You'll also get TypeScript types generated automatically:

- **Data interfaces** — Type definitions for each entity
- **Props interface** — What the component expects, including callbacks for actions (onView, onEdit, onDelete, etc.)

The sample data includes:

- Realistic names, dates, and descriptions (not "Lorem ipsum")
- Varied content lengths and statuses
- Edge cases (empty arrays, long text)

**Creates:**

- `product/sections/[section-id]/data.json` — Sample data with `_meta` descriptions
- `product/sections/[section-id]/types.ts` — TypeScript interfaces

## 3Design the Screen

Copy

```
/design-screen
```

Build the actual React components for the section. This is where the spec and sample data become a working UI.

### What Gets Created

**Exportable components** (props-based, portable):

The main component and any sub-components, all accepting data and callbacks via props. These are what get exported to your codebase.

Copy

```
// Example: Components accept props, never import data directly
export function InvoiceList({
  invoices,
  onView,
  onEdit,
  onDelete,
  onCreate
}: InvoiceListProps) {
  // ...
}
```

**Preview wrapper** (for Design OS only):

A wrapper that imports the sample data and feeds it to the component, so you can see it running in Design OS.

### Design Requirements

All screen designs include:

- **Mobile responsive** — Tailwind responsive prefixes (`sm:`, `md:`, `lg:`)
- **Light & dark mode** — Using `dark:` variants
- **Design tokens applied** — Your color palette and typography choices
- **All spec requirements** — Every user flow and UI requirement implemented

### Multiple Views

If the spec implies multiple views (list view, detail view, form, etc.), you'll be asked which to build first. Run `/design-screen` again for additional views.

**Creates:**

- `src/sections/[section-id]/components/[ViewName].tsx` — Main component
- `src/sections/[section-id]/components/[SubComponent].tsx` — Sub-components as needed
- `src/sections/[section-id]/components/index.ts` — Component exports
- `src/sections/[section-id]/[ViewName].tsx` — Preview wrapper

**Important:** Restart your dev server after creating screen designs to see the changes.

## 4Capture Screenshots (Optional)

Copy

```
/screenshot-design
```

Take screenshots of your screen designs for documentation. Screenshots are saved alongside the spec and data files.

This command:

1. Starts the dev server automatically
2. Navigates to your screen design
3. Hides the Design OS navigation bar
4. Captures a full-page screenshot

Screenshots are useful for:

- Visual reference during implementation
- Documentation and handoff materials
- Comparing designs across sections

**Requires:** Playwright MCP server. If not installed, you'll be prompted with setup instructions.

**Creates:** `product/sections/[section-id]/[screen-name].png`

## Repeat for Each Section

Work through your roadmap sections in order. Each section builds on the foundation you established and benefits from the consistency of your global data model and design tokens.







# Export

Phase 3: Generate everything your implementation agent needs to build the product.





## When to Export

You're ready to export when:

- Product vision and roadmap are defined
- At least one section has screen designs
- You're satisfied with the design direction

You can export at any point—it doesn't have to be "complete." Exporting generates a snapshot of your current designs. You can always export again later as you add more sections.

## Running the Export

Copy

```
/export-product
```

The export command:

1. **Checks prerequisites** — Verifies required files exist
2. **Gathers all design assets** — Components, types, data, tokens
3. **Generates implementation instructions** — Including ready-to-use prompts
4. **Generates test instructions** — TDD specs for each section
5. **Creates the export package** — A complete `product-plan/` directory
6. **Creates a zip file** — `product-plan.zip` for easy download

## What's Included

### Ready-to-Use Prompts

Copy

```
product-plan/prompts/
├── one-shot-prompt.md     # Prompt for full implementation
└── section-prompt.md      # Prompt template for section-by-section
```

These are pre-written prompts you copy/paste into your coding agent. They reference the instruction files and prompt your agent to ask important clarifying questions about authentication, user modeling, and tech stack before implementing.

### Instructions

Copy

```
product-plan/
├── product-overview.md              # Product summary (always provide)
└── instructions/
    ├── one-shot-instructions.md     # All milestones combined
    └── incremental/                 # Milestone-by-milestone implementation
        ├── 01-foundation.md         # Design tokens, data model, routing
        ├── 02-shell.md              # Application shell implementation
        ├── 03-[section-id].md       # One per section
        └── ...
```

**product-overview.md** provides context about the full product—always include it with any implementation session.

**one-shot-instructions.md** combines all milestones into a single document. Use this with `one-shot-prompt.md` for full implementation.

**Incremental instructions** break the work into milestones. Use these with `section-prompt.md` for step-by-step implementation.

### Design System

Copy

```
product-plan/design-system/
├── tokens.css           # CSS custom properties
├── tailwind-colors.md   # Tailwind configuration guide
└── fonts.md             # Google Fonts setup
```

### Data Model

Copy

```
product-plan/data-model/
├── README.md            # Entity descriptions
├── types.ts             # TypeScript interfaces
└── sample-data.json     # Combined sample data
```

### Shell Components

Copy

```
product-plan/shell/
├── README.md            # Design intent
├── components/
│   ├── AppShell.tsx     # Main layout wrapper
│   ├── MainNav.tsx      # Navigation
│   ├── UserMenu.tsx     # User menu
│   └── index.ts         # Exports
└── screenshot.png       # Visual reference (if captured)
```

### Section Components

For each section:

Copy

```
product-plan/sections/[section-id]/
├── README.md            # Feature overview, user flows
├── tests.md             # Test-writing instructions (TDD)
├── components/
│   ├── [Component].tsx  # Exportable components
│   └── index.ts         # Exports
├── types.ts             # TypeScript interfaces
├── sample-data.json     # Test data
└── screenshot.png       # Visual reference (if captured)
```

### Test Instructions

Each section includes a `tests.md` file with framework-agnostic test-writing instructions:

- **User flow tests** — Success and failure paths for key interactions
- **Empty state tests** — Verifying UI when no records exist
- **Component interaction tests** — Specific UI elements and behaviors to verify

These instructions describe WHAT to test, not HOW—your coding agent adapts them to your test framework (Jest, Vitest, Playwright, Cypress, RSpec, Minitest, PHPUnit, etc.).

## About the Components

Exported components are:

- **Props-based** — Accept data and callbacks via props, never import data directly
- **Portable** — Work with any React setup, no Design OS dependencies
- **Complete** — Full styling, responsive design, dark mode support
- **Production-ready** — Not prototypes or mockups

Copy

```
// Components expect data and callbacks as props
 navigate(`/invoices/${id}`)}
  onEdit={(id) => navigate(`/invoices/${id}/edit`)}
  onDelete={(id) => confirmDelete(id)}
  onCreate={() => navigate('/invoices/new')}
/>
```

Your implementation agent's job is to:

- Wire up callbacks to routing and API calls
- Replace sample data with real data from your backend
- Implement proper error handling and loading states
- Implement empty states when no records exist (first-time users, after deletions)
- Build the backend APIs the components need
- Write tests based on the provided test instructions (TDD approach)





# Codebase Implementation

How to work with your AI coding agent to build the product from your Design OS export.



## Getting Started

1. Copy the `product-plan/` folder into your target codebase
2. Start your AI coding agent (Claude Code, Cursor, etc.)
3. Choose your implementation approach below

## Implementation Approaches

Choose the approach that best fits your project:

- [Incremental Implementation](https://buildermethods.com/design-os/incremental-implementation) — Recommended for larger products or when you want to review progress section by section.
- [One-Shot Implementation](https://buildermethods.com/design-os/one-shot-implementation) — For simpler products or when you want to build everything in one session.

## Start with Plan Mode

The provided prompts assume you'll deliver them to Claude Code (or Cursor) in **Plan mode**. This encourages the agent to:

- Analyze the instructions and design specifications thoroughly
- Ask clarifying questions about functional requirements
- Build a complete implementation plan that works for your specific codebase and tech stack

Once you're satisfied with the plan, give your agent the go-ahead to begin implementation.

## Implementation Instructions

The provided prompts, instruction files, and referenced assets are designed to instruct your agent to:

- Read and analyze the design specifications before writing code
- Use the provided UI components as-is—don't redesign or restyle them
- Follow the data model types defined in the export
- Implement backend: database schema, API endpoints, business logic
- Use test-driven development with the provided `tests.md` instructions
- Handle empty states and error states gracefully
- Use sample data for initial testing before building real APIs
- Ask clarifying questions about auth, user modeling, and integration points

## Tips

- **Use the pre-written prompts** — They include important clarifying questions about auth and data modeling.
- **Add your own notes** — Customize prompts with project-specific context when needed.
- **Build on your designs** — Use completed sections as the starting point for future feature development.
- **Review thoroughly** — Check plans and implementations carefully to catch details and inconsistencies.
- **Fill in the gaps** — Backend business logic may need manual additions. Incremental implementation helps you identify these along the way.







# Incremental Implementation

Build your product section by section for more control and easier review.



For larger products or when you want to review progress incrementally, this approach lets you build one section at a time.

## How It Works

Work through the instructions in order:

1. **Foundation** (`instructions/incremental/01-foundation.md`) — Design tokens, data model types, routing
2. **Shell** (`instructions/incremental/02-shell.md`) — Application shell and navigation
3. **Sections** (`instructions/incremental/03-*.md`, `04-*.md`, etc.) — Each feature section, one at a time

## For Each Milestone

1. Open `product-plan/prompts/section-prompt.md`
2. Fill in the section variables at the top (SECTION_NAME, SECTION_ID, NN)
3. Add any section-specific notes
4. Deliver the prompt to your coding agent in **Plan mode**
5. Answer clarifying questions, then give the go-ahead to implement
6. Review and test before moving to the next milestone



#### Why Plan Mode?

Plan mode encourages the agent to analyze the instructions thoroughly, ask clarifying questions, and build a complete implementation plan before writing code. This catches issues early and ensures the plan works for your codebase.

## The Section Prompt

The section prompt is designed to give your agent everything it needs:

- References the section's instruction file and assets
- Points to `tests.md` for test-driven development
- Asks about auth, data relationships, and integration points

## Benefits of Incremental Implementation

- **Easier review** — Review and test each section before moving on
- **Catch issues early** — Problems are contained to a single section
- **Better control** — You decide when each section is ready
- **Flexible pacing** — Take breaks between sections as needed







# One-Shot Implementation

Build your entire product in a single session for simpler projects.



For simpler products or when you want to build everything in one session, this approach lets your agent plan and implement the entire product at once.

## How It Works

1. Open `product-plan/prompts/one-shot-prompt.md`
2. Add any additional notes (tech stack preferences, constraints)
3. Deliver the prompt to your coding agent in **Plan mode**
4. Answer the agent's clarifying questions about auth, user modeling, etc.
5. Give the go-ahead and let the agent implement everything



#### Why Plan Mode?

Plan mode encourages the agent to analyze the instructions thoroughly, ask clarifying questions, and build a complete implementation plan before writing code. This catches issues early and ensures the plan works for your codebase.

The prompt references `product-overview.md` and `instructions/one-shot-instructions.md`, and guides your agent to ask important questions before starting.

## Clarifying Questions

The prompt includes questions about:

- **Authentication & authorization** — Login methods, user roles
- **User & account modeling** — Single-user vs multi-user, teams/workspaces
- **Tech stack preferences** — Backend framework, database
- **Any other clarifications** — Project-specific requirements

## When to Use One-Shot

- **Smaller products** — Few sections, straightforward features
- **Prototypes** — Quick builds where iteration happens later
- **Familiar patterns** — When you know exactly what you want
- **Time constraints** — When you need something built fast

## Tips for One-Shot Success

- **Answer all questions first** — Don't let the agent start until clarifying questions are resolved
- **Review the plan** — Ask to see the implementation plan before coding begins
- **Test incrementally** — Even in one-shot mode, test as sections are completed