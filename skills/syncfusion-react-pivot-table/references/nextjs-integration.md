# Next.js Integration - Syncfusion React Pivot Table

## Overview

Integrating the Syncfusion React Pivot Table into Next.js applications requires careful handling of SSR (Server-Side Rendering), dynamic imports, and browser APIs. This guide covers best practices for seamless integration with Next.js 13+ (App Router) and Next.js 12 (Pages Router).

### Key Considerations
- **SSR Compatibility**: Pivot Table is browser-dependent; requires dynamic imports
- **Data Hydration**: Prevent mismatch between server and client renders
- **Performance**: Optimize bundle size and initial loads
- **API Routes**: Handle data fetching server-side for OLAP/database connections
- **Styling**: Manage Syncfusion CSS imports in Next.js

## Basic Next.js Setup (App Router)

### Step 1: Install Dependencies

```bash
npm install @syncfusion/ej2-react-pivotview @syncfusion/ej2-base @syncfusion/ej2-buttons @syncfusion/ej2-dropdowns
```

### Step 2: Create Pivot Component with Dynamic Import

```typescript
// components/PivotTable.tsx
'use client';  // This is a Client Component

import dynamic from 'next/dynamic';
import React from 'react';

// Dynamically import PivotView with ssr disabled
const PivotViewComponent = dynamic(
  () => import('@syncfusion/ej2-react-pivotview').then(mod => mod.PivotViewComponent),
  { ssr: false, loading: () => <div>Loading Pivot Table...</div> }
);

export function PivotTable() {
  const [isMounted, setIsMounted] = React.useState(false);

  React.useEffect(() => {
    setIsMounted(true);
  }, []);

  if (!isMounted) return <div>Loading...</div>;

  const dataSourceSettings = {
    dataSource: [
      { Country: 'USA', Product: 'Laptops', Sales: 5000, Year: 2020 },
      { Country: 'USA', Product: 'Mobiles', Sales: 3000, Year: 2020 },
      { Country: 'Canada', Product: 'Laptops', Sales: 2500, Year: 2020 }
    ],
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales' }]
  };

  return (
    <div style={{ width: '100%', height: '600px' }}>
      <PivotViewComponent
        id="pivotview"
        dataSourceSettings={dataSourceSettings}
        height="100%"
      />
    </div>
  );
}
```

### Step 3: Use in Page

```typescript
// app/dashboard/page.tsx
'use client';

import { PivotTable } from '@/components/PivotTable';

export default function DashboardPage() {
  return (
    <main>
      <h1>Sales Dashboard</h1>
      <PivotTable />
    </main>
  );
}
```

### Step 4: Configure CSS

In `app/layout.tsx` or your global CSS file:

```typescript
// app/layout.tsx
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-buttons/styles/material.css';
import '@syncfusion/ej2-dropdowns/styles/material.css';
import '@syncfusion/ej2-grids/styles/material.css';
import '@syncfusion/ej2-pivotview/styles/material.css';
import '@syncfusion/ej2-popups/styles/material.css';
```

## Pages Router Integration (Next.js 12)

### Setup for Pages Router

```typescript
// pages/pivot-dashboard.tsx
import dynamic from 'next/dynamic';
import React from 'react';

const PivotViewComponent = dynamic(
  () => import('@syncfusion/ej2-react-pivotview').then(m => m.PivotViewComponent),
  { ssr: false }
);

export default function PivotDashboard() {
  return (
    <div style={{ width: '100%', height: '600px' }}>
      <PivotViewComponent
        dataSourceSettings={{
          dataSource: [],
          rows: [{ name: 'Country' }],
          columns: [{ name: 'Year' }],
          values: [{ name: 'Sales' }]
        }}
        height="100%"
      />
    </div>
  );
}
```

## Server-Side Data Fetching

### Fetch Data in API Route

```typescript
// app/api/pivot-data/route.ts
import { NextResponse } from 'next/server';

export async function GET() {
  try {
    // Fetch from database or external API
    const data = await fetchPivotData();
    
    return NextResponse.json(data);
  } catch (error) {
    return NextResponse.json(
      { error: 'Failed to fetch data' },
      { status: 500 }
    );
  }
}

async function fetchPivotData() {
  // Example: fetch from your database
  // In production, use proper database client
  return [
    { Country: 'USA', Product: 'Laptops', Sales: 5000, Year: 2020 },
    { Country: 'USA', Product: 'Mobiles', Sales: 3000, Year: 2020 },
    { Country: 'Canada', Product: 'Laptops', Sales: 2500, Year: 2020 }
  ];
}
```

### Fetch Data in Component

```typescript
// components/ServerDataPivot.tsx
'use client';

import dynamic from 'next/dynamic';
import React from 'react';

const PivotViewComponent = dynamic(
  () => import('@syncfusion/ej2-react-pivotview').then(mod => mod.PivotViewComponent),
  { ssr: false }
);

export function ServerDataPivot() {
  const [data, setData] = React.useState([]);
  const [loading, setLoading] = React.useState(true);

  React.useEffect(() => {
    // Fetch from API route
    fetch('/api/pivot-data')
      .then(res => res.json())
      .then(data => {
        setData(data);
        setLoading(false);
      })
      .catch(err => {
        console.error('Failed to fetch pivot data:', err);
        setLoading(false);
      });
  }, []);

  if (loading) return <div>Loading data...</div>;

  const dataSourceSettings = {
    dataSource: data,
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales' }]
  };

  return (
    <div style={{ width: '100%', height: '600px' }}>
      <PivotViewComponent
        id="pivotview"
        dataSourceSettings={dataSourceSettings}
        height="100%"
      />
    </div>
  );
}
```

## Advanced: OLAP Server Connection

For server-side aggregation with OLAP:

```typescript
// components/OLAPPivot.tsx
'use client';

import dynamic from 'next/dynamic';
import React from 'react';

const PivotViewComponent = dynamic(
  () => import('@syncfusion/ej2-react-pivotview').then(mod => mod.PivotViewComponent),
  { ssr: false }
);

export function OLAPPivot() {
  const dataSourceSettings = {
    dataSource: {
      url: process.env.NEXT_PUBLIC_OLAP_SERVER_URL,
      mode: 'Server',
      type: 'XML'
    },
    rows: [{ name: '[Geography].[Country]' }],
    columns: [{ name: '[Date].[Year]' }],
    values: [{ name: '[Measures].[Amount]' }],
    filters: []
  };

  return (
    <div style={{ width: '100%', height: '600px' }}>
      <PivotViewComponent
        id="olap-pivot"
        dataSourceSettings={dataSourceSettings}
        height="100%"
      />
    </div>
  );
}
```

## Handling Dynamic Routes

### Dynamic Pivot Configuration

```typescript
// app/dashboard/[id]/page.tsx
'use client';

import dynamic from 'next/dynamic';
import { useParams } from 'next/navigation';
import React from 'react';

const PivotViewComponent = dynamic(
  () => import('@syncfusion/ej2-react-pivotview').then(mod => mod.PivotViewComponent),
  { ssr: false }
);

export default function DashboardDetail() {
  const params = useParams();
  const [config, setConfig] = React.useState(null);

  React.useEffect(() => {
    // Fetch configuration based on dashboard ID
    fetch(`/api/dashboard/${params.id}/config`)
      .then(res => res.json())
      .then(data => setConfig(data));
  }, [params.id]);

  if (!config) return <div>Loading configuration...</div>;

  return (
    <div>
      <h1>Dashboard: {config.name}</h1>
      <PivotViewComponent
        dataSourceSettings={config.pivotSettings}
        height={600}
      />
    </div>
  );
}
```

## Performance Optimization

### Image Optimization with next/image

For headers/branding in pivot tables:

```typescript
import Image from 'next/image';

export function PivotWithBranding() {
  return (
    <div>
      <div style={{ marginBottom: '20px' }}>
        <Image
          src="/company-logo.png"
          alt="Company Logo"
          width={200}
          height={50}
        />
      </div>
      {/* PivotViewComponent here */}
    </div>
  );
}
```

### Code Splitting with Dynamic Routes

Lazy load pivot features only when needed:

```typescript
const PivotViewComponent = dynamic(
  () => import('@syncfusion/ej2-react-pivotview').then(mod => mod.PivotViewComponent),
  { ssr: false }
);

const GroupingBar = dynamic(
  () => import('@syncfusion/ej2-react-pivotview').then(mod => mod.GroupingBar),
  { ssr: false }
);

const FieldList = dynamic(
  () => import('@syncfusion/ej2-react-pivotview').then(mod => mod.FieldList),
  { ssr: false }
);

// Only import when needed
```

### Environment Variables

```env
# .env.local
NEXT_PUBLIC_OLAP_SERVER_URL=https://your-olap-server.com
NEXT_PUBLIC_DATA_API=https://your-api.com
DATABASE_URL=your-database-connection-string
```

## State Management Integration

### Using Zustand

```typescript
// store/pivotStore.ts
import { create } from 'zustand';

interface PivotStore {
  config: any;
  setConfig: (config: any) => void;
  resetConfig: () => void;
}

export const usePivotStore = create<PivotStore>((set) => ({
  config: null,
  setConfig: (config) => set({ config }),
  resetConfig: () => set({ config: null })
}));
```

```typescript
// Use in component
'use client';

import { usePivotStore } from '@/store/pivotStore';

export function StoredPivot() {
  const { config, setConfig } = usePivotStore();

  return (
    <PivotViewComponent
      dataSourceSettings={config}
      actionComplete={(args: any) => {
        setConfig(args.dataSourceSettings);
      }}
    />
  );
}
```

### Using React Query

```typescript
import { useQuery } from '@tanstack/react-query';

export function QueryPivot() {
  const { data, isLoading } = useQuery({
    queryKey: ['pivot-data'],
    queryFn: () => fetch('/api/pivot-data').then(res => res.json())
  });

  if (isLoading) return <div>Loading...</div>;

  return (
    <PivotViewComponent
      dataSourceSettings={{
        dataSource: data,
        rows: [{ name: 'Country' }],
        columns: [{ name: 'Year' }],
        values: [{ name: 'Sales' }]
      }}
    />
  );
}
```

## Deployment Considerations

### Vercel Deployment

```json
// vercel.json
{
  "buildCommand": "next build",
  "devCommand": "next dev",
  "installCommand": "npm install",
  "env": {
    "NEXT_PUBLIC_OLAP_SERVER_URL": "@olap_server_url",
    "DATABASE_URL": "@database_url"
  }
}
```

### Build Optimization

In `next.config.js`:

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  // Optimize bundle size
  webpack: (config, { isServer }) => {
    if (!isServer) {
      // Exclude server-only packages from client bundle
      config.optimization.splitChunks.cacheGroups = {
        ...config.optimization.splitChunks.cacheGroups,
        syncfusion: {
          test: /[\\/]node_modules[\\/]@syncfusion[\\/]/,
          name: 'syncfusion-vendors',
          priority: 10,
          reuseExistingChunk: true
        }
      };
    }
    return config;
  },
  // Enable compression
  compress: true
};

module.exports = nextConfig;
```

## Common Issues and Solutions

### Issue: "ReferenceError: localStorage is not defined"

**Solution:** Check if code runs on client:

```typescript
'use client';  // Mark as client component

React.useEffect(() => {
  // localStorage only available after mount
  const saved = localStorage.getItem('pivotConfig');
}, []);
```

### Issue: Hydration Mismatch

**Solution:** Use dynamic import with ssr: false:

```typescript
const PivotViewComponent = dynamic(
  () => import('@syncfusion/ej2-react-pivotview').then(mod => mod.PivotViewComponent),
  { ssr: false }  // Critical for Next.js
);
```

### Issue: CSS Not Loading

**Solution:** Import CSS in root layout:

```typescript
// app/layout.tsx
import '@syncfusion/ej2-pivotview/styles/material.css';
```

### Issue: Performance Slow on First Load

**Solution:** Implement lazy loading:

```typescript
const PivotViewComponent = dynamic(
  () => import('@syncfusion/ej2-react-pivotview').then(mod => mod.PivotViewComponent),
  { ssr: false, loading: () => <Skeleton /> }
);
```

## Testing in Next.js

### Unit Test Example

```typescript
// __tests__/PivotTable.test.tsx
import { render } from '@testing-library/react';
import { PivotTable } from '@/components/PivotTable';

jest.mock('next/dynamic', () => ({
  __esModule: true,
  default: (...args: any[]) => {
    const dynamicModule = jest.requireActual('next/dynamic');
    const dynamicActualComp = dynamicModule.default;
    const RequiredComponent = dynamicActualComp(args[0]);
    return RequiredComponent.render ? RequiredComponent : RequiredComponent;
  }
}));

describe('PivotTable', () => {
  it('renders correctly', () => {
    const { container } = render(<PivotTable />);
    expect(container).toBeInTheDocument();
  });
});
```

## Best Practices

✅ **Do:**
- Use `'use client'` directive for client components
- Use `dynamic()` with `ssr: false` for Syncfusion components
- Fetch data from API routes, not directly in components  
- Handle loading and error states
- Optimize CSS imports with PurgeCSS

❌ **Don't:**
- Try to render Syncfusion components server-side
- Use `localStorage` without checking `typeof window`
- Import entire Syncfusion library at root level
- Mix server and client rendering
- Forget to set component heights

## Related Guides

- [Getting Started](./getting-started.md)
- [Data Binding](./data-binding.md)
- [State Persistence](./state-persistence.md)
- [Performance Optimization](./performance.md)
