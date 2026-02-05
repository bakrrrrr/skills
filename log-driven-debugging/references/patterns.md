# Debugging Patterns

## API/Network Issues

**Symptom**: Data not arriving, wrong data, request failures

**Log placement**:

```javascript
console.log("[API] Calling:", url, { params: queryParams });

try {
  const response = await fetch(url, options);
  console.log("[API] Response status:", response.status);
  const data = await response.json();
  console.log("[API] Response data:", JSON.stringify(data, null, 2));
  return data;
} catch (error) {
  console.error("[API] Error:", error.message);
  throw error;
}
```

---

## State Mutation Issues

**Symptom**: Value changes unexpectedly, UI shows wrong data

**Log placement**:

```javascript
// Before mutation
console.log("[State] Before update:", JSON.stringify(currentState));

// After mutation
Object.assign(currentState, updates);
console.log("[State] After update:", JSON.stringify(currentState));
```

---

## Async/Timing Issues

**Symptom**: Race conditions, promises resolve in wrong order, undefined values

**Log placement**:

```javascript
console.log("[Async] Starting operation:", operationId);

async function process() {
  console.log("[Async] Step A starting");
  await stepA();
  console.log("[Async] Step A complete, result:", resultA);
}
```

---

## Type/TypeScript Issues

**Symptom**: Property undefined, method not found, runtime errors

**Log placement**:

```javascript
console.log("[Types] user:", typeof user, user);
console.log("[Types] user.id:", typeof user?.id, user?.id);
console.log("[Types] Array.isArray(items):", Array.isArray(items));
```

---

## Function Not Being Called

**Symptom**: Expected function doesn't execute, handler not triggered

**Log placement**:

```javascript
button.addEventListener("click", (e) => {
  console.log("[Event] Click detected on:", e.target);
  handleClick();
});

if (shouldExecute) {
  console.log("[Logic] Condition met, executing");
  doSomething();
} else {
  console.log("[Logic] Condition false, shouldExecute:", shouldExecute);
}
```

---

## Environment/Configuration Issues

**Symptom**: Works in one environment, fails in another

**Log placement**:

```javascript
console.log("[Env] NODE_ENV:", process.env.NODE_ENV);
console.log("[Env] API_URL:", process.env.API_URL);
console.log("[Config] Loaded:", JSON.stringify(config, null, 2));
```

---

## React Component Issues

**Symptom**: Component not re-rendering, stale state, infinite loops

**Log placement**:

```javascript
// Track renders
function MyComponent({ userId }) {
  console.log("[MyComponent] Render, userId:", userId);

  const [data, setData] = useState(null);

  useEffect(() => {
    console.log("[MyComponent] useEffect triggered, userId:", userId);
    fetchData(userId).then((result) => {
      console.log("[MyComponent] Setting data:", result);
      setData(result);
    });
    return () => console.log("[MyComponent] Cleanup for userId:", userId);
  }, [userId]);

  console.log("[MyComponent] Current state - data:", data);
}
```

**Common causes**:

- Missing dependency in useEffect array
- Object/array reference changing on every render
- State update in render body (causes infinite loop)

---

## Callback/Event Loop Issues

**Symptom**: Events firing multiple times, callbacks executing unexpectedly

**Log placement**:

```javascript
// Track listener registration
const listenerId = Math.random().toString(36).slice(2, 8);
console.log("[Events] Registering listener:", listenerId);

element.addEventListener("event", (data) => {
  console.log("[Events] Listener fired:", listenerId, "data:", data);
});

// Track intervals
const intervalId = setInterval(() => {
  console.log("[Timer] Interval fired, counter:", counter);
}, 1000);
```
