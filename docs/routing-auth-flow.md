# Routing & Authentication Flow

## Authentication Provider

**CRITICAL**: All authentication is handled exclusively by **Clerk**. Do not implement any other authentication method.

## Protected Routes

### Dashboard Protection

The `/dashboard` route requires authentication. Protect it using middleware:

```typescript
// middleware.ts
import { clerkMiddleware, createRouteMatcher } from '@clerk/nextjs/server';

const isProtectedRoute = createRouteMatcher(['/dashboard(.*)']);

export default clerkMiddleware(async (auth, request) => {
  if (isProtectedRoute(request)) {
    await auth.protect();
  }
});

export const config = {
  matcher: [
    '/((?!_next|[^?]*\\.(?:html?|css|js(?!on)|jpe?g|webp|png|gif|svg|ttf|woff2?|ico|csv|docx?|xlsx?|zip|webmanifest)).*)',
    '/(api|trpc)(.*)',
  ],
};
```

Alternatively, check in the page component:

```typescript
// app/dashboard/page.tsx
import { auth } from '@clerk/nextjs/server';
import { redirect } from 'next/navigation';

export default async function DashboardPage() {
  const { userId } = await auth();
  
  if (!userId) {
    redirect('/');
  }

  return <div>Dashboard Content</div>;
}
```

## Homepage Redirect

Authenticated users accessing `/` should be redirected to `/dashboard`:

```typescript
// app/page.tsx
import { auth } from '@clerk/nextjs/server';
import { redirect } from 'next/navigation';

export default async function HomePage() {
  const { userId } = await auth();
  
  if (userId) {
    redirect('/dashboard');
  }

  return (
    <div>
      <h1>Welcome to Link Shortener</h1>
      {/* Landing page for unauthenticated users */}
    </div>
  );
}
```

## Sign-In/Sign-Up Modals

**CRITICAL**: Always use `mode="modal"` for sign-in and sign-up. Never create dedicated sign-in/sign-up pages.

```typescript
// app/components/Header.tsx
'use client';

import { SignInButton, SignUpButton, SignedIn, SignedOut, UserButton } from '@clerk/nextjs';

export function Header() {
  return (
    <header>
      <nav>
        <SignedOut>
          <SignInButton mode="modal">
            <button>Sign In</button>
          </SignInButton>
          
          <SignUpButton mode="modal">
            <button>Sign Up</button>
          </SignUpButton>
        </SignedOut>
        
        <SignedIn>
          <UserButton afterSignOutUrl="/" />
        </SignedIn>
      </nav>
    </header>
  );
}
```

## Environment Configuration

```bash
# .env.local

# ✅ Required
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_SECRET_KEY=sk_test_...

# ✅ Set after-auth redirects
NEXT_PUBLIC_CLERK_AFTER_SIGN_IN_URL=/dashboard
NEXT_PUBLIC_CLERK_AFTER_SIGN_UP_URL=/dashboard

# ❌ Do NOT set these (modal mode doesn't need them)
# NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in
# NEXT_PUBLIC_CLERK_SIGN_UP_URL=/sign-up
```

## Rules Summary

1. ✅ Use **Clerk only** for authentication
2. ✅ `/dashboard` requires authentication
3. ✅ Redirect authenticated users from `/` to `/dashboard`
4. ✅ Sign-in/sign-up use `mode="modal"`
5. ❌ Never create `/sign-in` or `/sign-up` page routes
6. ❌ Never implement custom authentication
