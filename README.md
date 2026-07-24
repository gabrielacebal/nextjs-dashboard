# Next.js App Router

This is the starter template for the Next.js App Router. It contains the starting code for the dashboard application.


## Routing

This project uses the Next.js App Router and file-system-based routing.

Each folder inside the app directory represents a URL segment, while page.tsx defines the page rendered for that route.

```text
app/
├── page.tsx              → /
├── dashboard/
│   └── page.tsx          → /dashboard
└── products/
    └── [id]/
        └── page.tsx      → /products/:id
```

Dynamic routes use square brackets:

```text
app/
└── dashboard/
    └── invoices/
        └── [id]/
            └── page.tsx      → /dashboard/invoices/:id
```

## Automatic Loading UI with `loading.tsx`

Next.js App Router uses a special file convention called `loading.tsx` to automatically display a loading state while a route is being rendered.

When Next.js finds a `loading.tsx` file inside a route folder, it automatically wraps that route's `page.tsx` and its nested children in a React `<Suspense>` boundary. You do not need to manually import or render the loading component.

For example:

```text
app/
└── dashboard/
    ├── loading.tsx
    └── page.tsx
```

```tsx
// app/dashboard/loading.tsx

export default function Loading() {
  return <p>Loading dashboard...</p>;
}
```

```tsx
// app/dashboard/page.tsx

export default async function DashboardPage() {
  const data = await fetchDashboardData();

  return <Dashboard data={data} />;
}
```

When a user navigates to `/dashboard`, Next.js immediately displays the UI defined in `loading.tsx` while `page.tsx` is waiting for its asynchronous data.

Once the page finishes rendering, Next.js automatically replaces the loading UI with the completed page content.

Conceptually, Next.js handles it like this:

```tsx
<Suspense fallback={<Loading />}>
  <DashboardPage />
</Suspense>
```

However, this `<Suspense>` boundary is created automatically by Next.js.

The loading state applies to the route segment where the file is located and to its nested routes. Shared layouts remain visible and interactive while the new route content is loading.

By default, `loading.tsx` is a Server Component. It can also be converted into a Client Component by adding the `"use client"` directive when client-side hooks or interactions are required.

> This special file convention is available when using the Next.js App Router.
