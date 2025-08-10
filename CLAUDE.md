# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

### Development

- `pnpm run dev` - Run the server in development mode with hot reload (using tsx)
- `pnpm run build` - Compile TypeScript to JavaScript in build/
- `pnpm run start` - Run the compiled server (node build/index.js)
- `pnpm run clean` - Remove build/ directory

### Testing

- Direct testing: Set `NOTY_API_TOKEN` environment variable and run `pnpm run dev`

## Architecture

### MCP Server Implementation

This is a Model Context Protocol (MCP) server that bridges AI tools (Claude Desktop, ChatGPT) with the Noty Stream API. The server exposes note-taking capabilities through standardized MCP tools.

### Core Components

1. **API Layer** (`src/api/`)
   - `client.ts` - Axios-based HTTP client with auth, debugging, and error handling
   - `notes.ts`, `groups.ts`, `todos.ts`, `workspaces.ts` - API endpoint wrappers

2. **Tools Layer** (`src/tools/`)
   - Each tool class (NotesTools, GroupsTools, etc.) maps API operations to MCP tool definitions
   - Tools return structured responses that AI models can understand
   - Error handling converts API errors to user-friendly messages

3. **Main Server** (`src/index.ts`)
   - Initializes MCP server using stdio transport
   - Registers all tools from tool classes
   - Routes tool calls to appropriate handlers
   - Manages tool responses and error formatting

### Key Design Patterns

- **Modular Tool Organization**: Each API domain (notes, groups, todos, workspaces) has its own tool class
- **Consistent Error Handling**: All API errors are caught and transformed to structured MCP responses
- **Environment-based Configuration**: Uses dotenv for API tokens and configuration
- **TypeScript Strict Mode**: Full type safety with strict TypeScript configuration

## Environment Configuration

Required environment variables:

- `NOTY_API_TOKEN` - JWT authentication token (required)
- `NOTY_API_URL` - API base URL (default: https://noty.stream/api)
- `DEBUG` - Enable debug logging (optional)

## Tool Categories

The server provides 40+ MCP tools organized into:

- **Notes Management**: CRUD operations, search, append content
- **Groups Management**: Hierarchical organization, tree operations
- **Todos Management**: Task tracking, completion status, statistics
- **Workspace Management**: Multi-workspace support, switching contexts

See TOOLS.md for complete tool reference.

## Distribution

To use this MCP server in AI tools:

1. Build the server: `pnpm run build`
2. Configure in your AI tool (Claude Desktop, etc.) to run: `node /path/to/build/index.js`

### Requirements
- Node.js 18+ installed on target system
- All npm dependencies are bundled in the compiled output
