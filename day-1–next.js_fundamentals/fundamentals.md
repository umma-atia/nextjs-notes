# Daily Learning Log

## Day 1 – Next.js Fundamentals

### 1. Next.js মূল ধারণা ও সুবিধা
Next.js হলো React-এর ওপর ভিত্তি করে তৈরি একটি powerful framework, যা modern web application development-কে অনেক সহজ করে তোলে। সাধারণ React অ্যাপে যেখানে rendering, SEO, performance optimization আলাদাভাবে handle করতে হয়, Next.js সেখানে built-in solution দেয়।

**Key Benefits:**
- Automatic rendering → পেজ দ্রুত লোড হয়
- SEO-friendly → Server থেকে pre-render হওয়া HTML পাওয়ায় search engine ভালোভাবে index করতে পারে
- Easy development → structure ও convention থাকার কারণে code maintain করা সহজ
- Built-in features → SSG, dynamic routing, async data loading ইত্যাদি

সংক্ষেপে, Next.js দিয়ে দ্রুত, scalable এবং SEO-optimized web app তৈরি করা যায়।

---

### 2. Rendering পদ্ধতি
Web page render করার প্রধান তিনটি পদ্ধতি সম্পর্কে ধারণা পেয়েছি:

- **Client-Side Rendering (CSR):**
  Browser নিজে data fetch করে page তৈরি করে। First load ধীর হতে পারে, কিন্তু highly interactive app-এর জন্য ভালো।

- **Server-Side Rendering (SSR):**
  Server থেকে ready HTML আসে। First load দ্রুত হয় এবং SEO-এর জন্য উপযোগী।

- **React Server Components (RSC):**
  কিছু component server-এ, কিছু client-এ render হয়। এতে performance ও data loading efficiency বাড়ে।

👉 SEO দরকার হলে SSR/RSC ভালো, আর highly dynamic interaction-এর জন্য CSR উপযুক্ত।

---

### 3. Project Setup ও Structure
Next.js project শুরু করার basic ধারণা:

Common folder structure:
- `pages/` → application routes/pages
- `components/` → reusable UI components
- `public/` → images, fonts, static assets
- `layouts/` → common page structure/layout

Pages ও layouts কিভাবে একসাথে কাজ করে, সেটার overview পেয়েছি।

---

### 4. Routing ও Navigation
Routing মানে বিভিন্ন URL-এর মাধ্যমে বিভিন্ন page access করা।

- Nested routing → multi-level pages
- Dynamic routing → URL parameter ব্যবহার (`/product/123`)
- Custom 404 page → page না থাকলে user-friendly error page

Next.js-এ file-based routing থাকায় routing অনেক সহজ।

---

### 5. Error Handling ও Loading States
User experience ভালো রাখার জন্য:
- Error handling → সমস্যা হলে user-কে clear message দেখানো
- Loading states → data load হওয়ার সময় spinner/loader দেখানো

এতে application আরও user-friendly হয়।

---

### 6. Component-এর ধরণ
Next.js-এ দুই ধরনের component:

- **Server Components:**
  Server-এ run করে, data-heavy UI-এর জন্য ভালো, performance বাড়ায়।

- **Client Components:**
  Browser-এ run করে, user interaction (click, form, animation) এর জন্য দরকার।

👉 Data display → Server Component
👉 Interaction → Client Component

---

### 7. Performance Optimization
Performance বাড়ানোর জন্য:
- Image optimization → optimized image loading
- Font optimization → faster text rendering
- Metadata → SEO improve করার জন্য title, description

এসব feature Next.js-এ built-in থাকায় optimization সহজ।

---

### 8. Data Fetching ও Progressive Rendering
Data আনার পদ্ধতি:
- **SSG:** build time-এ data fetch
- **SSR:** request অনুযায়ী data fetch
- **Client-side fetching:** browser থেকে API call

Progressive rendering → আগে basic UI, পরে full content load → perceived performance বাড়ে।

---

### 9. Advanced Rendering Concepts
- SSR → dynamic content
- SSG → static content (blog)
- ISR → static page incremental update

Use case অনুযায়ী rendering method নির্বাচন করলে performance ও scalability দুটোই ভালো হয়।

---
