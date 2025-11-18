# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Magic Resume is a modern online resume editor built with Next.js 14+. It features real-time preview, theme customization, dark mode support, PDF export, and multi-language support (Chinese and English).

## Development Commands

### Setup and Installation
```bash
pnpm install
```

### Development
```bash
pnpm dev
```
Starts dev server on `http://localhost:3000`

### Build and Production
```bash
pnpm build      # Build for production
pnpm start      # Start production server
```

### Linting
```bash
pnpm lint       # Run ESLint checks
```

## Architecture Overview

### Key Directories

- **`src/app`**: Next.js 14 App Router with:
  - `(public)`: Public landing page routes
  - `app/dashboard`: Resume list and management dashboard
  - `app/workbench`: Main editor interface where users edit resumes
  - `api`: API routes (grammar check, polish, proxy endpoints)

- **`src/store`**: Zustand-based state management:
  - `useResumeStore.ts`: Main state for resume data, includes CRUD operations and file system persistence (using File System Access API)
  - `useAIConfigStore.ts`: AI service configuration
  - `useGrammarStore.ts`: Grammar checking state

- **`src/components`**: React components organized by feature:
  - `editor/`: Editor-related components (toolbar, panels)
  - `preview/`: Resume preview/rendering components
  - `templates/`: Resume template layouts (classic, modern, left-right, timeline)
  - `ai/`: AI-powered features (grammar check, content polish)
  - `ui/`: shadcn/ui components (buttons, inputs, dialogs, etc.)
  - `shared/`: Shared UI elements
  - `magicui/`: Custom animated UI components

- **`src/types`**: TypeScript type definitions:
  - `resume.ts`: Core resume data structures (BasicInfo, Education, Experience, Project, etc.)
  - `template.ts`: Template configuration types

- **`src/config`**: Configuration constants:
  - `index.ts`: Template definitions, default field order, PDF export config, external URLs
  - Initial resume data for different languages

- **`src/utils`**: Utility functions (uuid generation, file system operations)

- **`src/i18n`**: Internationalization setup with next-intl (Chinese and English)

- **`src/theme`**: Theme configuration (colors, fonts)

- **`src/styles`**: Global styles and SCSS files

### State Management

Uses **Zustand** with `persist` middleware for client-side data persistence. The main state (`useResumeStore`) manages:
- Resume collection (resumes stored as Record<resumeId, ResumeData>)
- Active resume tracking
- All resume data mutations (basic info, education, experience, projects, skills, custom sections)
- Menu section visibility and reordering
- File system integration for exporting/importing

### Key Technologies

- **Framework**: Next.js 14 (App Router) with TypeScript
- **Styling**: Tailwind CSS + SCSS
- **UI Components**: Radix UI primitives with shadcn/ui wrapping
- **State**: Zustand (client-side) with localStorage persistence
- **Rich Text Editor**: Tiptap (used in various content editors)
- **Animations**: Framer Motion
- **PDF Export**: Server-side generation via external API endpoint
- **Internationalization**: next-intl (routes: `/en/*` and `/zh/*`)
- **Icons**: Lucide React
- **File Operations**: File System Access API (IndexedDB as fallback)

### Resume Data Structure

A resume contains:
- `basicInfo`: Name, title, email, phone, location, birth date
- `educations[]`: Educational background entries
- `experiences[]`: Work experience entries
- `projects[]`: Project showcases
- `skillContent`: Skills section (rich text)
- `menuSections[]`: Dynamic section configuration controlling visibility and order
- `customItems`: User-defined custom sections
- `globalSettings`: Font, color scheme, spacing preferences
- `templateId`: Selected template

### Component Patterns

1. **Templates**: Located in `src/components/templates/`, each template (Classic, Modern, etc.) defines layout-specific rendering of resume data
2. **Editor Panels**: Input controls for editing each section of the resume
3. **Preview**: Real-time rendered view of the resume using selected template
4. **AI Features**: Grammar checking and content polishing via API routes

## Build Configuration

- **TypeScript**: Strict mode enabled, `@/*` path alias points to `src/`
- **Output**: Standalone Docker-compatible build format
- **Environment**: next-intl plugin configured for i18n
- **Internationalization Routes**: Configured via `src/i18n/routing.public.ts`

## Important Notes

- Resume data is persisted to browser storage via Zustand's persist middleware
- File System Access API is used for native file operations (saving/loading resumes)
- PDF export uses an external Tencent SCF endpoint (configured in `src/config/index.ts`)
- The app ignores TypeScript build errors (see `next.config.mjs`)
- Locale detection is automatic based on URL pattern: `/en/*` or `/zh/*`
