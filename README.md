## Next.js App Router Course - Final

This is the final template for the Next.js App Router Course. It contains the final code for the dashboard application.

For more information, see the [course curriculum](https://nextjs.org/learn) on the Next.js Website.

# Learning notes
2. CSS styling
* global styles - global.css
* Tailwind
* CSS modules - home.module.css
* clsx library - conditionally style elements
3. Fonts and Images - optimization
* next/font
* next/image
4. Layout and Pages
* page.tsx - exported react component contains UI for the route
* layout - pages or another layout automatically nested inside layout
* root layout - required `/app/layout.tsx`
5. Navigating
* next/link - `Link` - client side navigation - fast without refresh
* show active links - `usePathname()` hook from 'next/navigation'
* code spliting and prefetching
  - code split by route segment - isolated page
  - prefetch code for linked route when the `Link` component shows up in viewport(save in `Router Cache`)
6. Set up DB