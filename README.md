# Next.js Coding Practices

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
For eg - Use date-fns instead of moment.js, zustand instead of Redux<br/>
iii) Import only what you need: 
```typescript
import { specific } from 'library' ✅
```
instead of the whole library
```typescript
import * as library from 'library' ❌
```
### 5. Optimize Third-Party Scripts with the Script Component
Load analytics, ads, or embeds via `<script>` with strategies like strategy="lazyOnload" or afterInteractive to prevent render-blocking.

### 6. Minify and Compress Static Assets
Enable automatic CSS/JS minification in production builds. For images and other assets, use responsive sizes, SVGs for icons and CDNs.<br/>
Always to store SVGs in the public folder of the app

### 7. Choose the Right Rendering Strategy (SSR, SSG, ISR, CSR)
i) Server-Side Rendering (SSR)
Pages are rendered on the server for each request. This provides fresh data on every page load and is great for SEO since content is immediately available to crawlers.
```typescript
export async function getServerSideProps(context) {
  const data = await fetchData()
  return { props: { data } }
}
```
ii) Static Site Generation (SSG)
iii) Incremental Static Regeneration (ISR)
```typescript
export async function getStaticProps() {
  const data = await fetchData()
  return { props: { data }, revalidate: 3600  } // ISR with 1 hour revalidation
}
```
iv) Client-Side Rendering (CSR)
```typescript
import { useEffect, useState } from 'react'

export default function Page() {
  const [data, setData] = useState(null)
  
  useEffect(() => {
    fetch('/api/data').then(res => res.json()).then(setData)
  }, [])
  
  return data ? <div>{data.content}</div> : <div>Loading...</div>
}
```

### 8. Monitoring Performance
i) Client-Side Monitoring: Core Web Vitals with useReportWebVitals
Use `useReportWebVitals` from `next/web-vitals` to monitor the performance
```typescript
import { useReportWebVitals } from 'next/web-vitals'; // App Router
// Or import Script from 'next/script'; for Pages Router

export default function RootLayout({ children }) {
  useReportWebVitals((metric) => {
    // Send to analytics (e.g., GA, console.log for testing)
    console.log(metric); // { name: 'LCP', value: 2500, ... }
    // Example: window.gtag?.('event', metric.name, { value: metric.value });
  });

  return (
    <html>
      <body>{children}</body>
    </html>
  );
}
```