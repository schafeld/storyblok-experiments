# Copilot Context - Storyblok Framework Demos

## Project Overview
This is a proof-of-concept project demonstrating Storyblok CMS integrations across multiple JavaScript frameworks. The goal is to showcase consistent content management patterns while respecting framework-specific conventions.

## Architecture Patterns

### Storyblok Integration Pattern
```typescript
// Standard pattern for Storyblok component resolution
const StoryblokComponent = ({ blok, ...props }) => {
  const Component = componentMap[blok.component];
  return Component ? <Component blok={blok} {...props} /> : null;
};
```

### Content Fetching Pattern
```typescript
// Use shared client from shared/utils/storyblok-client.ts
import { storyblokClient } from '@shared/utils/storyblok-client';

const fetchStory = async (slug: string, version: 'draft' | 'published' = 'published') => {
  const { data } = await storyblokClient.get(`cdn/stories/${slug}`, {
    version,
    resolve_relations: ['author', 'categories']
  });
  return data.story;
};
```

## Component Conventions

### Storyblok Component Structure
Each framework should implement these core components:
- `HeroSection` - Main page hero with background image and CTA
- `ArticleContent` - Rich text content with embedded components
- `NavigationMenu` - Site navigation with nested menu support
- `BlogList` - List of blog posts with pagination
- `ContactForm` - Form with Storyblok form submission

### Component Props Pattern
```typescript
interface ComponentProps {
  blok: StoryblokComponent; // The component data from Storyblok
  nested?: boolean;        // Whether component is nested in another
  editable?: boolean;      // Whether component should show editor bridge
}
```

## Framework-Specific Patterns

### Next.js (frameworks/nextjs-demo)
- Use `getStaticProps`/`getServerSideProps` for data fetching
- Implement ISR (Incremental Static Regeneration) for content updates
- Use Next.js Image component for Storyblok assets

### Nuxt 3 (frameworks/nuxt-demo)
- Use `useFetch` and `$fetch` composables
- Implement auto-imports for Storyblok components
- Use Nuxt Image for optimized image handling

### React SPA (frameworks/react-demo)
- Use React Query for data fetching and caching
- Implement client-side routing with React Router
- Use Suspense for loading states

### Vue 3 (frameworks/vue-demo)
- Use Composition API with Pinia for state management
- Implement Vue Router for navigation
- Use Suspense for async component loading

### Svelte (frameworks/svelte-demo)
- Use SvelteKit for full-stack functionality
- Implement stores for state management
- Use Svelte's reactive declarations for computed values

## Shared Utilities Usage

### Type Definitions
All frameworks use shared types from `shared/types/`:
```typescript
import type { HeroComponent, ArticleComponent } from '@shared/types/storyblok';
```

### Content Transformation
Use shared transformers for consistent data handling:
```typescript
import { transformRichText, transformAsset } from '@shared/utils/content-transformer';
```

### API Client
Use the configured Storyblok client:
```typescript
import { storyblokClient } from '@shared/utils/storyblok-client';
```

## Environment Variables
Each framework demo should use these environment variables:
```env
STORYBLOK_ACCESS_TOKEN=your_access_token
STORYBLOK_PREVIEW_TOKEN=your_preview_token
STORYBLOK_SPACE_ID=your_space_id
```

## Testing Patterns

### Component Testing
```typescript
// Test Storyblok components with mock data
import { render } from '@testing-library/react';
import { HeroSection } from '../components/HeroSection';
import { mockHeroBlok } from '@shared/tests/fixtures';

test('renders hero with correct content', () => {
  render(<HeroSection blok={mockHeroBlok} />);
  // Test implementation
});
```

### API Testing
```typescript
// Mock Storyblok API responses
import { storyblokClient } from '@shared/utils/storyblok-client';
import { mockStoryResponse } from '@shared/tests/mocks';

jest.mock('@shared/utils/storyblok-client');
```

## Content Management Patterns

### Rich Text Handling
```typescript
// Use shared rich text renderer
import { RichTextRenderer } from '@shared/components/RichTextRenderer';

const ArticleContent = ({ blok }) => (
  <RichTextRenderer content={blok.content} />
);
```

### Asset Optimization
```typescript
// Transform Storyblok assets for optimal delivery
import { getOptimizedImageUrl } from '@shared/utils/content-transformer';

const optimizedUrl = getOptimizedImageUrl(asset.filename, {
  width: 800,
  height: 600,
  format: 'webp'
});
```

### Preview Mode
Each framework should implement Storyblok's Visual Editor:
```typescript
// Enable bridge for editor integration
import { useStoryblokBridge } from '@storyblok/react';

const Page = ({ story }) => {
  useStoryblokBridge(story.id, (newStory) => {
    // Handle story updates in preview mode
  });
};
```

## Build and Deployment

### Build Scripts
Each framework should have consistent npm scripts:
- `dev` - Development server
- `build` - Production build
- `preview` - Preview production build
- `test` - Run tests
- `type-check` - TypeScript validation

### Deployment Configuration
- Vercel deployment config in `vercel.json`
- Netlify deployment config in `netlify.toml`
- Environment variable documentation in `.env.example`

## Code Quality

### TypeScript Configuration
- Strict mode enabled
- Import path aliases for shared utilities
- Consistent tsconfig extends from root

### Linting Rules
- ESLint with framework-specific configs
- Prettier for consistent formatting
- Import sorting with framework preferences

## Performance Considerations

### Image Optimization
- Use framework-specific image components
- Implement lazy loading for Storyblok assets
- Generate responsive image sets

### Caching Strategy
- Cache Storyblok API responses
- Implement stale-while-revalidate patterns
- Use framework-specific caching mechanisms

### Bundle Optimization
- Code splitting for component libraries
- Tree shaking for unused Storyblok features
- Framework-specific optimization techniques