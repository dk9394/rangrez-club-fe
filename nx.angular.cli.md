### Component Generation

#### Generate Component

```bash
npx nx g @nx/angular:component <component-name> [options]

# Examples:
npx nx g @nx/angular:component header --project=web
npx nx g @nx/angular:component login --project=admin --export
npx nx g @nx/angular:component button --project=shared --directory=ui
```

**Common Options:**

- `--project`: Target project (required)
- `--directory`: Subdirectory within project
- `--export`: Export component from module
- `--standalone`: Create standalone component
- `--style`: inline, scss, css, none
- `--changeDetection`: OnPush or Default

#### Generate Standalone Component

```bash
npx nx g @nx/angular:component <name> --standalone --project=<app>

# Example:
npx nx g @nx/angular:component user-profile --standalone --project=web
```

### Service Generation

#### Generate Service

```bash
npx nx g @nx/angular:service <service-name> [options]

# Examples:
npx nx g @nx/angular:service auth --project=shared
npx nx g @nx/angular:service api --project=web --directory=services
```

**Common Options:**

- `--project`: Target project
- `--directory`: Subdirectory for the service

### Module Generation

#### Generate Module

```bash
npx nx g @nx/angular:module <module-name> [options]

# Examples:
npx nx g @nx/angular:module feature --project=web
npx nx g @nx/angular:module core --project=shared --routing
```

**Common Options:**

- `--routing`: Add routing module
- `--project`: Target project

### Directive & Pipe Generation

#### Generate Directive

```bash
npx nx g @nx/angular:directive <directive-name> [options]

# Examples:
npx nx g @nx/angular:directive highlight --project=shared
npx nx g @nx/angular:directive tooltip --project=web --export
```

#### Generate Pipe

```bash
npx nx g @nx/angular:pipe <pipe-name> [options]

# Examples:
npx nx g @nx/angular:pipe currency-format --project=shared
npx nx g @nx/angular:pipe date-format --project=web --export
```

### Guard & Interceptor Generation

#### Generate Guard

```bash
npx nx g @nx/angular:guard <guard-name> [options]

# Examples:
npx nx g @nx/angular:guard auth --project=shared
npx nx g @nx/angular:guard admin --project=admin --implements=CanActivate
```

#### Generate Interceptor

```bash
npx nx g @nx/angular:interceptor <name> [options]

# Example:
npx nx g @nx/angular:interceptor auth --project=shared
```
