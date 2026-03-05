# Cloudflare Agents Architecture

## Overview

Cloudflare Agents is a framework for building persistent, stateful AI agents that run on Cloudflare Workers. It enables developers to create autonomous entities that maintain context, reason through problems, schedule work, and take action.

## Core Packages

### agents (Core SDK)
The foundation package providing the `Agent` base class built on Durable Objects. Features include:
- Persistent state management with automatic client sync
- `@callable()` decorator for type-safe RPC
- WebSocket connections with lifecycle hooks
- Scheduling (cron, one-time, recurring tasks)
- MCP server/client support
- Email routing integration

### @cloudflare/ai-chat
Higher-level AI chat abstractions:
- `AIChatAgent` class extending `Agent`
- Message persistence in SQLite
- Resumable streaming (survives disconnects)
- Tool execution (server and client-side)
- React hook: `useAgentChat`

### hono-agents
Middleware for integrating agents into Hono applications with minimal configuration.

### @cloudflare/codemode (Experimental)
Allows LLMs to generate and execute TypeScript code in sandboxed Workers instead of making individual tool calls.

## Communication Patterns

- **WebSocket**: Real-time bidirectional state sync between clients and agents
- **RPC**: Type-safe method calls via `@callable()` decorator
- **MCP**: Model Context Protocol for tool/resource discovery

## External Dependencies

- **Durable Objects**: Cloudflare's persistent execution environment
- **AI SDK**: Vercel's unified interface for LLM providers
- **MCP SDK**: Model Context Protocol for tool integration
