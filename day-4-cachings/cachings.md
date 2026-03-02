# Next.js-এ Caching

Next.js-এ caching মানে হচ্ছে—ডেটা আর রেন্ডার করা আউটপুট (HTML/Flight data) কোথায়, কতক্ষণ, আর কোন নিয়মে সংরক্ষণ হবে—যাতে পারফরম্যান্স বাড়ে এবং অপ্রয়োজনীয় রি-ফেচ/রি-রেন্ডার না হয়। App Router কনটেক্সটে কোর কনসেপ্টগুলো 👇

---

## 🧠 Next.js Caching—কী কী ক্যাশ হয়?

Next.js মূলত তিন লেয়ারে ক্যাশিং করে:

### 1. Request Memoization
- একই রিকোয়েস্ট বা রেন্ডার এর ভিতরে একই fetch() বারবার হলে একবারই নেটওয়ার্ক কল যায়।
- অটো ডিডুপ্লিকেশন → পারফরম্যান্স বুস্ট।

### 2. Data Cache (fetch cache)
- fetch()-এর রেসপন্স সার্ভারে ক্যাশ হয়।
- Static/ISR রেন্ডারিংয়ের সাথে কাজ করে।
- রিভ্যালিডেশন দিয়ে ক্যাশ ইনভ্যালিডেট করা যায়।

### 3. Full Route Cache (Route Cache)
- পুরো রুটের রেন্ডার আউটপুট ক্যাশ হয়।
- Static routes হলে CDN/Edge-এ সার্ভ হয় → আল্ট্রা ফাস্ট।

---

## 🗂️ Data Cache কন্ট্রোল (fetch options)

অফিসিয়াল ডকস অনুযায়ী, fetch()-এর ক্যাশিং তুমি এভাবে কন্ট্রোল করতে পারো:

```typescript
// Default (static-compatible): ক্যাশ অন
const res = await fetch(url, { cache: 'force-cache' });
```

```typescript
// No cache: সবসময় ফ্রেশ ডেটা
const res = await fetch(url, { cache: 'no-store' });
```

```typescript
// Time-based revalidation (ISR-style)
const res = await fetch(url, { next: { revalidate: 60 } });
```

**ব্যাখ্যা:**
- `force-cache` → ক্যাশ থেকে সার্ভ করে (স্ট্যাটিকের সাথে কাজ করে)।
- `no-store` → ক্যাশ বাইপাস (ডাইনামিক ডেটার জন্য)।
- `next.revalidate` → নির্দিষ্ট সময় পর ক্যাশ রিফ্রেশ।

---

## ♻️ Cache Revalidation (Tag-based & Path-based)

ডাইনামিকভাবে ক্যাশ ইনভ্যালিডেট করতে পারো:

### 🏷️ Tag-based Revalidation
```typescript
// fetch করার সময় ট্যাগ দাও
await fetch(url, { next: { tags: ['products'] } });

// পরে যেকোনো সার্ভার অ্যাকশন বা রুট হ্যান্ডলার থেকে
revalidateTag('products');
```

### 🛣️ Path-based Revalidation
```typescript
revalidatePath('/products');
```
👉 ইউজার অ্যাকশন (যেমন: প্রোডাক্ট আপডেট) হলে নির্দিষ্ট ডেটা/পেজ রিফ্রেশ করাতে পারো।

---

## 🚫 কখন ক্যাশ কাজ করে না (Automatic Opt-out)

অফিসিয়াল ডকস অনুযায়ী, কিছু কন্ডিশনে Next.js নিজে থেকেই ক্যাশ বন্ধ করে দেয়:
- cookies() / headers() ব্যবহার করলে
- fetch(url, { cache: 'no-store' }) ব্যবহার করলে
- Route-কে `dynamic = 'force-dynamic'` করলে
- Authenticated / per-user ডেটা থাকলে

এগুলো থাকলে রুট ডাইনামিক রেন্ডারিং হয় এবং Data Cache/Route Cache এড়িয়ে যায়।

---

## 🌍 Edge/CDN Cache
- Static routes হলে আউটপুট CDN/Edge-এ ক্যাশ হয়।
- ফলে ইউজারের কাছের লোকেশন থেকে রেসপন্স যায় → লেটেন্সি কমে।
- Edge runtime ব্যবহার করলে গ্লোবালি দ্রুত ডেলিভারি হয়।

```typescript
export const runtime = 'edge';
```

---

## 🧩 কবে কোন ক্যাশিং স্ট্র্যাটেজি?

| স্ট্র্যাটেজি | উদাহরণ | বিবরণ |
|--------------|---------|---------|
| Blog / Docs | force-cache + revalidate | ISR-style ক্যাশিং |
| Product list | Tag-based revalidation | ক্যাশ ইনভ্যালিডেট হয় যখন ডেটা আপডেট হয় |
| User-specific data | no-store | পার্সোনাল ডেটা বা ক্যাশ বন্ধ |
| Admin updates | revalidateTag() / revalidatePath() | নির্দিষ্ট ডেটা বা পেজ রিফ্রেশ |

---

## ✅ TL;DR
- **Request Memoization** → একই রেন্ডারে ডুপ্লিকেট fetch আটকায়।
- **Data Cache** → API রেসপন্স ক্যাশ করে।
- **Route Cache** → পুরো পেজ আউটপুট ক্যাশ করে।
- **Revalidation** → সময় বা ইভেন্ট অনুযায়ী ক্যাশ রিফ্রেশ।
- **Automatic opt-out** → পার্সোনালাইজড ডেটায় ক্যাশ বন্ধ।

---

এভাবেই Next.js-এ ক্যাশিং ব্যবস্থাপনা পারফরম্যান্স বাড়াতে ও ডেটা আপডেটের জন্য গুরুত্বপূর্ণ।
