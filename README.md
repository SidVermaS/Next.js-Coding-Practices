# Next.js Coding Practices

## Bundle Optimization
### 1. Implement Code Splitting and Lazy Loading
Dynamic imports for heavy components using `next/dynamic`:
```typescript
const Component = dynamic(() => import('./Component'))
```
This minimizes initial JavaScript bundle size and speeds up first-page loads.

### 2. Optimize Images with the Next.js Image Component
Use the built-in `<Image>` component in `next/image`. 
It automatically handles resizing, lazy loading, WebP/AVIF format conversion, and placeholder generation, reducing file sizes by up to 50%.

### 3. Font Optimization
Import fonts via `next/font` to self host font files. Avoid external requests.

### 4. Remove Unused Dependencies and Enable Tree Shaking
i) Audit your package.json with tools like `depcheck` or `npm ls --depth=0` to remove unused packages, which can shrink your bundle by 20-40%.<br/>
ii) Use `next/bundle-analyzer` to identify large dependencies. Either replace them with light weight alternatives or write the custom code for only the required components/functionality in the application.<br/>
iii) Import only what you need: 
```typescript
import { specific } from 'library' ✅
```
instead of the whole library
```typescript
import * as library from 'library' ❌
```