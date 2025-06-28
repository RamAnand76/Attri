# Bolt.new Template

A template for building AI-powered full-stack web development tools in the browser, based on the open-source Bolt.new codebase.

## 🚀 Features

- **AI-Powered Development**: Integrated with Claude Sonnet 3.5 for intelligent code generation
- **In-Browser Development**: Full development environment powered by WebContainer API
- **Real-time Preview**: Live preview of applications as they're being built
- **File Management**: Complete file system with editor, terminal, and file tree
- **Modern Stack**: Built with Remix, React, TypeScript, and Tailwind CSS

## 🛠 Tech Stack

- **Frontend**: React 18, Remix, TypeScript
- **Styling**: Tailwind CSS, UnoCSS, SCSS
- **AI Integration**: Anthropic Claude API, AI SDK
- **Development Environment**: WebContainer API
- **Editor**: CodeMirror 6
- **Terminal**: xterm.js
- **Deployment**: Cloudflare Pages/Workers

## 📋 Prerequisites

- Node.js (v20.15.1 or higher)
- pnpm (v9.4.0 or higher)
- Anthropic API key

## 🚀 Quick Start

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd bolt-template
   ```

2. **Install dependencies**
   ```bash
   pnpm install
   ```

3. **Set up environment variables**
   ```bash
   cp .env.example .env.local
   ```
   
   Add your Anthropic API key to `.env.local`:
   ```
   ANTHROPIC_API_KEY=your_api_key_here
   VITE_LOG_LEVEL=debug
   ```

4. **Start development server**
   ```bash
   pnpm run dev
   ```

5. **Open your browser**
   Navigate to `http://localhost:5173` and start building!

## 📁 Project Structure

```
├── app/                          # Main application code
│   ├── components/              # React components
│   │   ├── chat/               # Chat interface components
│   │   ├── editor/             # Code editor components
│   │   ├── workbench/          # Development environment
│   │   └── ui/                 # Reusable UI components
│   ├── lib/                    # Core libraries and utilities
│   │   ├── .server/            # Server-side code
│   │   ├── hooks/              # React hooks
│   │   ├── stores/             # State management
│   │   └── runtime/            # Runtime utilities
│   ├── routes/                 # Remix routes
│   ├── styles/                 # Global styles and themes
│   └── utils/                  # Utility functions
├── functions/                   # Cloudflare Functions
├── public/                     # Static assets
└── types/                      # TypeScript type definitions
```

## 🔧 Configuration

### Environment Variables

- `ANTHROPIC_API_KEY`: Your Anthropic API key for Claude integration
- `VITE_LOG_LEVEL`: Logging level (debug, info, warn, error)
- `VITE_DISABLE_PERSISTENCE`: Disable chat history persistence

### WebContainer API

This template uses the WebContainer API for in-browser development environments. The API is free for personal and open-source usage. For commercial usage, check [WebContainer API pricing](https://stackblitz.com/pricing#webcontainer-api).

## 🎨 Customization

### Themes

The template includes a comprehensive theming system with light and dark modes. Customize themes in:
- `app/styles/variables.scss` - CSS custom properties
- `app/lib/stores/theme.ts` - Theme state management

### AI Models

Currently configured for Anthropic's Claude, but can be extended to support other models:
- `app/lib/.server/llm/` - AI model integration
- `app/routes/api.chat.ts` - Chat API endpoint

### UI Components

All UI components are built with:
- Tailwind CSS for styling
- Framer Motion for animations
- Radix UI for accessible primitives

## 📚 Key Features Explained

### Chat Interface
- Real-time streaming responses
- Message history with persistence
- Prompt enhancement
- File modification tracking

### Code Editor
- Syntax highlighting with CodeMirror 6
- Multiple language support
- File tree navigation
- Real-time collaboration

### Development Environment
- Terminal with full shell access
- Package manager integration
- Live preview with hot reload
- File system operations

### AI Integration
- Streaming text generation
- Code parsing and execution
- File modification detection
- Context-aware responses

## 🚀 Deployment

### Cloudflare Pages

1. **Build the project**
   ```bash
   pnpm run build
   ```

2. **Deploy to Cloudflare Pages**
   ```bash
   pnpm run deploy
   ```

### Environment Variables for Production

Set these in your Cloudflare Pages dashboard:
- `ANTHROPIC_API_KEY`
- Any other environment variables from `.env.local`

## 🧪 Development

### Available Scripts

- `pnpm run dev` - Start development server
- `pnpm run build` - Build for production
- `pnpm run start` - Start production server locally
- `pnpm run test` - Run tests
- `pnpm run typecheck` - Run TypeScript checks
- `pnpm run lint` - Run ESLint

### Testing

```bash
# Run all tests
pnpm run test

# Run tests in watch mode
pnpm run test:watch

# Run type checking
pnpm run typecheck
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

MIT License - see [LICENSE](LICENSE) for details.

## 🙏 Acknowledgments

- [StackBlitz](https://stackblitz.com) for WebContainer API
- [Anthropic](https://anthropic.com) for Claude AI
- [Remix](https://remix.run) for the web framework
- [Cloudflare](https://cloudflare.com) for hosting platform

## 🆘 Support

- [Documentation](./docs/)
- [Issues](https://github.com/your-username/bolt-template/issues)
- [Discussions](https://github.com/your-username/bolt-template/discussions)

---

Built with ❤️ using the Bolt.new open-source codebase