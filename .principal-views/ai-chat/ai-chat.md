# AI Chat Package

## Overview

The `@cloudflare/ai-chat` package provides a higher-level abstraction for building AI-powered chat agents. It extends the core `Agent` class with chat-specific features like message persistence, resumable streaming, and tool execution.

## Key Features

### AIChatAgent Class
The main class that extends `Agent` from the core package. Provides:
- Automatic message persistence to SQLite
- WebSocket-based chat protocol
- Tool execution (server and client-side)
- Human-in-the-loop approval workflows

### Message Persistence
Messages are stored in SQLite via Durable Objects:
- Full conversation history preserved
- Supports message compaction for large outputs
- Row size limits handled automatically

### Resumable Streaming
Streaming responses survive client disconnects:
- Stream state preserved in memory
- Clients can reconnect and resume
- No message loss during network issues

### Client Tools
Dynamic tool registration from the client:
- Tools defined at runtime
- Server doesn't need to know tool surface ahead of time
- Useful for SDK/platform use cases

### WS Chat Transport
WebSocket transport layer:
- Bidirectional streaming
- Message framing and protocol handling
- Integration with AI SDK streaming

### V5 Migration
Automatic message format transformation:
- Handles legacy AI SDK v5 message formats
- Transparent conversion to current format

## React Integration

The `useAgentChat` hook provides:
- Message state management
- Streaming response handling
- Tool call execution on client
- Connection state management

## Experimental: Forever

Persistent conversation tracking for long-running agent interactions.
