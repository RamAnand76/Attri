# Getting Started with Bolt Template

This guide will help you get up and running with the Bolt template for building AI-powered development tools.

## Prerequisites

Before you begin, ensure you have:

- **Node.js** (v20.15.1 or higher)
- **pnpm** (v9.4.0 or higher) 
- **Anthropic API Key** - Get one from [Anthropic Console](https://console.anthropic.com/)

## Installation

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

4. **Add your API key**
   Edit `.env.local` and add your Anthropic API key:
   ```
   ANTHROPIC_API_KEY=your_actual_api_key_here
   ```

## First Run

Start the development server:

```bash
pnpm run dev
```

Open your browser to `http://localhost:5173` and you should see the Bolt interface.

## Understanding the Interface

### Chat Panel (Left)
- Send prompts to the AI
- View conversation history
- Use the enhance prompt feature

### Workbench (Right)
- **Code View**: File editor with syntax highlighting
- **Preview View**: Live preview of running applications
- **Terminal**: Full shell access for running commands

### Key Features to Try

1. **Create a simple app**
   ```
   Create a simple React todo app with Tailwind CSS
   ```

2. **Modify existing code**
   ```
   Add a dark mode toggle to the app
   ```

3. **Install packages**
   ```
   Add React Router for navigation
   ```

## Next Steps

- Read the [Architecture Guide](./ARCHITECTURE.md)
- Check out [Customization Options](./CUSTOMIZATION.md)
- Learn about [Deployment](./DEPLOYMENT.md)