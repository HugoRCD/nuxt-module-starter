# AGENTS.md

> **Note:** This is a starter template. Once the project is initialized with your module name and configuration, this file should be updated or replaced with project-specific instructions.

## What Is This?

This is a **Nuxt Module Starter** — a monorepo template for creating Nuxt modules with:

- A publishable Nuxt module (`packages/my-module`)
- A playground app for development (`apps/playground`)
- A Docus-powered documentation site (`apps/docs`)
- GitHub workflows for CI, releases, and PR previews
- Automd for automated README snippets

## Initialization Checklist

When starting a new project from this starter, the following must be replaced:

### 1. Module Identity

Replace all occurrences of the following placeholders:

| Placeholder | Replace With | Example |
|-------------|--------------|---------|
| `my-module` | Your package name (kebab-case) | `nuxt-icon` |
| `My Module` | Your module display name | `Nuxt Icon` |
| `myModule` | Your module config key (camelCase) | `nuxtIcon` |
| `your-org/your-module` | Your GitHub repository | `nuxt/icon` |
| `My new Nuxt module` | Your module description | `Icon module for Nuxt` |
| `example.com` | Your documentation URL | `nuxt-icon.nuxt.dev` |
| `@YourName` | Your GitHub handle | `@HugoRCD` |

### 2. Files to Rename

```
packages/my-module/          → packages/<your-module>/
apps/playground/             (update nuxt.config.ts module reference)
apps/docs/                   (update package.json name)
```

### 3. Files Requiring Updates

| File | What to Update |
|------|----------------|
| `package.json` (root) | `name`, `repository.url`, script filters |
| `packages/my-module/package.json` | `name`, `description`, `repository.url` |
| `packages/my-module/src/module.ts` | `meta.name`, `meta.configKey`, `meta.docs` |
| `apps/playground/nuxt.config.ts` | Module import and config key |
| `apps/docs/package.json` | `name` |
| `README.md` | Module name, description, badges |
| `.github/snippets/*.md` | Feature descriptions, URLs |
| `apps/docs/content/` | Documentation content |
| `LICENSE` | Author name (if needed) |

### 4. Dependency Updates

After initialization, check for outdated dependencies:

```bash
# Check outdated packages
bun outdated

# Update all dependencies (interactive)
bunx npm-check-updates -i

# Or update directly
bunx npm-check-updates -u && bun install
```

Key dependencies to keep current:
- `nuxt` and `@nuxt/*` packages
- `@nuxt/kit`, `@nuxt/schema`, `@nuxt/module-builder`
- `docus` (for docs)
- `turbo`, `vitest`, `eslint`

## Monorepo Structure

```
├── apps/
│   ├── playground/          # Dev environment for testing the module
│   │   ├── app.vue          # Test your module features here
│   │   └── nuxt.config.ts   # Module is imported here
│   └── docs/                # Docus documentation site
│       ├── content/         # Markdown documentation pages
│       └── public/          # Static assets
├── packages/
│   └── my-module/           # The publishable Nuxt module
│       ├── src/
│       │   ├── module.ts    # Module definition (main entry)
│       │   └── runtime/     # Runtime code (plugins, composables, components)
│       └── test/            # Module tests
├── .github/
│   ├── workflows/           # CI, release, PR preview workflows
│   └── snippets/            # Automd README snippets
├── turbo.json               # Turborepo configuration
└── package.json             # Root workspace configuration
```

## Commands

```bash
# Install dependencies
bun install

# Prepare module for development (generates types)
bun run dev:prepare

# Start playground (for module development)
bun run dev

# Start documentation site
bun run docs

# Build module
bun run build:module

# Build documentation
bun run build:docs

# Run all tests
bun run test

# Lint all packages
bun run lint
bun run lint:fix

# Type check all packages
bun run typecheck

# Update README with automd snippets
bun run automd
```

## Development Workflow

1. **Develop the module** in `packages/my-module/src/`
2. **Test features** in `apps/playground/app.vue`
3. **Write tests** in `packages/my-module/test/`
4. **Document** in `apps/docs/content/`
5. **Update README** snippets in `.github/snippets/`

## Module Development

### Adding Composables

```typescript
// packages/my-module/src/runtime/composables/useMyFeature.ts
export function useMyFeature() {
  return { /* ... */ }
}

// packages/my-module/src/module.ts
addImports({
  name: 'useMyFeature',
  from: resolver.resolve('./runtime/composables/useMyFeature')
})
```

### Adding Components

```typescript
// packages/my-module/src/module.ts
addComponent({
  name: 'MyComponent',
  filePath: resolver.resolve('./runtime/components/MyComponent.vue')
})
```

### Module Options

```typescript
// packages/my-module/src/module.ts
export interface ModuleOptions {
  enabled: boolean
  prefix: string
}

export default defineNuxtModule<ModuleOptions>({
  defaults: {
    enabled: true,
    prefix: 'My'
  },
  setup(options, nuxt) {
    // Access options.enabled, options.prefix
  }
})
```

## Publishing

The module uses `changelogen` for changelog generation and versioning:

```bash
# From packages/my-module
bun run release
```

This will:
1. Run lint and tests
2. Build the module
3. Generate changelog
4. Publish to npm
5. Push git tags

## CI/CD

- **CI** (`ci.yml`): Runs on push/PR to main — build, lint, typecheck, test
- **Release** (`release.yml`): Automated releases via changelogen
- **PR Preview** (`publish` job in `ci.yml`): Creates npm preview packages for PRs

## Code Guidelines

### Nuxt UI (in playground/docs)

Use Nuxt UI components and semantic colors:

```vue
<!-- Use components -->
<UButton>Click</UButton>
<UInput v-model="value" />

<!-- Use semantic colors -->
<div class="bg-muted text-highlighted">
<div class="border-default">
```

### Auto-imports

Nuxt auto-imports from standard directories — no manual imports needed for:
- Composables from `composables/`
- Components from `components/`
- Utils from `utils/`

### File Naming

- **Files**: kebab-case (`my-composable.ts`)
- **Vue components**: PascalCase (`MyComponent.vue`)
- **Directories**: kebab-case

## After Initialization

Once your module is set up:

1. **Update this file** with your module-specific instructions, or
2. **Delete this file** if not needed for your workflow

Consider keeping sections relevant to your module:
- Project-specific commands
- Architecture overview
- Development guidelines
- Testing instructions
