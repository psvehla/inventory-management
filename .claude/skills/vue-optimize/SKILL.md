---
name: vue-optimize
description: Analyze Vue 3 component structure and suggest optimizations for performance, code reuse, and maintainability. Works on any Vue 3 codebase. Use when you want to audit components for inefficiencies, duplicated logic, or structural issues.
user-invocable: true
argument-hint: [file-or-directory]
---

# Vue 3 Component Optimizer

Analyze Vue 3 components for performance issues, code reuse opportunities, and structural improvements. If `$ARGUMENTS` is provided, scope the analysis to that file or directory. Otherwise, analyze all `.vue` files in the project.

## Analysis Process

### 1. Discover the Codebase

Before analyzing, understand the project context:

- Glob for all `.vue` files to build a component inventory
- Read the router config to understand page vs shared components
- Identify composables (files exporting `use*` functions in `composables/` or similar)
- Identify the component pattern in use: `<script setup>`, Options API, or `setup()` return
- Note any UI library imports (Vuetify, PrimeVue, Element Plus, etc.)

### 2. Analyze Performance

Check each component for these issues, ordered by impact:

**High Impact**
- **Unnecessary reactivity**: Large objects or arrays wrapped in `ref()`/`reactive()` that never change — should be `shallowRef()` or plain `const`
- **Missing computed**: Derived values recalculated in templates or methods instead of using `computed()`
- **Expensive v-for without key**: `v-for` loops missing `:key` or using array index as key on dynamic lists
- **Watchers that should be computed**: `watch()` that sets a ref based on other reactive data — replace with `computed()`
- **Unbounded watchers**: `watch()` or `watchEffect()` without cleanup that could cause memory leaks (missing `onUnmounted` cleanup for timers, event listeners, subscriptions)

**Medium Impact**
- **Large component bundles**: Components over 300 lines that could be split or lazy-loaded
- **Synchronous heavy computation**: Expensive operations in `setup()` or lifecycle hooks that block rendering
- **Redundant re-renders**: Passing new object/array literals as props (`:style="{ color: 'red' }"` in loops) — extract to constants or computed
- **Missing `v-once`**: Static content rendered inside dynamic components that never changes
- **Event handler allocations in loops**: Inline arrow functions in `v-for` creating new functions per render — extract to methods

**Low Impact**
- **Unused imports**: Imported components, composables, or utilities that are never referenced
- **Unnecessary template refs**: `ref` attributes on elements that are never accessed via `templateRef.value`
- **Over-watching**: `watch()` on an entire object when only one property is needed — use a getter

### 3. Analyze Code Reuse

Look for opportunities to reduce duplication:

**Composable Extraction**
- Identical or near-identical reactive logic (ref + watch + methods) appearing in 2+ components — extract to a shared composable
- Repeated API call patterns (fetch + loading + error state) — extract to a `useFetch` or domain-specific composable
- Duplicated formatting functions (dates, currencies, numbers) — extract to a utility or composable

**Component Extraction**
- Repeated template patterns (card layouts, stat displays, status badges, empty states) used in 2+ views — extract to a shared component
- Large template sections (50+ lines) that are self-contained — extract to a child component
- Duplicated form field patterns — extract to a form component

**Slot & Provide/Inject Opportunities**
- Prop drilling through 3+ component levels — consider `provide`/`inject`
- Components with many conditional template blocks — consider named slots for customization

### 4. Analyze Structure

Evaluate component organization and maintainability:

- **Single Responsibility**: Components handling more than one concern (e.g., data fetching + display + editing) — split into container/presentational
- **Component size**: Flag components over 300 lines of template or 200 lines of script as candidates for splitting
- **Prop count**: Components accepting more than 8 props — consider grouping into an object prop or splitting the component
- **Event count**: Components emitting more than 5 events — consider consolidating with an event object or using a store
- **Mixed concerns**: API calls directly in components instead of composables or a service layer
- **Inconsistent patterns**: Mix of Options API and Composition API, or mix of `<script setup>` and `setup()` return — flag for consistency

### 5. Generate Report

Present findings as a prioritized, actionable report:

```
## Vue Component Analysis Report

### Critical (fix immediately)
1. **[Performance]** `ComponentName.vue:42` — Description of issue
   **Suggested fix:** Code snippet or approach

### Important (fix soon)
2. **[Reuse]** `ComponentA.vue` + `ComponentB.vue` — Description of duplication
   **Suggested fix:** Extract to `useXxx()` composable

### Nice to Have (improve when touching)
3. **[Structure]** `LargeComponent.vue` — 450 lines, handles fetch + display + edit
   **Suggested fix:** Split into container + presentational components
```

Group findings by severity: **Critical** (bugs, memory leaks, major perf), **Important** (duplication, medium perf), **Nice to Have** (style, minor improvements).

Include line numbers and file paths for every finding.

### 6. Interactive Fix Application

After presenting the report, ask the user which findings they want to fix. For each selected fix:

1. Show the proposed change (before/after)
2. Ask for confirmation before applying
3. Apply the fix using the appropriate tool (Edit for modifications, Write for new files)
4. For `.vue` file creation or significant modification, delegate to the `vue-expert` subagent
5. After applying, verify the fix doesn't break anything (check for import errors, template references)

### 7. Post-Fix Verification

After all selected fixes are applied:

- Run the project's linter if configured (`npm run lint` or similar)
- Check for TypeScript errors if applicable (`npx vue-tsc --noEmit`)
- Summarize what was changed and any manual follow-up needed

## What NOT to Do

- Do not change business logic, API endpoints, or data models
- Do not remove features or alter user-facing behavior
- Do not refactor working code just for style preferences
- Do not add dependencies without asking
- Do not apply fixes without user confirmation
- Do not flag issues in third-party library code or node_modules
- Do not suggest micro-optimizations with negligible real-world impact
