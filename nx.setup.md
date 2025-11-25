# Nx Monorepo Setup Guide

This document provides a comprehensive guide for setting up and working with the Nx monorepo for Rangrez Club Frontend.

## Table of Contents

- [Initial Setup](#initial-setup)
- [Project Structure](#project-structure)
- [Angular CLI Commands with Nx](#angular-cli-commands-with-nx)
- [Common Nx Commands](#common-nx-commands)
- [Best Practices](#best-practices)

---

## Initial Setup

### 1. Create Nx Workspace with Angular

Create a new Nx workspace with Angular monorepo preset:

```bash
npx create-nx-workspace@latest rangrez-club-fe \
  --preset=angular-monorepo \
  --appName=web \
  --style=scss \
  --bundler=esbuild \
  --nx-cloud=skip \
  --packageManager=npm \
  --interactive=false
```

**Explanation:**

- `--preset=angular-monorepo`: Sets up workspace with Angular-specific tooling and structure
- `--appName=web`: Creates the first application named "web"
- `--style=scss`: Uses SCSS for styling (alternatives: css, less, sass)
- `--bundler=esbuild`: Uses esbuild for faster builds (alternatives: webpack, rspack)
- `--nx-cloud=skip`: Skips Nx Cloud integration
- `--packageManager=npm`: Uses npm for package management
- `--interactive=false`: Runs in non-interactive mode with provided defaults

### 2. Generate Additional Applications

Navigate to the workspace and generate the admin application:

```bash
cd rangrez-club-fe
npx nx g @nx/angular:app admin \
  --style=scss \
  --bundler=esbuild \
  --e2eTestRunner=none \
  --unitTestRunner=jest
```

**Explanation:**

- `@nx/angular:app`: Uses Angular application generator
- `admin`: Name of the application
- `--e2eTestRunner=none`: Skips e2e test setup (alternatives: playwright, cypress)
- `--unitTestRunner=jest`: Uses Jest for unit testing (alternative: none)

**Note:** By default, new apps are generated in the root. To move them to the `apps` folder, see [Moving Applications](#moving-applications-to-apps-folder).

### 3. Generate Shared Library

Create a shared library for common code:

```bash
npx nx g @nx/angular:lib \
  --name=shared \
  --directory=libs/shared \
  --unitTestRunner=jest \
  --importPath=@rangrez-club-fe/shared
```

**Explanation:**

- `@nx/angular:lib`: Uses Angular library generator
- `--directory=libs/shared`: Places the library in libs/shared folder
- `--importPath=@rangrez-club-fe/shared`: Sets the import alias for the library
- This allows importing as: `import { Something } from '@rangrez-club-fe/shared'`

## Project Structure

After setup, your monorepo structure should look like:

```
rangrez-club-fe/
├── apps/
│   ├── web/                    # Main web application
│   │   ├── src/
│   │   ├── project.json
│   │   └── ...
│   ├── admin/                  # Admin application
│   │   ├── src/
│   │   ├── project.json
│   │   └── ...
│   └── web-e2e/               # E2E tests for web app
├── libs/
│   └── shared/                # Shared library
│       ├── src/
│       │   ├── index.ts       # Public API
│       │   └── lib/
│       └── project.json
├── dist/                      # Build output
│   └── apps/
│       ├── web/
│       └── admin/
├── node_modules/
├── nx.json                    # Nx workspace configuration
├── package.json
├── tsconfig.base.json         # Base TypeScript config
└── README.md
```

---

## Angular CLI Commands with Nx

Nx provides a wrapper around Angular CLI commands. Here are the most commonly used commands:

### Application Management

#### Generate New Application

```bash
npx nx g @nx/angular:app <app-name> [options]

# Examples:
npx nx g @nx/angular:app dashboard --style=scss --routing
npx nx g @nx/angular:app mobile --directory=apps/mobile
```

**Common Options:**

- `--style`: Stylesheet format (scss, css, less)
- `--routing`: Add Angular routing
- `--prefix`: Component selector prefix (default: app)
- `--bundler`: Build system (esbuild, webpack)

#### Serve Application

```bash
npx nx serve <app-name> [options]

# Examples:
npx nx serve web                    # Default development mode
npx nx serve admin --port=4201      # Custom port
npx nx serve web --configuration=production
```

**Common Options:**

- `--port`: Port number (default: 4200)
- `--open`: Open browser automatically
- `--configuration`: Build configuration (development, production)

#### Build Application

```bash
npx nx build <app-name> [options]

# Examples:
npx nx build web                    # Production build
npx nx build admin --configuration=development
npx nx build web --output-path=dist/custom-path
```

**Common Options:**

- `--configuration`: production or development
- `--output-path`: Custom output directory
- `--optimization`: Enable/disable optimization
- `--source-map`: Generate source maps

### Library Management

#### Generate Library

```bash
npx nx g @nx/angular:lib <lib-name> [options]

# Examples:
npx nx g @nx/angular:lib ui --directory=libs/ui
npx nx g @nx/angular:lib auth --importPath=@rangrez-club-fe/auth
npx nx g @nx/angular:lib utils --buildable
```

**Common Options:**

- `--directory`: Library location
- `--importPath`: Custom import path/alias
- `--buildable`: Make library buildable separately
- `--publishable`: Make library publishable to npm
- `--prefix`: Component selector prefix

#### Build Library

```bash
npx nx build <lib-name>

# Example:
npx nx build shared
```

### Testing

#### Run Unit Tests

```bash
npx nx test <project-name> [options]

# Examples:
npx nx test web                     # Run tests once
npx nx test shared --watch          # Watch mode
npx nx test admin --code-coverage   # With coverage
```

#### Run E2E Tests

```bash
npx nx e2e <project-name>-e2e

# Example:
npx nx e2e web-e2e
```

### Linting

#### Run Linter

```bash
npx nx lint <project-name> [options]

# Examples:
npx nx lint web
npx nx lint admin --fix             # Auto-fix issues
```

---

## Common Nx Commands

### Project Management

#### List All Projects

```bash
npx nx show projects
```

#### View Project Details

```bash
npx nx show project <project-name>

# Example:
npx nx show project web
```

#### View Project Graph

```bash
npx nx graph

# Interactive dependency graph visualization
```

### Running Multiple Targets

#### Run Target for All Projects

```bash
npx nx run-many --target=<target> --all

# Examples:
npx nx run-many --target=build --all
npx nx run-many --target=test --all
npx nx run-many --target=lint --all
```

#### Run Target for Specific Projects

```bash
npx nx run-many --target=<target> --projects=<proj1>,<proj2>

# Example:
npx nx run-many --target=build --projects=web,admin
```

#### Run Affected Commands

```bash
# Test only affected projects
npx nx affected --target=test

# Build only affected projects
npx nx affected --target=build

# Lint only affected projects
npx nx affected --target=lint

# See what's affected
npx nx affected:graph
```

### Code Generation

#### List Available Generators

```bash
npx nx list @nx/angular
```

#### Show Generator Options

```bash
npx nx g @nx/angular:<generator> --help

# Example:
npx nx g @nx/angular:component --help
```

### Dependency Management

#### Update Nx and Dependencies

```bash
npx nx migrate latest              # Check for updates
npx nx migrate --run-migrations    # Apply migrations
```

### Workspace Management

#### Reset Nx Cache

```bash
npx nx reset
```

#### View Nx Configuration

```bash
npx nx show project <project-name> --json
```

---

## Best Practices

### 1. Library Organization

- **Feature Libraries**: Contains business logic for specific features
  ```bash
  npx nx g @nx/angular:lib feature-auth --directory=libs/features/auth
  ```
- **UI Libraries**: Contains presentational components
  ```bash
  npx nx g @nx/angular:lib ui-components --directory=libs/ui
  ```
- **Utility Libraries**: Contains helper functions and utilities
  ```bash
  npx nx g @nx/angular:lib utils --directory=libs/utils
  ```
- **Data Access Libraries**: Contains services for API calls
  ```bash
  npx nx g @nx/angular:lib data-access --directory=libs/data-access
  ```

### 2. Import Paths

Always use the configured import paths from `tsconfig.base.json`:

```typescript
// Good
import { SharedComponent } from '@rangrez-club-fe/shared';

// Avoid
import { SharedComponent } from '../../../libs/shared/src/lib/shared/shared';
```

### 3. Code Sharing

- Place shared code in libraries, not in applications
- Use the `--export` flag when generating components/services in libraries
- Keep applications thin and libraries rich

### 4. Building Strategy

```bash
# Development
npx nx serve web                    # Fast rebuild with hot reload

# Production
npx nx build web --configuration=production
npx nx build admin --configuration=production

# Build all apps
npx nx run-many --target=build --all --configuration=production
```

### 5. Testing Strategy

```bash
# Run affected tests only
npx nx affected --target=test

# Run all tests with coverage
npx nx run-many --target=test --all --code-coverage
```

### 6. Performance Tips

- Use `npx nx affected` commands to only build/test changed code
- Enable build caching in `nx.json`
- Use `--parallel` flag for faster execution:
  ```bash
  npx nx run-many --target=build --all --parallel=3
  ```

### 7. Dependency Constraints

Configure dependency constraints in `nx.json` to enforce architectural boundaries:

```json
{
  "targetDefaults": {
    "build": {
      "dependsOn": ["^build"]
    }
  }
}
```

---

## Quick Reference

### Most Used Commands

```bash
# Serve apps
npx nx serve web
npx nx serve admin --port=4201

# Build apps
npx nx build web
npx nx build admin

# Generate components
npx nx g @nx/angular:component <name> --project=<app> --standalone

# Generate services
npx nx g @nx/angular:service <name> --project=<lib>

# Generate libraries
npx nx g @nx/angular:lib <name> --directory=libs/<name>

# Run tests
npx nx test <project>
npx nx affected --target=test

# Lint code
npx nx lint <project>
npx nx affected --target=lint

# View project graph
npx nx graph

# List all projects
npx nx show projects
```

---

## Additional Resources

- [Nx Documentation](https://nx.dev)
- [Angular Documentation](https://angular.dev)
- [Nx Angular Plugin](https://nx.dev/nx-api/angular)
- [Nx Console](https://nx.dev/getting-started/editor-setup) - VSCode extension for Nx
