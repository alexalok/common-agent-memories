# Feature Planning Workflow

This document outlines the standard workflow for planning new features/tasks in the project.

## Planning Phase

When a user requests a new feature, follow these steps:

### 1. Branch Management
- Ensure work is done on a new branch `<feature-name>` with a descriptive but consice name

### 2. Feature Description
- If not provided, ask the user for a clear description of the feature
- Understand the requirements, constraints, and expected outcomes

### 3. Comprehensive Planning
- Analyze the existing codebase to understand how the feature fits
- Reference existing symbols, patterns, and conventions
- Create a detailed plan that includes:
  - Overview of the feature
  - Technical approach
  - Dependencies and considerations
  - Clear TODO steps with specific implementation details
  - References to existing code patterns and files

### 4. Plan Documentation
- Save the plan to `.agent/tasks/<feature_name>.md`
- Include:
  - Feature description
  - Technical design decisions
  - Step-by-step implementation plan
  - Acceptance criteria
  - Testing considerations

### 5. Review and Iteration
- Present the plan to the user for review
- Ask for feedback and clarification
- Iterate on the plan until approved
- Only proceed with implementation after explicit approval

## Example Structure for Task File

```markdown
# Feature: <Feature Name>

## Description
[Clear description of what the feature does and why it's needed]

## Technical Approach
[How the feature will be implemented technically]

## Dependencies
- [List of dependencies on existing code/systems]

## Implementation Steps
1. [ ] Step 1: [Specific action with file references]
2. [ ] Step 2: [Specific action with method/class references]
3. [ ] Step 3: [Testing approach]
...

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
...

## Testing Plan
[How the feature will be tested, if needed]
```