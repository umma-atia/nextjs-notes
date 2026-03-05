# Next.js Layouts and Pages:

এই ডকুমেন্টে **Next.js App Router** ব্যবহার করে কীভাবে **Pages, Layouts, Nested Routes, Dynamic Routes এবং Navigation** তৈরি করতে হয় তা সহজ ভাষায় ব্যাখ্যা করা হয়েছে।

---

## 📦 Next.js Routing System

Next.js একটি **File-System Based Routing** ব্যবহার করে।

অর্থাৎ:

* **Folder → URL Segment**
* **File → UI Component**

উদাহরণ:

```
app/
 ├ page.js
 ├ blog/
 │   ├ page.js
 │   └ [slug]/
 │       └ page.js
```

Routes হবে:

```
/
/blog
/blog/post-1
/blog/post-2
```

---

## 📄 Creating a Page

**Page** হচ্ছে এমন একটি UI যা একটি নির্দিষ্ট URL route এ render হয়।

Page তৈরি করার জন্য:

1. `app` directory এর ভিতরে `page.js` তৈরি করুন
2. একটি React component default export করুন

### Example

```
app/page.js
```

```javascript
export default function Page() {
  return <h1>Hello Next.js!</h1>
}
```

Route:

```
/
```

এটি আপনার **Homepage**।

---

## 🧱 Creating a Layout

**Layout** হলো এমন UI যা একাধিক page এর মধ্যে shared থাকে।

উদাহরণ:

* Navbar
* Sidebar
* Footer

Layout এর সুবিধা:

* Navigation এর সময় state preserve থাকে
* Layout re-render হয় না
* Performance ভালো হয়

Layout তৈরি করতে `layout.js` ফাইল ব্যবহার করা হয়।

### Example

```
app/layout.js
```

```javascript
export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <main>{children}</main>
      </body>
    </html>
  )
}
```

### Important

* `children` এর জায়গায় page render হয়
* Root layout এ অবশ্যই থাকতে হবে:

```
<html>
<body>
```

---

## 🌳 Nested Routes

Nested route মানে URL এর ভিতরে একাধিক segment।

উদাহরণ:

```
/blog/post-1
```

Segments:

```
/        → Root
blog     → Segment
post-1   → Leaf Segment
```

Folder structure:

```
app/
 ├ blog/
 │   └ page.js
```

Route:

```
/blog
```

---

## 🪺 Creating Nested Routes

Nested folder ব্যবহার করে nested route তৈরি করা যায়।

Example:

```
app/
 ├ blog/
 │   ├ page.js
 │   └ [slug]/
 │        └ page.js
```

Routes:

```
/blog
/blog/post-1
/blog/post-2
```

---

## ⚡ Dynamic Routes

Dynamic route ব্যবহার করে data অনুযায়ী multiple page তৈরি করা যায়।

উদাহরণ:

```
blog/post-1
blog/post-2
blog/post-3
```

Dynamic route তৈরি করতে folder নাম **square bracket** এর ভিতরে লিখতে হয়।

```
[slug]
```

Example:

```
app/blog/[slug]/page.js
```

```javascript
export default async function BlogPostPage({ params }) {
  const { slug } = params

  return (
    <div>
      <h1>Blog Post: {slug}</h1>
    </div>
  )
}
```

যদি URL হয়:

```
/blog/react
```

তাহলে:

```
slug = react
```

---

## 🧩 Nested Layouts

Layout একটার ভিতরে আরেকটা থাকতে পারে।

Structure:

```
app/
 ├ layout.js
 ├ blog/
 │   ├ layout.js
 │   └ page.js
```

Rendering flow:

```
Root Layout
   ↓
Blog Layout
   ↓
Blog Page
```

Example:

```
app/blog/layout.js
```

```javascript
export default function BlogLayout({ children }) {
  return <section>{children}</section>
}
```

---

## 🔎 Using Search Params

Server Component এ query parameter ব্যবহার করা যায়।

Example URL:

```
/?page=2&filter=react
```

Example code:

```javascript
export default async function Page({ searchParams }) {
  const filters = searchParams.filters
}
```

Use cases:

* Pagination
* Filtering
* Search

---

## 🧑‍💻 Client Side Search Params

Client component এ search params ব্যবহার করতে হয়:

```
useSearchParams()
```

Example:

```javascript
"use client"

import { useSearchParams } from 'next/navigation'

export default function Filter() {
  const searchParams = useSearchParams()
}
```

---

## 🔗 Linking Between Pages

Page navigation এর জন্য ব্যবহার করা হয় `<Link>` component।

Import:

```
next/link
```

Example:

```javascript
import Link from 'next/link'

export default function Post({ post }) {
  return (
    <li>
      <Link href={`/blog/${post.slug}`}>
        {post.title}
      </Link>
    </li>
  )
}
```

Features:

* Client-side navigation
* Prefetching
* Faster page loading

---

## 🧰 Route Props Helpers

Next.js কিছু helper type দেয়।

### PageProps

Page component এর props।

Includes:

* params
* searchParams

Example:

```typescript
export default async function Page(props: PageProps<'/blog/[slug]'>) {
  const { slug } = props.params
}
```

---

### LayoutProps

Layout component এর props।

Includes:

* children
* named slots

Example:

```typescript
export default function Layout(props: LayoutProps<'/dashboard'>) {
  return (
    <section>
      {props.children}
    </section>
  )
}
```

---

## ℹ️ Good to Know

* Static route হলে `params = {}`
* `PageProps` এবং `LayoutProps` globally available
* Import করার প্রয়োজন নেই

Types generate হয় যখন:

```
next dev
next build
next typegen
```

---

## 📚 Summary

Next.js App Router এ routing system খুব সহজ:

| Concept   | Description   |
| --------- | ------------- |
| Folder    | URL route     |
| page.js   | Page UI       |
| layout.js | Shared UI     |
| [slug]    | Dynamic route |
| Link      | Navigation    |

---

## 🚀 Example Folder Structure

```
app
 ├ layout.js
 ├ page.js
 ├ blog
 │   ├ layout.js
 │   ├ page.js
 │   └ [slug]
 │        └ page.js
```

Routes:

```
/
/blog
/blog/post-1
/blog/post-2
```

---
