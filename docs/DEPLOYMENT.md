# Deployment Guide

This guide covers deploying your Bolt template application to various platforms.

## Cloudflare Pages (Recommended)

Cloudflare Pages is the recommended deployment platform as the template is optimized for it.

### Prerequisites

- Cloudflare account
- Wrangler CLI installed globally: `npm install -g wrangler`
- Repository connected to Cloudflare Pages

### Automatic Deployment

1. **Connect your repository to Cloudflare Pages**
   - Go to [Cloudflare Pages](https://pages.cloudflare.com/)
   - Click "Create a project"
   - Connect your Git repository

2. **Configure build settings**
   ```
   Build command: pnpm run build
   Build output directory: build/client
   Root directory: /
   ```

3. **Set environment variables**
   In your Cloudflare Pages dashboard:
   ```
   ANTHROPIC_API_KEY=your_api_key_here
   NODE_VERSION=20.15.1
   ```

4. **Deploy**
   Push to your main branch to trigger automatic deployment.

### Manual Deployment

```bash
# Build the project
pnpm run build

# Deploy using Wrangler
pnpm run deploy
```

### Custom Domain

1. In Cloudflare Pages dashboard, go to "Custom domains"
2. Add your domain
3. Update DNS records as instructed

## Vercel

While not the primary target, you can deploy to Vercel with some modifications.

### Setup

1. **Install Vercel CLI**
   ```bash
   npm install -g vercel
   ```

2. **Create vercel.json**
   ```json
   {
     "buildCommand": "pnpm run build",
     "outputDirectory": "build/client",
     "framework": "remix",
     "functions": {
       "app/routes/**/*.ts": {
         "runtime": "nodejs18.x"
       }
     }
   }
   ```

3. **Deploy**
   ```bash
   vercel --prod
   ```

### Environment Variables

Set in Vercel dashboard:
```
ANTHROPIC_API_KEY=your_api_key_here
```

## Netlify

Deploy to Netlify with adapter modifications.

### Setup

1. **Install Netlify CLI**
   ```bash
   npm install -g netlify-cli
   ```

2. **Create netlify.toml**
   ```toml
   [build]
     command = "pnpm run build"
     publish = "build/client"
   
   [build.environment]
     NODE_VERSION = "20.15.1"
   
   [[redirects]]
     from = "/*"
     to = "/.netlify/functions/server"
     status = 200
   ```

3. **Deploy**
   ```bash
   netlify deploy --prod
   ```

## Docker Deployment

For containerized deployments.

### Dockerfile

```dockerfile
FROM node:20.15.1-alpine

WORKDIR /app

# Install pnpm
RUN npm install -g pnpm@9.4.0

# Copy package files
COPY package.json pnpm-lock.yaml ./

# Install dependencies
RUN pnpm install --frozen-lockfile

# Copy source code
COPY . .

# Build application
RUN pnpm run build

# Expose port
EXPOSE 3000

# Start application
CMD ["pnpm", "run", "start"]
```

### Docker Compose

```yaml
version: '3.8'
services:
  bolt-app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - ANTHROPIC_API_KEY=${ANTHROPIC_API_KEY}
      - NODE_ENV=production
    restart: unless-stopped
```

### Build and Run

```bash
# Build image
docker build -t bolt-template .

# Run container
docker run -p 3000:3000 -e ANTHROPIC_API_KEY=your_key bolt-template
```

## Self-Hosted

Deploy on your own server.

### Prerequisites

- Node.js 20.15.1+
- pnpm 9.4.0+
- Process manager (PM2 recommended)

### Setup

1. **Install PM2**
   ```bash
   npm install -g pm2
   ```

2. **Create ecosystem file**
   ```javascript
   // ecosystem.config.js
   module.exports = {
     apps: [{
       name: 'bolt-template',
       script: 'npm',
       args: 'run start',
       cwd: '/path/to/your/app',
       env: {
         NODE_ENV: 'production',
         ANTHROPIC_API_KEY: 'your_api_key_here'
       }
     }]
   };
   ```

3. **Deploy**
   ```bash
   # Build application
   pnpm run build
   
   # Start with PM2
   pm2 start ecosystem.config.js
   
   # Save PM2 configuration
   pm2 save
   pm2 startup
   ```

### Nginx Configuration

```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## Environment Variables

### Required Variables

- `ANTHROPIC_API_KEY` - Your Anthropic API key

### Optional Variables

- `VITE_LOG_LEVEL` - Logging level (debug, info, warn, error)
- `VITE_DISABLE_PERSISTENCE` - Disable chat persistence
- `NODE_ENV` - Environment (development, production)

### Security Best Practices

1. **Never commit API keys** to version control
2. **Use environment-specific configs** for different deployments
3. **Rotate API keys** regularly
4. **Use HTTPS** in production
5. **Set up monitoring** and alerting

## Performance Optimization

### Build Optimization

```bash
# Analyze bundle size
pnpm run build --analyze

# Enable compression
# (handled automatically by Cloudflare)
```

### Caching Strategy

- **Static assets**: Long-term caching (1 year)
- **API responses**: Short-term caching (5 minutes)
- **HTML**: No caching for dynamic content

### CDN Configuration

For optimal performance:
- Enable Cloudflare CDN
- Configure cache rules
- Enable Brotli compression
- Use WebP images where possible

## Monitoring

### Health Checks

Add health check endpoint:

```typescript
// app/routes/health.ts
export async function loader() {
  return new Response('OK', { status: 200 });
}
```

### Error Tracking

Integrate error tracking service:

```typescript
// app/entry.client.tsx
import * as Sentry from '@sentry/remix';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
});
```

### Analytics

Add analytics tracking:

```typescript
// app/root.tsx
import { Analytics } from '@vercel/analytics/react';

export default function App() {
  return (
    <>
      <Outlet />
      <Analytics />
    </>
  );
}
```

## Troubleshooting

### Common Issues

1. **Build failures**
   - Check Node.js version compatibility
   - Verify all dependencies are installed
   - Check for TypeScript errors

2. **Runtime errors**
   - Verify environment variables are set
   - Check API key validity
   - Review server logs

3. **Performance issues**
   - Enable compression
   - Optimize bundle size
   - Use CDN for static assets

### Debug Mode

Enable debug logging:

```bash
VITE_LOG_LEVEL=debug pnpm run dev
```

This deployment guide should cover most common deployment scenarios. Choose the platform that best fits your needs and infrastructure requirements.