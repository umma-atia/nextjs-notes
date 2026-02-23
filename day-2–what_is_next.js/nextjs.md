# Day 2: Next.js – Core Concepts & Features

## 🔹 What is Next.js?
Next.js হলো React এর ওপর ভিত্তি করে তৈরি একটি ফ্রেমওয়ার্ক, যা আধুনিক ওয়েব অ্যাপ্লিকেশন তৈরি করার জন্য ব্যবহৃত হয়। এটি পারফরম্যান্স, SEO এবং ডেভেলপার এক্সপেরিয়েন্স অনেক সহজ করে দেয়।

## 🔹 Why Next.js?
React দিয়ে অ্যাপ বানালে অনেক কনফিগারেশন নিজে করতে হয়।  
Next.js সেই কাজগুলো built-in ভাবে হ্যান্ডেল করে, ফলে ডেভেলপমেন্ট সহজ হয়।

## 🔹 Core Features of Next.js

### 1️⃣ Server Side Rendering (SSR)
- প্রতিটি request এর সময় server থেকে HTML তৈরি হয়, যা seo-প্রিয়।
- এটি দ্রুত প্রথম লোডের জন্য সহায়ক।

```javascript
export async function getServerSideProps() {
  return { props: { data: "Hello" } }
}

### 2️⃣ Static Site Generation (SSG)
build এর সময়েই HTML তৈরি হয়, ফলে পারফরম্যান্স খুবই দ্রুত হয়।
বিশেষ করে ব্লগ, ল্যান্ডিং পেজের জন্য উপযুক্ত।
কোড উদাহরণ:
```javascript
export async function getStaticProps() {
  return { props: { posts: [] } }
}
### 3️⃣ File-based Routing
pages ফোল্ডারের ফাইল থেকে সরাসরি route তৈরি হয়।
আলাদা router configuration এর দরকার হয় না।
উদাহরণ:
pages/index.js → /
pages/about.js → /about
### 4️⃣ API Routes
Next.js দিয়ে backend API তৈরি করা যায়।
pages/api ফোল্ডার ব্যবহার করে API endpoints তৈরি হয়।
উদাহরণ:
pages/api/user.js → /api/user
### 5️⃣ Built-in Performance Optimization
অটোমেটিক কোড স্প্লিটিং
ইমেজ অপ্টিমাইজেশন (next/image)
ফাস্ট রিফ্রেশ (Fast Refresh) সাপোর্ট
### 6️⃣ SEO Support
<Head> কম্পোনেন্ট ব্যবহার করে meta tags সেট করা যায়।
উদাহরণ:
```
import Head from "next/head"

## 🔹 When to Use Next.js?
-SEO দরকার হলে
-দ্রুত লোড হওয়া ওয়েবসাইট বানাতে চাইলে
-React অ্যাপে স্ট্রাকচার এবং পারফরম্যান্স উন্নত করতে চাইলে
-ফুল-স্ট্যাক (ফ্রন্টএন্ড + API) অ্যাপ তৈরি করতে চাইলে

## Summary (Day 2 Learning)
Next.js হলো React এর উপর ভিত্তি করে তৈরি একটি শক্তিশালী ফ্রেমওয়ার্ক, যা:

-SSR এবং SSG এর মাধ্যমে পারফরম্যান্স উন্নত করে
-Routing সহজ করে
-API তৈরি সম্ভব করে
-SEO এবং পারফরম্যান্সে উন্নত
-Production-ready React framework
