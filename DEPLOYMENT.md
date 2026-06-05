# Roomify Deployment Guide

## Overview
Roomify is a React Router v7 application with Server-Side Rendering (SSR) support. This guide covers deployment to Vercel and other hosting platforms.

## Prerequisites
- Node.js 20.x or higher
- npm or yarn
- Vercel CLI (optional, for local testing)
- Git repository

## Production Build

### Local Build Verification
Before deploying, verify the build works locally:

```bash
# Install dependencies
npm ci

# Run type checking
npm run typecheck

# Build the application
npm run build

# Test production build locally
npm run start
```

## Vercel Deployment

### Setup
1. **Connect Repository**
   - Go to [vercel.com](https://vercel.com)
   - Import your GitHub/GitLab repository
   - Vercel will auto-detect the project as React Router

2. **Environment Variables**
   Set these in your Vercel dashboard:
   - `VITE_PUTER_WORKER_URL`: Your Puter worker endpoint
     - Example: `https://your-project.puter.work`

3. **Build Settings**
   - Build Command: `npm run build`
   - Install Command: `npm ci`
   - Output Directory: (auto-detected)

### Deploy
```bash
# Using Vercel CLI
vercel

# Or push to your connected repository
git push
```

## Environment Configuration

### Required Variables
```env
# .env or .env.production
VITE_PUTER_WORKER_URL=https://your-puter-worker-url.puter.work
```

### Optional Variables
```env
# Add other environment variables as needed
# DATABASE_URL=your-database-url
# API_KEY=your-api-key
```

## Troubleshooting

### Build Failures
**"Permission denied" or "EACCES" errors**
- ✅ Fixed: Removed `node_modules` from git tracking
- Ensure `.gitignore` is properly configured

**TypeScript Compilation Errors**
- Run `npm run typecheck` to identify errors
- Check [visualizer.$id.tsx] for type mismatches
- Ensure all imports are properly typed

**Build Stops During Deployment**
- Check Vercel build logs for detailed error messages
- Verify all environment variables are set
- Ensure `package.json` scripts are correct

### Runtime Issues

**Blank Page or 500 Errors**
- Check Vercel function logs in dashboard
- Verify `VITE_PUTER_WORKER_URL` environment variable
- Ensure Puter API is accessible from Vercel region

**Image Loading Issues**
- Verify Puter hosting URLs are publicly accessible
- Check CORS headers on Puter service
- Ensure image URLs are absolute (not relative)

## Performance Optimization

### Build Artifacts
- Client assets: `build/client/` (optimized for browser)
- Server bundle: `build/server/index.js` (39.39 KB)
- CSS: Automatically optimized and injected

### Monitoring
- **Build Time**: Target ~30-40 seconds
- **Bundle Size**: 
  - Client: ~250 KB (gzipped)
  - Server: ~40 KB
- **First Contentful Paint**: <2 seconds

## File Structure
```
roomify/
├── build/                    # Production build output
│   ├── client/              # Client assets
│   └── server/              # Server bundle
├── app/                     # Application code
│   ├── routes/             # Route components
│   └── root.tsx            # Root layout
├── components/             # React components
├── lib/                    # Utilities & actions
├── public/                 # Static assets
├── vercel.json            # Vercel configuration
├── react-router.config.ts # React Router config
├── vite.config.ts         # Vite build config
└── package.json           # Dependencies
```

## Configuration Files

### vercel.json
- Specifies build and dev commands
- Declares environment variables
- Sets deployment region and settings

### .gitignore
Properly configured to exclude:
- `node_modules/` (reinstalled at build time)
- `.react-router/` (auto-generated)
- `build/` (generated during build)
- `.env` (keep only `.env.example`)
- IDE and cache files

## Common Issues

| Issue | Solution |
|-------|----------|
| Build timeout | Increase build timeout in vercel.json or optimize dependencies |
| Memory errors | Reduce bundle size or increase function memory |
| Missing env vars | Set environment variables in Vercel dashboard |
| Puter API errors | Verify VITE_PUTER_WORKER_URL is correct and accessible |
| Type errors | Run `npm run typecheck` to fix TypeScript issues |

## Rollback
```bash
# Rollback to previous deployment
vercel rollback

# Or redeploy a specific commit
git checkout <commit-hash>
git push
```

## Support
- [React Router Documentation](https://reactrouter.com)
- [Vercel Documentation](https://vercel.com/docs)
- [Vite Documentation](https://vitejs.dev)

---

**Last Updated**: June 2026
**Status**: Production-Ready ✅
