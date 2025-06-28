# Architecture Overview

This document explains the architecture of the Bolt template and how its components work together.

## High-Level Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Chat UI       │    │   Workbench     │    │   AI Service    │
│                 │    │                 │    │                 │
│ - Message List  │    │ - Code Editor   │    │ - Claude API    │
│ - Input Field   │    │ - File Tree     │    │ - Streaming     │
│ - History       │    │ - Terminal      │    │ - Parsing       │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌─────────────────┐
                    │  WebContainer   │
                    │                 │
                    │ - File System   │
                    │ - Node.js       │
                    │ - Package Mgr   │
                    │ - Dev Server    │
                    └─────────────────┘
```

## Core Components

### 1. Chat System (`app/components/chat/`)

**Purpose**: Handles AI conversation and message streaming

**Key Files**:
- `Chat.client.tsx` - Main chat component with hooks
- `BaseChat.tsx` - UI layout and structure
- `Messages.client.tsx` - Message rendering
- `Markdown.tsx` - Message content parsing

**Data Flow**:
1. User sends message via `SendButton`
2. `useChat` hook streams response from `/api/chat`
3. `useMessageParser` extracts artifacts and actions
4. Messages update in real-time

### 2. Workbench (`app/components/workbench/`)

**Purpose**: Development environment with editor, preview, and terminal

**Key Files**:
- `Workbench.client.tsx` - Main workbench container
- `EditorPanel.tsx` - Code editor and file management
- `Preview.tsx` - Live application preview
- `Terminal.tsx` - Shell interface

**Features**:
- Multi-file editing with CodeMirror
- Real-time preview with hot reload
- Terminal with full shell access
- File tree navigation

### 3. State Management (`app/lib/stores/`)

**Purpose**: Centralized state using Nanostores

**Key Stores**:
- `workbenchStore` - Files, editor, terminal state
- `chatStore` - Chat history and UI state
- `themeStore` - Theme preferences
- `filesStore` - File system operations

**Benefits**:
- Reactive updates across components
- Persistent state with hot reload
- Type-safe state management

### 4. AI Integration (`app/lib/.server/llm/`)

**Purpose**: AI model integration and response processing

**Key Files**:
- `stream-text.ts` - Streaming text generation
- `model.ts` - AI model configuration
- `prompts.ts` - System prompts and instructions

**Features**:
- Streaming responses for real-time updates
- Artifact parsing for code execution
- Context-aware conversations

### 5. Runtime System (`app/lib/runtime/`)

**Purpose**: Code execution and file operations

**Key Files**:
- `action-runner.ts` - Executes AI-generated actions
- `message-parser.ts` - Parses artifacts from AI responses

**Workflow**:
1. AI generates artifacts with actions
2. Parser extracts file and shell actions
3. Action runner executes in WebContainer
4. Results update in real-time

## Data Flow

### Message Processing
```
User Input → AI API → Streaming Response → Message Parser → Action Runner → WebContainer
```

### File Operations
```
Editor Changes → Files Store → WebContainer FS → Preview Update
```

### State Updates
```
Store Changes → React Components → UI Updates
```

## WebContainer Integration

The template uses WebContainer API for:

- **File System**: Read/write operations
- **Package Manager**: npm/pnpm operations  
- **Terminal**: Shell command execution
- **Dev Server**: Running applications
- **Preview**: Live application updates

## Security Considerations

- API keys stored securely in environment variables
- WebContainer provides sandboxed execution
- File operations restricted to project directory
- No direct server access from client code

## Performance Optimizations

- **Code Splitting**: Lazy loading of components
- **Streaming**: Real-time AI responses
- **Debouncing**: File change operations
- **Memoization**: Expensive computations cached
- **Virtual Scrolling**: Large file lists

## Extension Points

The architecture supports extending:

- **AI Models**: Add new providers in `llm/`
- **File Types**: Extend editor language support
- **Actions**: Add new action types in runtime
- **UI Components**: Modular component system
- **Themes**: CSS custom properties system