# Agents Core Observability

## Overview

The agents core package provides built-in observability through Node.js `diagnostics_channel` API. Events are published to named channels, allowing zero-overhead monitoring when no subscribers are attached.

## Diagnostics Channels

### agents:state
Emitted when agent state changes. Events include:
- `state:update` - State was modified via `setState()`
- `state:sync` - State synced to connected clients

### agents:rpc
Emitted for `@callable()` method invocations:
- `rpc` - Method call started
- `rpc:success` - Method completed successfully
- `rpc:error` - Method threw an error

### agents:message
WebSocket message and tool events:
- `message:receive` - Incoming WebSocket message
- `message:send` - Outgoing WebSocket message
- `tool:call` - Tool invocation started
- `tool:result` - Tool returned result

### agents:schedule
Scheduling and queue events:
- `schedule:alarm` - Alarm triggered
- `schedule:cron` - Cron job executed
- `queue:enqueue` - Task added to background queue
- `queue:process` - Queue task processed

### agents:lifecycle
Connection lifecycle events:
- `connect` - Client connected
- `disconnect` - Client disconnected
- `destroy` - Agent instance destroyed

### agents:workflow
Durable workflow events:
- `workflow:start` - Workflow execution started
- `workflow:step` - Workflow step completed
- `workflow:complete` - Workflow finished
- `workflow:error` - Workflow failed

### agents:mcp
Model Context Protocol events:
- `mcp:tool:call` - MCP tool invocation
- `mcp:resource:read` - MCP resource accessed
- `mcp:connect` - MCP client connected
- `mcp:disconnect` - MCP client disconnected

### agents:email
Email routing events:
- `email:receive` - Email received
- `email:forward` - Email forwarded

### agents:workspace (Experimental)
Virtual filesystem events:
- `workspace:read` - File read
- `workspace:write` - File written
- `workspace:delete` - File deleted

## Usage

Subscribe to channels for monitoring:

```typescript
import { subscribe } from "agents/observability";

const unsub = subscribe("rpc", (event) => {
  console.log(event.payload.method);
});
```

In production, events are automatically forwarded to Tail Workers via `event.diagnosticsChannelEvents`.
