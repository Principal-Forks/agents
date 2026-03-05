# RPC Request Handling

## Overview

RPC (Remote Procedure Call) enables type-safe method invocation from clients to agents over WebSocket connections. Methods decorated with `@callable()` are exposed for remote invocation.

## How It Works

1. **Client sends RPC request** via WebSocket with method name and arguments
2. **Agent validates the request**:
   - Method exists on the agent class
   - Method is decorated with `@callable()`
3. **Method executes**:
   - **Regular methods**: Execute and return result
   - **Streaming methods**: Pass `StreamingResponse` object for incremental results
4. **Response sent** back to client with result or error

## Event Flow

### Success Path
```
rpc:started → rpc:validated → rpc:executing → rpc (completed)
```

| Event | Description |
|-------|-------------|
| `rpc:started` | Request received and parsed |
| `rpc:validated` | Method exists and is callable |
| `rpc:executing` | Method invocation begins |
| `rpc` | Method completed successfully |

### Error Paths
```
rpc:started → rpc:error (validation failed)
rpc:started → rpc:validated → rpc:executing → rpc:error (execution failed)
```

| Error Type | Stage | Description |
|------------|-------|-------------|
| `method_not_found` | validation | Method does not exist on agent |
| `not_callable` | validation | Method exists but lacks `@callable()` decorator |
| `execution_error` | execution | Method threw an exception |

## Error Scenarios

- **Method does not exist**: Unknown method name
- **Method not callable**: Method exists but not decorated with `@callable()`
- **Execution throws**: Method threw an exception during execution
- **Streaming error**: Streaming method failed before stream closed

## Usage Example

```typescript
import { Agent, callable } from "agents";

class MyAgent extends Agent {
  @callable()
  async greet(name: string): Promise<string> {
    return `Hello, ${name}!`;
  }

  @callable({ streaming: true })
  async streamData(stream: StreamingResponse, count: number): Promise<void> {
    for (let i = 0; i < count; i++) {
      stream.send({ index: i });
    }
    stream.done();
  }
}
```

## Client Integration

```typescript
import { useAgent } from "agents/react";

function MyComponent() {
  const agent = useAgent({ agent: "my-agent", name: "instance-1" });

  const handleClick = async () => {
    const result = await agent.call("greet", "World");
    console.log(result); // "Hello, World!"
  };
}
```
