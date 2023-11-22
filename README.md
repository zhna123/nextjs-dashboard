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
* use vercel postgres
* seed initial data
7. Fetch data
* Different approaches to fetch data:
  - API (Route handlers)
  - DB queries (creating API or using react server component to query DB directly)
* SQL
* Request waterfall
* Parallel data fetching with `Promise.all()` / `Promise.allSettled()`
8. Static and dynamic rendering
* static rendering - data fetching and rendering happen on the server during build time/or revalidation, result can be cached
* dynamic rendering - render at request time
  - `unstable_noStore` API
  - slow data request problem
9. Streaming
1) page level - `loading.tsx`
2) component level - `<Suspense>`
  - group components together when we want multiple component to load at the same time
* suspense boundaries consideration (whole page, component, or section)
* Commonly, ove data fetching down to the components that need it
10. Partial rendering
* render a route with a static loading shell, while keeping some parts dynamic.(isolate the dynamic parts of a route)
11. Search and Pagination
* searchParams
  - use url search params: benefits compare to client side state
    * Bookmarkable and Shareable URLs
    * directly consumed on the server to render the initial state
    * easier to track user behavior 
  - useSearchParams (access current url params, eg: {page: '1', query: 'pending'})
* usePathname
  - read current url pathname
* useRouter
  - navigation between routes within client components
* Best practice - debouncing (`use-debounce` library): limits the rate at which a function can fire
12. Mutating data
* Server Actions - run asynchronous code directly on the server
* `action` attribute in `<form>` - receive `formData` (FormData type)
* next.js caching - revalidate cache using APIs like `revalidatePath` and `revalidateTag`
* Steps for adding:
  1. create new route and form
  2. create server action
    - `use server;` to mark all exported functions in the file as server functions
    - this is where to define the server actions for the form created
    - extract formData to create an object
    - validate data (using`Zod`)
      * note: storing money in cents
    - insert data to db
    - revalidate and redirect 
      * `revalidatePath('/dashboard/invoices');`
      * `redirect('/dashboard/invoices');`
* Updating
  1. dynamic route segment - create routes based on data, eg [id], [slug], [post]
  2. pass in id, and read id from page `params` - need to pre-populate data for the form
  3. fetch the invoice by id - pre-populate data for the form
  4. pass id to server action - pass function to `action` (use `bind`) attribute of the form
  5. create the update action
* Deleting
  1. update form to call server action(need to pass id using `bind`)
  2. create the delete action
13. handling errors
* try/catch
* `error.tsx`(client component) - define UI boundary, catch all unexpected errors, and display backup UI
* 404 errors
  - `notFound` function 
  - `not-found.tsx` file - UI
  - `notFound` will take precedence over `error.tsx`
14. Accessibility - server side form validation
* `eslint-plugin-jsx-a11y` plugin from next.js (didn't work for me as of 11/16/2023)
* form validation
  - client side 
  - server side
    * `useFormState` hook from `react-dom` (canary as of 11/16)
    * server action - use `Zod` to validate data
    * `safeParse()` - success or error
    * (pass `prevState: State` to the action function)
    * display errors in the form from `state`
15. Authentication - use `NextAuth.js`
  1) Create log in route (with log in Form)
  2) Set up `NextAuth.js`
    - `npm install next-auth@beta` (compitable wiht Next.js v14)
    - generate secret key `openssl rand -base64 32`
    - Add key to `.env` file, eg: `AUTH_SECRET=your-secret-key` (also update prod env variable)
  3) Adding page options
    - create `auth.config.ts` at the root, export `authConfig`
  4) Protect routes with middleware (logic to protect routes)
    - `authorized()` callback (used to verify if the request is authorized to access a page via Next.js Middleware)
    - `providers`
    - import `authConfig` into a middleware file, ie: create the `middleware.ts` at the root
      * initializing `NextAuth.js` with the `authConfig` object and exporting the `auth` property
  5) Password hashing - use `bcrypt`
    - create `auth.ts` at the root
    - add credential provider
    - add sign in functionality
      * use the `authorize` function to handle the authentication logic
      * use `zod` for validation
      * Query user from DB
      * use `bcrypt.compare` to check if passowrds match
  6) Update login form
    * connect the auth logic with login form
      - create a new action called `authenticate` in `actions.ts`
      - import `signIn` from `auth.ts`
    * `useFormState` and `useFormStatus` in `login-form.tsx`
  7) Add log out function
    * call the `signOut` function from `auth.ts `in the `<form>` element in `<SideNav />`
16. Adding metadata - next.js metadata api
  * detail about the page
  * SEO
  * usually in `<head>`
  * types of metadata
    - title metadata
    - keyword metadata
    - open graph metadata
    - favicon metadata
  * Add metadata
    - config based
    - file based
  * examples:
    - add `favicon` and `open graph` images
      * put `favicon.ico` and `opengraph-image.jpg` under root of `app` folder
    - add metadata object(including title, description, .etc) in `layout.tsx` or `page.tsx`
      * use `template`

