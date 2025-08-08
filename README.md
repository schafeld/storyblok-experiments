# storyblok-experiments
Experimental implementations of Storyblok in different frameworks, based on [Storyblok documentation](https://www.storyblok.com/technologies).

### Repository

[storyblok-experiments](https://github.com/schafeld/storyblok-experiments)

### Concept phase

Prompt to get architecture support from Claude:

```text
I want to set up a new demo project with Copilot support, ideally suitable for spec-driven development. What files and folders should I create? The project goal is to implement several proof of concepts with different framework integrations for Storyblok (https://www.storyblok.com/technologies).
```


# Storyblok Multi-Framework Demo Project Structure

```
storyblok-framework-demos/
├── .github/
│   ├── workflows/
│   │   ├── ci.yml
│   │   └── deploy-previews.yml
│   └── ISSUE_TEMPLATE/
│       ├── bug_report.md
│       └── feature_request.md
├── docs/
│   ├── README.md
│   ├── ARCHITECTURE.md
│   ├── API_SPECS.md
│   ├── DEPLOYMENT.md
│   └── frameworks/
│       ├── nextjs.md
│       ├── nuxt.md
│       ├── react.md
│       ├── vue.md
│       └── svelte.md
├── specs/
│   ├── api/
│   │   ├── storyblok-api.yaml
│   │   └── webhooks.yaml
│   ├── components/
│   │   ├── hero.spec.yaml
│   │   ├── article.spec.yaml
│   │   └── navigation.spec.yaml
│   └── content-types/
│       ├── page.spec.yaml
│       ├── blog-post.spec.yaml
│       └── landing-page.spec.yaml
├── shared/
│   ├── types/
│   │   ├── storyblok.ts
│   │   ├── content.ts
│   │   └── api.ts
│   ├── components/
│   │   ├── StoryblokComponent.ts
│   │   └── RichTextRenderer.ts
│   ├── utils/
│   │   ├── storyblok-client.ts
│   │   ├── content-transformer.ts
│   │   └── validation.ts
│   └── constants/
│       ├── content-types.ts
│       └── api-endpoints.ts
├── frameworks/
│   ├── nextjs-demo/
│   │   ├── package.json
│   │   ├── next.config.js
│   │   ├── .env.example
│   │   ├── pages/
│   │   ├── components/
│   │   ├── lib/
│   │   └── tests/
│   ├── nuxt-demo/
│   │   ├── package.json
│   │   ├── nuxt.config.ts
│   │   ├── .env.example
│   │   ├── pages/
│   │   ├── components/
│   │   ├── composables/
│   │   └── tests/
│   ├── react-demo/
│   │   ├── package.json
│   │   ├── vite.config.js
│   │   ├── .env.example
│   │   ├── src/
│   │   └── tests/
│   ├── vue-demo/
│   │   ├── package.json
│   │   ├── vite.config.js
│   │   ├── .env.example
│   │   ├── src/
│   │   └── tests/
│   └── svelte-demo/
│       ├── package.json
│       ├── svelte.config.js
│       ├── .env.example
│       ├── src/
│       └── tests/
├── tools/
│   ├── scripts/
│   │   ├── setup-framework.sh
│   │   ├── generate-types.js
│   │   └── sync-content-types.js
│   ├── generators/
│   │   ├── component-template.js
│   │   └── page-template.js
│   └── validators/
│       ├── spec-validator.js
│       └── content-validator.js
├── tests/
│   ├── integration/
│   │   ├── storyblok-api.test.js
│   │   └── webhooks.test.js
│   ├── e2e/
│   │   ├── playwright.config.js
│   │   └── specs/
│   └── shared/
│       ├── fixtures/
│       └── mocks/
├── .gitignore
├── .copilot/
│   ├── instructions.md
│   └── context.md
├── package.json (root workspace)
├── tsconfig.json
├── jest.config.js
├── README.md
└── CONTRIBUTING.md
```

## Key Files for Copilot Support & Spec-Driven Development

### 1. `.copilot/instructions.md`
```markdown
# Copilot Instructions for Storyblok Framework Demos

## Project Context
This is a multi-framework demo project showcasing Storyblok CMS integrations across different frameworks (Next.js, Nuxt, React, Vue, Svelte).

## Development Approach
- **Spec-driven development**: All components and APIs should be defined in specs/ before implementation
- **Type-first**: Generate TypeScript types from OpenAPI specs and Storyblok schemas
- **Framework-agnostic shared logic**: Common utilities in shared/ folder
- **Component-based architecture**: Each framework implements the same component set

## Code Patterns
- Use shared types from shared/types/
- Follow framework-specific conventions in each demo
- Implement Storyblok components consistently across frameworks
- Write tests alongside implementation

## File Naming Conventions
- Components: PascalCase (e.g., HeroSection.vue)
- Utilities: camelCase (e.g., contentTransformer.ts)
- Specs: kebab-case (e.g., hero-section.spec.yaml)
```

### 2. `specs/components/hero.spec.yaml`
```yaml
openapi: 3.0.0
info:
  title: Hero Component Specification
  version: 1.0.0
components:
  schemas:
    HeroComponent:
      type: object
      properties:
        component:
          type: string
          enum: [hero]
        headline:
          type: string
          maxLength: 100
        subheadline:
          type: string
          maxLength: 200
        background_image:
          $ref: '#/components/schemas/StoryblokAsset'
        cta_button:
          $ref: '#/components/schemas/CTAButton'
        layout:
          type: string
          enum: [centered, left-aligned, right-aligned]
      required:
        - component
        - headline
    StoryblokAsset:
      type: object
      properties:
        id:
          type: number
        alt:
          type: string
        name:
          type: string
        focus:
          type: string
        title:
          type: string
        filename:
          type: string
    CTAButton:
      type: object
      properties:
        label:
          type: string
        url:
          type: string
        target:
          type: string
          enum: [_self, _blank]
```

### 3. `shared/types/storyblok.ts`
```typescript
// Generated from specs - do not edit manually
export interface StoryblokAsset {
  id: number;
  alt: string;
  name: string;
  focus?: string;
  title?: string;
  filename: string;
}

export interface CTAButton {
  label: string;
  url: string;
  target?: '_self' | '_blank';
}

export interface HeroComponent {
  component: 'hero';
  headline: string;
  subheadline?: string;
  background_image?: StoryblokAsset;
  cta_button?: CTAButton;
  layout?: 'centered' | 'left-aligned' | 'right-aligned';
}
```

### 4. Root `package.json`
```json
{
  "name": "storyblok-framework-demos",
  "private": true,
  "workspaces": [
    "frameworks/*"
  ],
  "scripts": {
    "dev": "concurrently \"npm run dev --workspace=nextjs-demo\" \"npm run dev --workspace=nuxt-demo\"",
    "build": "npm run build --workspaces",
    "test": "jest",
    "test:e2e": "playwright test",
    "generate:types": "node tools/scripts/generate-types.js",
    "validate:specs": "node tools/validators/spec-validator.js",
    "setup:framework": "bash tools/scripts/setup-framework.sh"
  },
  "devDependencies": {
    "@types/node": "^20.0.0",
    "typescript": "^5.0.0",
    "jest": "^29.0.0",
    "playwright": "^1.40.0",
    "concurrently": "^8.0.0",
    "openapi-typescript": "^6.0.0"
  }
}
```

### 5. `tools/scripts/generate-types.js`
```javascript
#!/usr/bin/env node
const fs = require('fs');
const path = require('path');
const { exec } = require('child_process');

// Generate TypeScript types from OpenAPI specs
const specsDir = path.join(__dirname, '../../specs');
const typesDir = path.join(__dirname, '../../shared/types');

// This script would generate types from your YAML specs
console.log('Generating TypeScript types from specifications...');
// Implementation would use openapi-typescript or similar
```

## Getting Started Commands

1. **Initialize the project:**
```bash
mkdir storyblok-framework-demos
cd storyblok-framework-demos
npm init -w frameworks/nextjs-demo -w frameworks/nuxt-demo
```

2. **Set up each framework demo:**
```bash
npm run setup:framework nextjs
npm run setup:framework nuxt
npm run setup:framework react
```

3. **Generate types from specs:**
```bash
npm run generate:types
```

This structure provides:
- **Spec-driven development** with OpenAPI specs for components
- **Copilot context** through instructions and well-organized code
- **Shared utilities** for consistent implementation
- **Multiple framework demos** in isolated workspaces
- **Type generation** from specifications
- **Comprehensive testing setup**
- **CI/CD ready** with GitHub workflows