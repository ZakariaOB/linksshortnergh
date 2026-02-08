# Dark Mode Fix - Technical Explanation

## Problem
The dark mode toggle button had no effect when clicked. The ThemeProvider was correctly adding/removing the `.dark` class on the `<html>` element, but the Tailwind CSS `dark:` variant classes were not responding to the class changes.

## Root Cause
Tailwind CSS 4 uses **media query-based dark mode by default** (via `@media (prefers-color-scheme: dark)`). This means the `dark:` variant classes only respond to the system's color scheme preference, not to a manually toggled `.dark` class.

## Solution
Configure Tailwind CSS 4 to use **class-based dark mode** by adding the `@variant` directive to `app/globals.css`:

```css
@variant dark (.dark *);
```

This directive tells Tailwind CSS that `dark:` utilities should apply when any ancestor element has the `.dark` class.

### How it works:
1. User clicks the theme toggle button
2. ThemeProvider's `setTheme()` function is called
3. The `useEffect` in ThemeProvider adds/removes the `.dark` class on `document.documentElement` (the `<html>` tag)
4. With the `@variant` directive in place, Tailwind CSS now recognizes the `.dark` class and applies all `dark:` variant styles
5. All elements with `dark:` classes (e.g., `dark:bg-zinc-900`, `dark:text-zinc-50`) now respond to the toggle

## Files Modified
- `app/globals.css` - Added `@variant dark (.dark *);` directive

## Verification
See the screenshots showing the dark mode toggle working correctly:
- Light mode: Cards with white backgrounds and light text
- Dark mode: Cards with dark backgrounds and light text

The toggle successfully adds/removes the `.dark` class and all Tailwind `dark:` variant styles are applied accordingly.
