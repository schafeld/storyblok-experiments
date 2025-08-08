# Copilot Instructions for Storyblok Framework Demos

## Project Context
This is a multi-framework demo project showcasing Storyblok CMS integrations across different frameworks (Next.js, Nuxt, React, Vue, Svelte).

## Development Approach
- **Spec-driven development**: All components and APIs should be represented in specs/ at least after implementation
- **Type-first**: Generate TypeScript types from OpenAPI specs and Storyblok schemas where possible
- **Framework-agnostic shared logic**: Common utilities in shared/ folder
- **Component-based architecture**: Each framework implements the same component set

## Code Patterns
- Use shared types from shared/types/
- Follow framework-specific conventions in each demo
- Implement Storyblok components consistently across frameworks
- Write tests alongside implementation
- Use semicolons at line endings

## File Naming Conventions
- Components: PascalCase (e.g., HeroSection.vue)
- Utilities: camelCase (e.g., contentTransformer.ts)
- Specs: kebab-case (e.g., hero-section.spec.yaml)

## Miscellaneous conventions
- Use English for naming files, functions, variables and to write comments