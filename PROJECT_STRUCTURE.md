# Project Structure & Architecture

## Directory Layout

```
roomify/
├── app/                          # React Router application code
│   ├── routes/
│   │   ├── home.tsx             # Home/upload page
│   │   └── visualizer.$id.tsx   # Visualization & comparison page
│   ├── root.tsx                 # Root layout component
│   ├── routes.ts                # Route configuration
│   └── app.css                  # Global styles
│
├── components/                   # Reusable React components
│   ├── Navbar.tsx              # Navigation bar & auth
│   ├── Upload.tsx              # File upload with progress
│   └── ui/
│       └── Button.tsx           # Reusable button component
│
├── lib/                         # Utility functions & actions
│   ├── ai.action.ts            # AI rendering actions (Gemini integration)
│   ├── puter.action.ts         # Puter authentication & project management
│   ├── puter.hosting.ts        # Puter file hosting utilities
│   ├── constants.ts            # Application constants
│   ├── utils.ts                # Helper functions
│   └── puter.worker.js         # Puter worker script
│
├── public/                       # Static assets
│   └── readme/                  # README assets
│
├── build/                        # Production build output (generated)
│   ├── client/                  # Client-side bundles
│   │   └── assets/
│   └── server/                  # Server-side bundle
│
├── Configuration Files
├── vercel.json                  # Vercel deployment config
├── vite.config.ts              # Vite build configuration
├── react-router.config.ts       # React Router SSR config
├── tsconfig.json               # TypeScript configuration
├── tailwind.config.js          # Tailwind CSS config (implicit)
├── package.json                # Dependencies & scripts
├── .gitignore                  # Git ignore patterns
├── .env                        # Environment variables (local)
├── .env.example               # Environment template
├── Dockerfile                 # Docker configuration
└── README.md                  # Project documentation
```

## Core Technologies

### Framework & Build
- **React Router v7.12.0**: Full-stack framework with SSR
- **Vite v7.1.7**: Lightning-fast build tool
- **TypeScript v5.9.2**: Type-safe JavaScript
- **React v19.2.4**: UI library
- **React DOM v19.2.4**: DOM rendering

### Styling
- **Tailwind CSS v4.1.13**: Utility-first CSS
- **Tailwind Vite Plugin**: Vite integration

### AI & Backend Services
- **Puter.js v2.2.10**: File hosting, auth, and worker execution
- **Gemini Vision API**: 2D-to-3D image transformation

### Additional Libraries
- **lucide-react v0.563.0**: Icon library
- **react-compare-slider v3.1.0**: Image comparison component
- **isbot v5.1.31**: Bot detection for SSR optimization

## Key Files Explained

### app/root.tsx
- Root layout component
- Handles global authentication state
- Manages error boundaries
- Loads fonts and base styles

### app/routes/home.tsx
- Upload page for floor plans
- File processing and validation
- Integration with AI rendering

### app/routes/visualizer.$id.tsx
- Displays before/after comparison
- 3D render generation with Gemini
- Project sharing and download
- Local storage management

### lib/ai.action.ts
- `generate3DView()`: Calls Gemini Vision API with floor plan
- Image format conversion (Data URL, blob)
- Error handling for AI service

### lib/puter.action.ts
- `signIn()`: Puter authentication
- `createProject()`: Save projects to Puter
- `getProjects()`: Fetch user's projects
- File hosting integration

### lib/constants.ts
- Configuration values
- Storage paths
- Timing constants
- AI prompt template

## Data Flow

```
┌─────────────────────────────────────────┐
│ User Upload (home.tsx)                  │
│ - Image selection                       │
│ - File validation                       │
│ - Progress indication                   │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│ AI Processing (generate3DView)          │
│ - Convert to base64                     │
│ - Call Gemini Vision API                │
│ - Receive 3D render                     │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│ Project Management (puter.action.ts)    │
│ - Upload to Puter hosting               │
│ - Save metadata                         │
│ - Store in database                     │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│ Visualization (visualizer.$id.tsx)      │
│ - Load project from storage             │
│ - Display comparison slider             │
│ - Enable download/sharing               │
└─────────────────────────────────────────┘
```

## Build Process

### Development Build
```
npm run dev
↓
Vite dev server + HMR
↓
React Router hot reload
↓
Instant feedback
```

### Production Build
```
npm run build
↓
TypeScript compilation
↓
Vite optimization
├─ Client bundle (~250KB gzipped)
├─ Server bundle (~40KB)
└─ CSS extraction
↓
Optimized for deployment
```

## Type System

### Key Interfaces
```typescript
interface DesignItem {
  id: string;
  sourceImage: string;
  renderedImage?: string;
  createdAt: string;
}

interface AuthContext {
  isSignedIn: boolean;
  user?: PuterUser;
}
```

## Environment & Configuration

### Environment Variables
```env
VITE_PUTER_WORKER_URL=https://your-worker.puter.work
```

### Build Configuration (vite.config.ts)
- React Router plugin
- Tailwind CSS plugin
- TypeScript path mapping
- ES2022 target

### React Router Config
- SSR enabled
- File-based routing
- Lazy loading support

## Performance Considerations

### Optimizations Applied
- ✅ Tree-shaking via Vite
- ✅ Code splitting for routes
- ✅ CSS extraction & optimization
- ✅ Server-side rendering
- ✅ Lazy component loading
- ✅ Image optimization (compare slider)

### Metrics
- **Build Time**: ~30-40 seconds
- **Client Bundle**: ~250 KB (gzipped)
- **Server Bundle**: ~40 KB
- **Initial Load**: <2 seconds (target)

## Development Workflow

### Local Development
```bash
npm run dev       # Start dev server with HMR
npm run typecheck # Validate types
npm run build     # Create production build
npm start         # Serve production build
```

### Code Quality
```bash
# All TypeScript files are type-checked
npm run typecheck

# No linting configured (add ESLint if needed)
# No formatting enforced (consider Prettier)
```

## Future Improvements
- [ ] Add ESLint for code quality
- [ ] Add Vitest for unit testing
- [ ] Add E2E testing (Playwright/Cypress)
- [ ] Implement caching strategy
- [ ] Add analytics
- [ ] Progressive Web App (PWA) support
- [ ] Internationalization (i18n)

---

**Last Updated**: June 2026
**Version**: 1.0.0
