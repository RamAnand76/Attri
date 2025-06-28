# Template Setup Instructions

This document provides step-by-step instructions for setting up the Bolt template for your own project.

## 🚀 Quick Setup

### 1. Use This Template

Click the "Use this template" button on GitHub or clone the repository:

```bash
git clone https://github.com/your-username/bolt-template.git my-ai-dev-tool
cd my-ai-dev-tool
```

### 2. Update Project Information

**Update package.json:**
```json
{
  "name": "my-ai-dev-tool",
  "description": "My custom AI-powered development tool",
  "version": "1.0.0",
  "repository": {
    "type": "git",
    "url": "https://github.com/your-username/my-ai-dev-tool.git"
  }
}
```

**Update README.md:**
- Replace "Bolt Template" with your project name
- Update repository URLs
- Customize the description and features

### 3. Environment Setup

```bash
# Install dependencies
pnpm install

# Copy environment template
cp .env.example .env.local

# Add your API key
echo "ANTHROPIC_API_KEY=your_api_key_here" >> .env.local
```

### 4. Customize Branding

**Update app title and metadata:**
```typescript
// app/routes/_index.tsx
export const meta: MetaFunction = () => {
  return [
    { title: 'My AI Dev Tool' },
    { name: 'description', content: 'Your custom description here' }
  ];
};
```

**Update header branding:**
```typescript
// app/components/header/Header.tsx
<a href="/" className="text-2xl font-semibold text-accent flex items-center">
  Your App Name
</a>
```

### 5. Test the Setup

```bash
# Start development server
pnpm run dev

# Open browser to http://localhost:5173
# Test with a simple prompt like "Create a hello world React app"
```

## 🎨 Customization Options

### Theming

**Colors and Styling:**
- Edit `app/styles/variables.scss` for color schemes
- Modify `uno.config.ts` for design tokens
- Update `app/lib/stores/theme.ts` for theme logic

**Logo and Icons:**
- Replace files in `icons/` directory
- Update `public/favicon.svg` and `public/logo.svg`
- Modify icon references in components

### AI Configuration

**System Prompts:**
```typescript
// app/lib/.server/llm/prompts.ts
export const getSystemPrompt = (cwd: string = WORK_DIR) => `
You are [Your AI Assistant Name], specialized in [your domain].
[Your custom instructions here...]
`;
```

**Model Settings:**
```typescript
// app/lib/.server/llm/constants.ts
export const MAX_TOKENS = 8192; // Adjust as needed
export const MAX_RESPONSE_SEGMENTS = 2; // Adjust as needed
```

### UI Customization

**Welcome Message:**
```typescript
// app/components/chat/BaseChat.tsx
<h1 className="text-5xl text-center font-bold text-bolt-elements-textPrimary mb-2">
  Your Custom Welcome Message
</h1>
<p className="mb-4 text-center text-bolt-elements-textSecondary">
  Your custom subtitle here
</p>
```

**Example Prompts:**
```typescript
// app/components/chat/BaseChat.tsx
const EXAMPLE_PROMPTS = [
  { text: 'Your custom example 1' },
  { text: 'Your custom example 2' },
  { text: 'Your custom example 3' },
];
```

## 🔧 Advanced Configuration

### Adding New Features

**Custom Components:**
1. Create components in `app/components/`
2. Add to the appropriate section (chat, workbench, ui)
3. Export from index files for easy imports

**New API Endpoints:**
1. Add routes in `app/routes/api.*.ts`
2. Follow the existing pattern for streaming responses
3. Update types in `types/` directory

**State Management:**
1. Create new stores in `app/lib/stores/`
2. Use nanostores for reactive state
3. Add persistence if needed

### WebContainer Customization

**File System Operations:**
```typescript
// app/lib/stores/files.ts
// Customize file handling, filtering, and operations
```

**Terminal Configuration:**
```typescript
// app/components/workbench/terminal/Terminal.tsx
// Customize terminal appearance and behavior
```

### Deployment Configuration

**Cloudflare Pages:**
```toml
# wrangler.toml
name = "my-ai-dev-tool"
compatibility_flags = ["nodejs_compat"]
compatibility_date = "2024-07-01"
pages_build_output_dir = "./build/client"
```

**Environment Variables:**
- Set `ANTHROPIC_API_KEY` in your deployment platform
- Add any custom environment variables
- Update `.env.example` with new variables

## 📦 Distribution

### Creating a Template Repository

1. **Clean up the repository:**
   ```bash
   # Remove template-specific files
   rm TEMPLATE_SETUP.md
   
   # Update README.md with your project info
   # Update package.json with your details
   ```

2. **Create template repository on GitHub:**
   - Enable "Template repository" in settings
   - Add appropriate topics and description
   - Create comprehensive README

3. **Documentation:**
   - Keep the `docs/` directory updated
   - Add project-specific documentation
   - Include contribution guidelines

### Publishing to npm (Optional)

If creating a CLI tool or package:

```bash
# Update package.json for publishing
{
  "name": "@your-org/ai-dev-tool",
  "version": "1.0.0",
  "bin": {
    "my-tool": "./bin/cli.js"
  }
}

# Publish
npm publish
```

## 🧪 Testing Your Template

### Manual Testing Checklist

- [ ] Chat interface loads correctly
- [ ] AI responses stream properly
- [ ] Code editor syntax highlighting works
- [ ] File operations (create, edit, save) work
- [ ] Terminal commands execute
- [ ] Preview updates in real-time
- [ ] Theme switching works
- [ ] Mobile responsiveness

### Automated Testing

```bash
# Run test suite
pnpm run test

# Type checking
pnpm run typecheck

# Linting
pnpm run lint
```

### Performance Testing

```bash
# Build and analyze
pnpm run build

# Test production build
pnpm run preview
```

## 🆘 Troubleshooting

### Common Issues

**API Key Issues:**
- Verify API key is correctly set in `.env.local`
- Check API key has sufficient credits
- Ensure no extra spaces or quotes

**Build Issues:**
- Clear node_modules and reinstall: `rm -rf node_modules && pnpm install`
- Check Node.js version: `node --version` (should be 20.15.1+)
- Verify TypeScript compilation: `pnpm run typecheck`

**Runtime Issues:**
- Check browser console for errors
- Verify WebContainer API is loading
- Test with simple prompts first

### Getting Help

- Check the [documentation](./docs/)
- Review [GitHub issues](https://github.com/stackblitz/bolt.new/issues)
- Join the [StackBlitz Discord](https://discord.gg/stackblitz)

## 🎯 Next Steps

After setup:

1. **Customize the AI behavior** for your specific use case
2. **Add domain-specific features** and components
3. **Create comprehensive documentation** for your users
4. **Set up CI/CD** for automated testing and deployment
5. **Gather user feedback** and iterate on the experience

Your AI-powered development tool is now ready to use! 🚀