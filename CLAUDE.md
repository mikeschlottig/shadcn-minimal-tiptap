# CLAUDE.md - AI Assistant Guide for Minimal Tiptap Editor

## Project Overview

Minimal Tiptap Editor is a lightweight, customizable rich text editor component built on [Tiptap](https://tiptap.dev) for integration with [Shadcn UI](https://ui.shadcn.com). It provides an intuitive interface for text formatting with features like image handling, code blocks, and customizable toolbars.

**Repository**: Distributable as a Shadcn UI registry component

## Tech Stack

- **Framework**: React 19.1
- **Build Tool**: Vite 5.4
- **Language**: TypeScript 5.8
- **Styling**: Tailwind CSS 4.1 with CSS variables
- **Editor Core**: Tiptap 3.0 (ProseMirror-based)
- **UI Components**: Radix UI primitives
- **Forms**: React Hook Form + Zod validation
- **Package Manager**: pnpm
- **Theme**: next-themes (dark/light mode)
- **Analytics**: Vercel Analytics

## Project Structure

```
src/
  App.tsx                    # Main demo application
  main.tsx                   # React entry point
  global.css                 # Global Tailwind styles
  lib/
    utils.ts                 # Utility functions (cn)
  components/
    minimal-tiptap/          # Main editor component
      minimal-tiptap.tsx     # Editor implementation
      index.ts               # Public exports
      components/            # UI subcomponents
        bubble-menu/         # Link bubble menu
        image/               # Image editing dialogs
        link/                # Link editing popovers
        section/             # Toolbar sections (one-five)
        toolbar-button.tsx
        toolbar-section.tsx
      extensions/            # Custom Tiptap extensions
        code-block-lowlight/
        color/
        file-handler/
        horizontal-rule/
        image/
        reset-marks-on-enter/
        unset-all-marks/
      hooks/                 # Custom React hooks
        use-minimal-tiptap.ts
        use-container-size.ts
        use-theme.ts
        use-throttle.ts
      styles/                # Editor-specific CSS
        index.css
        partials/
    ui/                      # Shadcn UI components
    custom/                  # Demo-specific components
scripts/
  build.ts                   # Registry build script
  schema.ts                  # Registry schema
registry/
  block-registry.json        # Shadcn component registry
public/                      # Static assets
```

## Key Files

| File | Purpose |
|------|---------|
| `src/main.tsx` | Application entry point with providers |
| `src/App.tsx` | Demo application and form example |
| `src/components/minimal-tiptap/minimal-tiptap.tsx` | Main editor component |
| `src/components/minimal-tiptap/hooks/use-minimal-tiptap.ts` | Editor configuration hook |
| `src/components/minimal-tiptap/extensions/index.ts` | Extension exports |
| `registry/block-registry.json` | Shadcn registry for distribution |
| `scripts/build.ts` | Registry build script |
| `components.json` | Shadcn UI configuration |
| `vite.config.ts` | Vite build configuration |

## Development Setup

### Prerequisites
- Node.js 18+
- pnpm

### Quick Start

```bash
# Install dependencies
pnpm install

# Start development server
pnpm dev

# Build for production
pnpm build

# Preview production build
pnpm preview
```

### Building the Registry

After modifying the component, rebuild the registry for distribution:

```bash
# Build component registry
pnpm build-registry

# Host registry locally for testing
pnpm host-registry
```

## Common Commands

```bash
# Development
pnpm dev              # Start dev server (port 5173)
pnpm build            # Build for production
pnpm preview          # Preview production build

# Registry
pnpm build-registry   # Build shadcn registry
pnpm host-registry    # Host registry at localhost:8080

# Code Quality
pnpm lint             # Run ESLint
pnpm format           # Format with Prettier
```

## Testing

No testing framework is currently configured. Consider adding:
- Vitest for unit tests
- Playwright or Cypress for E2E tests

## Code Conventions

### TypeScript
- Strict mode enabled
- Use explicit types for function parameters and return values
- Path aliases: `@/` maps to `src/`

### File Naming
- Components: PascalCase (e.g., `MinimalTiptapEditor`)
- Files: kebab-case (e.g., `minimal-tiptap.tsx`)
- Hooks: use- prefix (e.g., `use-minimal-tiptap.ts`)

### Component Pattern
```typescript
import * as React from "react"
import { cn } from "@/lib/utils"

interface ComponentProps extends React.HTMLAttributes<HTMLDivElement> {
  // Props
}

export const Component = React.forwardRef<HTMLDivElement, ComponentProps>(
  ({ className, ...props }, ref) => {
    return <div ref={ref} className={cn("base-classes", className)} {...props} />
  }
)

Component.displayName = "Component"
```

### Styling
- Use Tailwind CSS utility classes
- Use `cn()` utility for conditional classes
- CSS variables for theming (defined in `global.css`)
- Component-specific styles in `styles/partials/`

### Code Quality
- **Linter**: ESLint with TypeScript and React plugins
- **Formatter**: Prettier with Tailwind plugin
- Run `pnpm format` before committing

## Editor Configuration

### Props
Key props for the MinimalTiptapEditor:
- `value` - Initial content
- `onChange` - Content change handler
- `output` - Format: 'html' | 'json' | 'text'
- `placeholder` - Placeholder text
- `throttleDelay` - Update throttling (ms)
- `editorClassName` - Editor container classes
- `editorContentClassName` - Content area classes

### Image Extension
```typescript
Image.configure({
  allowedMimeTypes: ["image/jpeg", "image/png", "image/gif"],
  maxFileSize: 5 * 1024 * 1024, // 5MB
  uploadFn: async (file, editor) => {
    // Return uploaded image URL
    return "https://example.com/image.jpg"
  },
  allowBase64: false, // Enable if no uploadFn
})
```

### Toolbar Sections
- `SectionOne` - Heading levels
- `SectionTwo` - Text formatting (bold, italic, etc.)
- `SectionThree` - Links
- `SectionFour` - Lists
- `SectionFive` - Blocks (code, quote, hr)

## Installation in Other Projects

### Via Shadcn Registry
```bash
npx shadcn@latest add https://raw.githubusercontent.com/Aslam97/shadcn-minimal-tiptap/main/registry/block-registry.json
```

### Manual Installation
1. Copy the `minimal-tiptap` directory
2. Install Tiptap dependencies
3. Add required Shadcn UI components
4. Configure TooltipProvider in app root
5. Reference Tailwind entry in styles

## When Making Changes

1. **Follow existing patterns** - Check similar components in `minimal-tiptap/`
2. **Use TypeScript** - Add proper types for all new code
3. **Style with Tailwind** - Use utility classes, avoid custom CSS
4. **Update registry** - Run `pnpm build-registry` after component changes
5. **Format code** - Run `pnpm format` before committing
6. **Test locally** - Use `pnpm host-registry` to test distribution

## Common Issues

- **Tailwind styles not loading**: Ensure `@reference` in `styles/index.css` points to your entry CSS
- **Tooltips not working**: Wrap app with `TooltipProvider`
- **Images not uploading**: Configure `uploadFn` or enable `allowBase64`
- **Type errors**: Check `@tiptap/*` package versions match

## Resources

- [Tiptap Documentation](https://tiptap.dev/docs)
- [Shadcn UI Components](https://ui.shadcn.com)
- [Radix UI Primitives](https://www.radix-ui.com)
- [Official Tiptap Template](https://tiptap.dev/docs/ui-components/templates/simple-editor)
