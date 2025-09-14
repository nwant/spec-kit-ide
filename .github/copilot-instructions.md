# GitHub Copilot Instructions for Spec Kit Development

## Context
This project uses GitHub's Spec Kit framework for Spec-Driven Development (SDD). Specifications are the source of truth that drive code generation through AI agents.

## Key Concepts

### Spec-Driven Development Workflow
1. **Specify**: Create detailed specifications for features
2. **Plan**: Generate implementation plans from specifications
3. **Tasks**: Break down plans into actionable tasks
4. **Implement**: Generate code using AI agents

### Project Structure
```
project/
├── memory/
│   └── constitution.md    # Core principles and constraints
├── specs/
│   └── NNN/               # Numbered specification folders (001, 002, etc.)
│       ├── spec.yml       # Feature specification
│       ├── plan.yml       # Implementation plan
│       └── tasks.yml      # Task breakdown
├── scripts/               # Automation scripts
└── templates/            # Spec kit templates
```

## Specification Format Guidelines

### When Writing Specifications
- Be explicit and unambiguous - AI agents are "literal-minded"
- Include clear functional requirements that are testable
- Mark ambiguities with `[NEEDS CLARIFICATION]`
- Reference the constitution for architectural decisions
- Use structured YAML format with proper schema

### Example Specification Structure
```yaml
name: Feature Name
description: |
  Detailed description of the feature
requirements:
  functional:
    - id: F001
      description: Specific, testable requirement
      acceptance_criteria:
        - Measurable criterion
  non_functional:
    - id: NF001
      description: Performance/security requirement
user_scenarios:
  - id: US001
    actor: User Role
    action: What they do
    outcome: Expected result
```

## Code Generation Patterns

### Constitution Compliance
Always check generated code against `memory/constitution.md` principles:
- Library-first development
- Modular architecture
- Simplicity over complexity
- Clear separation of concerns

### Task References
When implementing tasks, reference them in comments:
```python
# Task T001: Implement user authentication
def authenticate_user(credentials):
    # Implementation following spec 001 requirements
    pass
```

### Specification Traceability
Link code to specifications:
```python
class UserAuth:
    """
    Implements authentication as specified in specs/001/spec.yml
    
    Requirements:
    - F001: User login with email/password
    - F002: Session management
    - NF001: Passwords hashed with bcrypt
    """
```

## Common Patterns

### API Endpoints from Specs
```python
# Generated from specs/002/spec.yml - API endpoints
@app.route('/api/users', methods=['POST'])
def create_user():
    """Implements requirement F003: User creation endpoint"""
    pass
```

### Test Generation
```python
# Test generated from specs/001/spec.yml acceptance criteria
def test_user_authentication():
    """Validates F001: User can authenticate with valid credentials"""
    assert authenticate_user(valid_credentials) == True
```

### Error Handling
```python
# Error handling per constitution.md error principles
try:
    result = process_specification()
except SpecificationError as e:
    # Log and handle per spec kit patterns
    logger.error(f"Spec validation failed: {e}")
    raise
```

## Copilot Suggestions

### For New Features
1. First check if a specification exists in `specs/`
2. Reference the constitution for architectural patterns
3. Follow the numbered task sequence (T001, T002, etc.)
4. Include specification traceability comments

### For Modifications
1. Update the specification first
2. Regenerate plan and tasks if needed
3. Maintain backward compatibility per constitution
4. Document specification changes in commits

### For Tests
1. Generate tests from acceptance criteria
2. Cover all functional requirements
3. Include edge cases mentioned in specs
4. Reference specification IDs in test names

## Anti-Patterns to Avoid
- Don't implement features without specifications
- Don't violate constitution principles
- Don't skip the plan/task phases
- Don't remove specification traceability comments
- Don't make assumptions - mark as `[NEEDS CLARIFICATION]`

## Useful Commands
- `specify <feature>` - Create new specification
- `specify --plan <spec>` - Generate plan from spec
- `specify --tasks <plan>` - Generate tasks from plan
- Check constitution compliance before committing

## Remember
- Specifications are executable documentation
- AI agents need explicit instructions
- The constitution is the project's DNA
- Every line of code traces back to a specification
