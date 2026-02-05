---
name: "log-driven-debugging"
description: "Guides systematic verification-based debugging for JavaScript/TypeScript (Node.js, Bun, browser). Use when: (1) User reports unexpected behavior, errors, or bugs, (2) Need to trace data flow or state changes without assuming root causes, (3) Issue requires identifying exactly where code deviates from expected behavior."
---

# Log-Driven Debugging

## Core Principle

**Never assume the cause. Verify with logs.**

## Debugging Workflow

1. **Gather information** - What is the exact symptom? When does it occur? What is the expected behavior?
2. **Form hypothesis** - What could cause this? List probable causes ranked by likelihood.
3. **Add targeted logs** - Add console.log statements at probable cause points to capture state.
4. **Request logs from user** - Ask them to run the code and paste the console output.
5. **Analyze logs** - Compare actual values to expected values. Verify or invalidate each hypothesis.
6. **Iterate** - Based on evidence, refine hypothesis and repeat until root cause is found.

## Log Placement Strategy

- **Entry logs**: Log function/method entry with input parameters
- **Exit logs**: Log function/method exit with return values
- **Before/after critical operations**: Log state before and after mutations
- **Conditional branches**: Log which branch executed and why
- **Async operations**: Log before and after promises/await points
- **Boundary points**: Log data entering/leaving modules or components

## What to Log

Include sufficient context for diagnosis:

- **Variable values**: `console.log('userId:', userId)`
- **Object shapes**: `console.log('user:', JSON.stringify(user, null, 2))`
- **Execution path**: `console.log('[AuthService] Checking permissions...')`
- **Error details**: `console.error('[API] Failed:', error.message, error.stack)`
- **Timestamps**: Use `Date.now()` or `console.time()` for timing issues

## Requesting Logs from Users

Always be specific about what to capture:

```
Please run your code and paste the complete console output here.
If the issue is in a browser, open DevTools (F12) and paste:
1. Console tab output
2. Network tab response (if API-related)
```

## Interpreting Logs

- **Expected ≠ Actual**: You've found the deviation point
- **Log missing**: The code path is different than expected
- **Undefined/null**: Value not set at this point (trace upstream)
- **Unexpected value**: Root cause is upstream or in transformation

## Common Patterns

See [patterns.md](references/patterns.md) for debugging scenarios:
- API/Network issues
- State mutation issues
- Async/timing issues
- Type/TypeScript issues
