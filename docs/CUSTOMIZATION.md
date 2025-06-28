# Customization Guide

Learn how to customize the Bolt template for your specific needs.

## Theming

### Color Scheme

The template uses CSS custom properties for theming. Customize colors in `app/styles/variables.scss`:

```scss
:root {
  --bolt-elements-textPrimary: #your-color;
  --bolt-elements-bg-depth-1: #your-bg-color;
  // ... more variables
}
```

### Dark/Light Mode

Toggle themes programmatically:

```typescript
import { toggleTheme, themeStore } from '~/lib/stores/theme';

// Toggle theme
toggleTheme();

// Get current theme
const currentTheme = themeStore.get(); // 'light' | 'dark'
```

### Custom Themes

Add new themes by extending the theme system:

```typescript
// app/lib/stores/theme.ts
export type Theme = 'dark' | 'light' | 'custom';

// Add custom theme variables in variables.scss
:root[data-theme='custom'] {
  --bolt-elements-textPrimary: #custom-color;
}
```

## AI Model Configuration

### Switching AI Providers

Currently supports Anthropic Claude. To add other providers:

1. **Install the provider SDK**
   ```bash
   pnpm add @ai-sdk/openai
   ```

2. **Update the model configuration**
   ```typescript
   // app/lib/.server/llm/model.ts
   import { createOpenAI } from '@ai-sdk/openai';
   
   export function getOpenAIModel(apiKey: string) {
     const openai = createOpenAI({ apiKey });
     return openai('gpt-4');
   }
   ```

3. **Update the stream-text function**
   ```typescript
   // app/lib/.server/llm/stream-text.ts
   export function streamText(messages: Messages, env: Env, options?: StreamingOptions) {
     const model = env.OPENAI_API_KEY 
       ? getOpenAIModel(env.OPENAI_API_KEY)
       : getAnthropicModel(env.ANTHROPIC_API_KEY);
     
     return _streamText({
       model,
       // ... other options
     });
   }
   ```

### Custom System Prompts

Modify the AI behavior by updating system prompts:

```typescript
// app/lib/.server/llm/prompts.ts
export const getSystemPrompt = (cwd: string = WORK_DIR) => `
You are a helpful AI assistant specialized in web development.
Your custom instructions here...
`;
```

## UI Customization

### Adding New Components

Create reusable components in `app/components/ui/`:

```typescript
// app/components/ui/CustomButton.tsx
import { memo } from 'react';
import { classNames } from '~/utils/classNames';

interface CustomButtonProps {
  variant?: 'primary' | 'secondary';
  children: React.ReactNode;
  onClick?: () => void;
}

export const CustomButton = memo(({ variant = 'primary', children, onClick }: CustomButtonProps) => {
  return (
    <button
      className={classNames(
        'px-4 py-2 rounded-md transition-colors',
        {
          'bg-blue-500 text-white hover:bg-blue-600': variant === 'primary',
          'bg-gray-200 text-gray-800 hover:bg-gray-300': variant === 'secondary',
        }
      )}
      onClick={onClick}
    >
      {children}
    </button>
  );
});
```

### Modifying the Layout

Update the main layout in `app/routes/_index.tsx`:

```typescript
export default function Index() {
  return (
    <div className="flex flex-col h-full w-full">
      <Header />
      {/* Add your custom sections here */}
      <CustomSection />
      <ClientOnly fallback={<BaseChat />}>
        {() => <Chat />}
      </ClientOnly>
    </div>
  );
}
```

### Custom Icons

Add custom icons using UnoCSS:

1. **Add SVG files to `icons/` directory**

2. **Use in components**
   ```typescript
   <div className="i-custom:your-icon-name text-xl" />
   ```

## Editor Customization

### Adding Language Support

Extend CodeMirror language support:

```typescript
// app/components/editor/codemirror/languages.ts
import { LanguageDescription } from '@codemirror/language';

export const supportedLanguages = [
  // ... existing languages
  LanguageDescription.of({
    name: 'Rust',
    extensions: ['rs'],
    async load() {
      return import('@codemirror/lang-rust').then((module) => module.rust());
    },
  }),
];
```

### Custom Editor Themes

Create custom CodeMirror themes:

```typescript
// app/components/editor/codemirror/custom-theme.ts
import { EditorView } from '@codemirror/view';

export const customTheme = EditorView.theme({
  '&': {
    color: 'white',
    backgroundColor: '#034'
  },
  '.cm-content': {
    padding: '10px 0'
  },
  // ... more styles
});
```

## Workbench Customization

### Adding New Panels

Extend the workbench with custom panels:

```typescript
// app/components/workbench/CustomPanel.tsx
export const CustomPanel = memo(() => {
  return (
    <div className="h-full flex flex-col">
      <PanelHeader>
        <div className="i-ph:custom-icon" />
        Custom Panel
      </PanelHeader>
      <div className="flex-1 p-4">
        {/* Your custom panel content */}
      </div>
    </div>
  );
});
```

### Custom File Actions

Add custom file operations:

```typescript
// app/lib/stores/workbench.ts
export class WorkbenchStore {
  // ... existing methods
  
  async customFileOperation(filePath: string) {
    // Your custom file operation logic
    const file = this.#filesStore.getFile(filePath);
    if (file) {
      // Process file
      await this.#filesStore.saveFile(filePath, processedContent);
    }
  }
}
```

## Deployment Customization

### Environment-Specific Configuration

Create environment-specific configs:

```typescript
// app/config/index.ts
const config = {
  development: {
    apiUrl: 'http://localhost:5173',
    logLevel: 'debug',
  },
  production: {
    apiUrl: 'https://your-domain.com',
    logLevel: 'error',
  },
};

export default config[process.env.NODE_ENV || 'development'];
```

### Custom Build Process

Modify the build process in `vite.config.ts`:

```typescript
export default defineConfig((config) => {
  return {
    // ... existing config
    define: {
      __APP_VERSION__: JSON.stringify(process.env.npm_package_version),
      __BUILD_TIME__: JSON.stringify(new Date().toISOString()),
    },
    plugins: [
      // ... existing plugins
      customPlugin(),
    ],
  };
});
```

## Advanced Customization

### Custom WebContainer Actions

Add new action types for the AI to execute:

```typescript
// app/types/actions.ts
export type ActionType = 'file' | 'shell' | 'custom';

export interface CustomAction extends BaseAction {
  type: 'custom';
  operation: string;
  parameters: Record<string, any>;
}
```

### State Persistence

Customize state persistence:

```typescript
// app/lib/stores/custom-store.ts
import { persistentAtom } from '@nanostores/persistent';

export const customStore = persistentAtom<CustomState>('custom-state', {
  // default state
}, {
  encode: JSON.stringify,
  decode: JSON.parse,
});
```

### Custom Shortcuts

Add keyboard shortcuts:

```typescript
// app/lib/stores/settings.ts
export const shortcutsStore = map<Shortcuts>({
  // ... existing shortcuts
  customAction: {
    key: 'k',
    ctrlOrMetaKey: true,
    action: () => {
      // Your custom action
    },
  },
});
```

This customization guide covers the most common modification points. For more advanced customizations, refer to the source code and component documentation.