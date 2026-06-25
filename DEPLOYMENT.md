# Vercel Deployment Guide

This portfolio is now fully configured for Vercel deployment as a modern Vite + React application.

## Project Structure

```
portfolio/
├── client/                  # React application source
│   ├── pages/             # Page components
│   │   ├── Index.tsx      # Main portfolio page
│   │   └── NotFound.tsx   # 404 error page
│   ├── components/        # Reusable UI components
│   ├── lib/              # Utility functions
│   ├── hooks/            # Custom React hooks
│   ├── App.tsx           # Main App component with routing
│   ├── global.css        # Global styles (Tailwind imports)
│   └── vite-env.d.ts     # Vite type definitions
├── shared/               # Shared utilities (if needed)
├── index.html            # HTML entry point
├── vite.config.ts        # Vite configuration
├── tsconfig.json         # TypeScript configuration
├── package.json          # Project dependencies
├── vercel.json           # Vercel deployment config
└── .gitignore           # Git ignore rules

Note: The old server/ directory is not needed for Vercel deployment
```

## What Changed

### Removed
- **Express server** (`server/` directory and related config)
- **Custom Node.js build** (`vite.config.server.ts`, `server/node-build.ts`)
- **Netlify configuration** (migrated to Vercel)
- Build output redirected from `dist/spa` → `dist`

### Updated
- **package.json**: Removed `express`, `dotenv`, `serverless-http` dependencies
- **vite.config.ts**: Simplified to standard Vite configuration
- **tsconfig.json**: Excluded `server/` from type checking
- **index.html**: Updated entry point script path
- **client/App.tsx**: Standard React 18 entry point

### Added
- **vercel.json**: Vercel deployment configuration
- **DEPLOYMENT.md**: This guide

## Local Development

### Prerequisites
- Node.js 18+ (recommended 22+)
- pnpm 10.14.0+ (or npm/yarn)

### Install Dependencies
```bash
npm install
# or
pnpm install
```

### Start Dev Server
```bash
npm run dev
```
This starts Vite dev server at `http://localhost:5173/` (or next available port)

### Build for Production
```bash
npm run build
```
Output is generated in `dist/` folder ready for deployment

### Preview Production Build
```bash
npm run build
npm run preview
```

## Vercel Deployment

### Quick Deploy (Recommended)

#### Option 1: Via Vercel CLI
```bash
npm i -g vercel
vercel
```
Follow the prompts to connect your repo and deploy.

#### Option 2: Via GitHub Integration
1. Push code to GitHub
2. Go to [vercel.com](https://vercel.com)
3. Click "New Project"
4. Select your GitHub repository
5. Click "Deploy"

Vercel automatically detects `vercel.json` and uses:
- Build Command: `npm run build`
- Output Directory: `dist`
- Framework: Vite

### Environment Variables (if needed)
Add to `.env` file locally or in Vercel project settings:
```
VITE_API_URL=https://your-api.com
```

Access in code:
```javascript
const apiUrl = import.meta.env.VITE_API_URL
```

## Routing

This is a **Single Page Application (SPA)** using React Router. All routes are handled by the React app in the browser.

Routes configured in `client/App.tsx`:
- `/` → Portfolio homepage (Index page)
- `/*` → 404 Not Found

**Important**: Vercel automatically configures SPA routing for Vite projects, so no additional configuration is needed.

## Build Output

The build process generates:
```
dist/
├── index.html              # Main HTML file
├── assets/
│   ├── vendor-*.js         # React, React Router, dependencies
│   ├── index-*.js          # Application code
│   └── index-*.css         # Tailwind CSS output
├── favicon.ico             # Favicon
├── robots.txt              # SEO robots.txt
└── placeholder.svg         # Placeholder image
```

All asset paths in the built HTML are absolute (with `/`) and automatically versioned by Vite for cache busting.

## Performance

- **Bundle Size**: ~750KB (uncompressed), ~223KB (gzip)
- **First Paint**: <1s typical
- **Build Time**: <10s
- **Asset Caching**: Vite adds content hashes to filenames for long-term caching

## Troubleshooting

### Port Already in Use
If port 5173 is in use:
```bash
npm run dev
```
Vite automatically tries the next available port (5174, 5175, etc.)

### Build Fails
1. Clear cache: `rm -rf node_modules dist && npm install`
2. Check Node version: `node --version` (should be 18+)
3. Verify syntax: `npm run typecheck`

### Routes Not Working
- Verify all components are exported from `client/pages/`
- Check React Router paths in `client/App.tsx`
- Ensure no circular imports

### Images Not Loading
- All images use absolute URLs (from Builder.io API or external CDN)
- Local images should be placed in `public/` and referenced as `/image-name.jpg`
- For development, images are served from Vite dev server
- In production, they're served from Vercel CDN

## Assets and Images

Currently using:
- **Profile Image**: Builder.io API URL
- **Project Images**: Builder.io API URLs  
- **Achievement Images**: Builder.io API URLs
- **Fonts**: Google Fonts (imported in `client/global.css`)
  - Poppins (primary)
  - Space Grotesk (secondary)

To use local images:
1. Add images to `public/` folder
2. Reference in code: `<img src="/image-name.jpg" alt="..." />`
3. They'll be served from Vercel's edge network

## Next Steps

1. ✅ `npm install` - Install dependencies
2. ✅ `npm run dev` - Test locally
3. ✅ `npm run build` - Verify build
4. ✅ Push to GitHub - Connect to Vercel
5. ✅ Vercel deploys automatically on push

## Support

For Vercel docs: https://vercel.com/docs
For Vite docs: https://vitejs.dev
For React docs: https://react.dev
