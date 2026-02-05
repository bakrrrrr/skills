# Debugging Patterns

## API/Network Issues

**Symptom**: Data not arriving, wrong data, request failures

**Hypothesis**: Response not parsed correctly, wrong endpoint, network issue

**Log placement**:
```javascript
// Request
console.log('[API] Calling:', url, { params: queryParams })

// Response
try {
  const response = await fetch(url, options)
  console.log('[API] Response status:', response.status)
  const data = await response.json()
  console.log('[API] Response data:', JSON.stringify(data, null, 2))
  return data
} catch (error) {
  console.error('[API] Error:', error.message)
  throw error
}
```

---

## State Mutation Issues

**Symptom**: Value changes unexpectedly, UI shows wrong data

**Hypothesis**: State being modified in unexpected place, reference sharing

**Log placement**:
```javascript
// Before mutation
console.log('[State] Before update:', JSON.stringify(currentState))

// After mutation
Object.assign(currentState, updates)
console.log('[State] After update:', JSON.stringify(currentState))

// React component
console.log('[Component] Props:', props, 'State:', state)
```

---

## Async/Timing Issues

**Symptom**: Race conditions, promises resolve in wrong order, undefined values

**Hypothesis**: Code running before async operation completes, order of execution

**Log placement**:
```javascript
console.log('[Async] Starting operation:', operationId)

Promise.resolve()
  .then(() => {
    console.log('[Async] Step 1 complete')
    return step2()
  })
  .then(() => {
    console.log('[Async] Step 2 complete')
  })

// With async/await
async function process() {
  console.log('[Async] Step A starting')
  await stepA()
  console.log('[Async] Step A complete, result:', resultA)
}
```

---

## Type/TypeScript Issues

**Symptom**: Property undefined, method not found, runtime errors

**Hypothesis**: Wrong type at runtime, type assertion incorrect, null not handled

**Log placement**:
```javascript
console.log('[Types] user:', typeof user, user)
console.log('[Types] user.id:', typeof user?.id, user?.id)
console.log('[Types] items.length:', items?.length)

// Check array operations
console.log('[Types] Array.isArray(items):', Array.isArray(items))
console.log('[Types] items[0]:', items?.[0])
```

---

## Function Not Being Called

**Symptom**: Expected function doesn't execute, handler not triggered

**Hypothesis**: Event not attached, condition preventing execution, wrong reference

**Log placement**:
```javascript
// Event handlers
button.addEventListener('click', (e) => {
  console.log('[Event] Click detected on:', e.target)
  console.log('[Event] Handler executing')
  handleClick()
})

// Conditional execution
if (shouldExecute) {
  console.log('[Logic] Condition met, executing')
  doSomething()
} else {
  console.log('[Logic] Condition false, skipping. shouldExecute:', shouldExecute)
}
```

---

## Environment/Configuration Issues

**Symptom**: Works in one environment, fails in another

**Hypothesis**: Environment variables not set, missing config, different behavior

**Log placement**:
```javascript
console.log('[Env] NODE_ENV:', process.env.NODE_ENV)
console.log('[Env] API_URL:', process.env.API_URL)
console.log('[Config] Loaded config:', JSON.stringify(config, null, 2))
```

---

## Callback/Event Loop Issues

**Symptom**: Events firing multiple times, callbacks executing unexpectedly

**Hypothesis**: Duplicate listeners, closure capturing stale state

**Log placement**:
```javascript
// Track listener count
const listenerId = Math.random().toString(36).slice(2)
console.log('[Events] Registering listener:', listenerId)

element.addEventListener('event', (data) => {
  console.log('[Events] Listener fired:', listenerId, 'with data:', data)
})

// For intervals/timeouts
const intervalId = setInterval(() => {
  console.log('[Timer] Interval fired, counter:', counter)
}, 1000)
```
