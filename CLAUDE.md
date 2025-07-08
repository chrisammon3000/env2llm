# General Guidelines
- Think through each problem step-by-step
- Always reason from first principles
- Stop & take note of your assumptions every once in a while

# Scope Restrictions
Adhere to an appropriate scope for a proof of concept, meaning ONLY ESSENTIAL BUSINESS LOGIC
- No error handling
- No monitoring, metrics or observability other than logging
    - No performance metrics
    - No processing time metrics
    - No resource usage metrics
- No migration or backwards compatibility concerns
- No scalability concerns
- No recovery mechanisms
    - No retry logic
    - No circuit breakers
    - No graceful degradation
- No fallback strategies
    - No Degraded modes
    - No manual review triggers
    - No confidence decay
- No quality metrics
- No failure modes

## Documentation Guidelines
- Think through each problem step-by-step
- Always reason from first principles
- Stop & take note of your assumptions every once in a while
- When working in the conceptual and logical layers, restrict your thinking to high level concepts only
- Specific implementation details are appropiate in the physical layer
- Only address the minimum scope appropriate for a PoC
- Document existing behavior, not aspirational features
- When documenting algorithms, always use pseudocode
- Do not include timeline references in any documents
- Follow existing patterns, conventions and formats in the existing documentation

## Code
- Always find the cleanest approach to each implementation
- Only write the minimum code necessary to achieve the desired functionality
- Follow existing patterns and conventions
- Make sure you understand the existing codebase and the context of the project before writing code
- Do not include unapproved fallback logic. When in doubt, raise an error.
- Include comments and docstrings to explain your code to humans
- Disregard backwards compatibility when writing code logic

## Git
- Do not include "Co-Authored-By: Claude <noreply@anthropic.com>" in commits

## Testing
- Always run Python scripts using the appropriate .venv-* environment with the python3 executable