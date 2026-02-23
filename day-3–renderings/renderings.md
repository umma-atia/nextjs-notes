## Day 3- Core concept of Renderings 

### 🔁 What is Rendering?
**Rendering** মানে: ইউজারের জন্য পেজ বা কনটেন্ট তৈরি করে দেখানো।  
সার্ভার বা ব্রাউজার ডেটা নিয়ে HTML বানায়।

Next.js-এ Rendering মানে হচ্ছে— পেজ/রুট **কীভাবে**, **কখন**, আর **কোথায়** HTML তৈরি হবে। App Router (নতুন রাউটার) কনসেপ্ট ধরে নিচে ক্লিয়ারভাবে বল হলো:

### 🔁 Next.js Rendering—কোর কনসেপ্ট
Next.js তিনটা লেয়ারে রেন্ডারিংকে কন্ট্রোল করতে দেয়:
1. **কখন** রেন্ডার হবে → build time না request time
2. **কোথায়** রেন্ডার হবে → server নাclient
3. **কিভাবে** রেন্ডার হবে → static, dynamic, streaming ইত্যাদি

### 🧱 Server vs Client Rendering
1.🖥️ Server Components (ডিফল্ট)
-App Router-এ সব কম্পোনেন্ট ডিফল্টভাবে Server Component
-সার্ভারে রেন্ডার হয় → কম JS পাঠায় → দ্রুত লোড
-ডেটাবেস/সিক্রেট সরাসরি ব্যবহার করা যায়
2.🌐 Client Components
-ব্রাউজারে রেন্ডার হয় (interactive UI দরকার হলে)
-'use client' লিখে ডিক্লেয়ার করতে হয়
-ইভেন্ট হ্যান্ডলার, state, browser API ব্যবহার করতে পারো

### 🕒 Static vs Dynamic Rendering
Next.js অটোভাবে বুঝে নেয় কোন রুট Static হবে, আর কোনটা Dynamic হবে—তুমি চাইলে ফোর্স করতেও পারো।
* 🧊 Static Rendering (SSG)
  -Build time-এ HTML তৈরি হয়
  -ব্লগ, ডকস, ল্যান্ডিং পেজের জন্য বেস্ট
  -ফাস্টেস্ট পারফরম্যান্স
```tsx
// ডিফল্টভাবে static হতে পারে যদি ডেটা স্টেবল হয়
export const revalidate = 3600; // ISR সহ static
```
* 🔥 Dynamic Rendering (SSR)
  -প্রতি request-এ HTML তৈরি হয়
  -কুকি, হেডার, ইউজার-স্পেসিফিক ডেটা লাগলে দরকার
```tsx
export const dynamic = 'force-dynamic';
<ins>নোট:</ins> cookies(), headers() ব্যবহার করলে Next.js নিজে থেকেই রুটকে dynamic ধরে নেয়।
```
* ♻️ ISR (Incremental Static Regeneration)
  -Static পেজ, কিন্তু নির্দিষ্ট সময় পর আবার রিজেনারেট হয়
  -“স্ট্যাটিক + আপডেটেড কনটেন্ট”—দুটোর বেস্ট
```tsx
export const revalidate = 60; // প্রতি 60 সেকেন্ডে নতুন করে বানাতে পারে
```
* 🌊 Streaming & Suspense (পার্শিয়াল রেন্ডার)
  -পুরো পেজ না বানিয়েই ধাপে ধাপে HTML পাঠায়
  -ভারী অংশ লোড হতে সময় নিলে ইউজার অপেক্ষা না করে বাকি অংশ দেখতে পায়
  -React Suspense দিয়ে কন্ট্রোল করা হয়
```tsx
<Suspense fallback={<Loading />}>
  <HeavyComponent />
</Suspense>
```
  - কখন দরকার?  
 ড্যাশবোর্ড, বড় ডেটা-ডিপেন্ডেন্ট UI—UX অনেক স্মুথ হয়।

### 🧠 Rendering কে কীভাবে কন্ট্রোল করবে (Route Segment Config)
Next.js-এ রুট-লেভেলে আচরণ ফিক্স করতে পারো:
```tsx
export const dynamic = 'force-static'; // জোর করে static
export const revalidate = 300;         // ISR টাইম
export const runtime = 'edge';         // edge-এ রেন্ডার
force-static → সবসময় static
force-dynamic → সবসময় dynamic
runtime = 'edge' → ইউজারের কাছাকাছি লোকেশনে দ্রুত রেন্ডার
```
### 🧩 কোন কেসে কোন Rendering?
📰 ব্লগ / ডকস → Static (SSG) + ISR
👤 ইউজার প্রোফাইল / ড্যাশবোর্ড → Dynamic (SSR)
📊 ভারী ডেটা পেজ → Streaming + Suspense
🎛️ ইন্টার‍্যাকটিভ UI → Client Components

### ✅ এক লাইনে:
Static = বিল্ড টাইমে বানাও (সবচেয়ে ফাস্ট)
Dynamic = request এ বানাও (পার্সোনালাইজড)
ISR = স্ট্যাটিক, কিন্তু সময়মতো আপডেট
Streaming = ধাপে ধাপে রেন্ডার (UX স্মুথ)
