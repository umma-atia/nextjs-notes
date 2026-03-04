# 📘 Next.js Project Structure & Organization (Folder & File Conventions) 

---
## 🔝 Top-Level Folders

এগুলো দিয়ে অ্যাপের কোড ও স্ট্যাটিক ফাইল সাজানো হয়।

| ফোল্ডার | কাজ |
|----------|------|
| app | App Router (নতুন রাউটিং সিস্টেম) |
| pages | Pages Router (পুরনো রাউটিং সিস্টেম) |
| public | Static assets (image, favicon ইত্যাদি) |
| src | ঐচ্ছিক সোর্স কোড ফোল্ডার |

👉 ফোল্ডারের নাম অনুযায়ী URL তৈরি হয়।

---

## 📄 Top-Level Files

এই ফাইলগুলো প্রজেক্ট কনফিগার, ডিপেন্ডেন্সি ম্যানেজ, environment variable সেট করার জন্য ব্যবহৃত হয়।

| ফাইল | কাজ |
|------|------|
| next.config.js | Next.js কনফিগারেশন |
| package.json | ডিপেন্ডেন্সি ও স্ক্রিপ্ট |
| instrumentation.ts | Monitoring / OpenTelemetry |
| proxy.ts | Request proxy |
| .env | Environment variables |
| eslint.config.mjs | ESLint কনফিগ |
| tsconfig.json | TypeScript কনফিগ |
| jsconfig.json | JavaScript কনফিগ |
| .gitignore | Git কোন ফাইল ট্র্যাক করবে না |

⚠️ `.env` ফাইল কখনো version control-এ রাখা উচিত নয়।

---

## 🌐 Routing Files

| ফাইল | কাজ |
|------|------|
| layout.js | Shared UI (header, footer) |
| page.js | Route পাবলিক করে |
| loading.js | Loading UI |
| error.js | Error UI |
| not-found.js | 404 UI |
| route.js | API endpoint |
| template.js | Re-rendered layout |

👉 `page.js` বা `route.js` না থাকলে route পাবলিক হবে না।

---

## 📂 Nested Routes

ফোল্ডারের ভিতরে ফোল্ডার মানেই nested URL।

উদাহরণ:


app/page.tsx → /
app/blog/page.tsx → /blog
app/blog/authors/page.tsx → /blog/authors


👉 Parent `layout.js` child route-গুলোকে wrap করে।

---

## 🔄 Dynamic Routes

Square bracket ব্যবহার করা হয়।

| Pattern | কাজ |
|----------|------|
| [slug] | একক প্যারামিটার |
| [...slug] | Catch-all |
| [[...slug]] | Optional catch-all |

উদাহরণ:


app/blog/[slug]/page.tsx
→ /blog/my-first-post


---

## 🧩 Component Hierarchy

Render হয় এই ক্রমে:


layout.js
template.js
error.js
loading.js
not-found.js
page.js


Nested route হলে parent layout-এর ভিতরে child render হয়।

---

## 📌 Colocation

`app` ফোল্ডারে ফাইল রাখলেই রাউট হয়ে যাবে — এমন নয়।

🔑 গুরুত্বপূর্ণ বিষয়:

- `page.js` বা `route.js` না থাকলে route পাবলিক হবে না
- শুধু `page.js` / `route.js`-এর return করা অংশ ক্লায়েন্টে যাবে
- তাই `_components` বা `_lib` নিরাপদ

তুমি চাইলে `app`-এর বাইরে ফাইল রাখতেও পারো।

---

## 🔒 Private Folders

Private folder বানাতে হলে ফোল্ডারের নামের আগে `_` দিতে হয়।


_folderName


### 🔎 এর মানে

- Internal implementation
- Routing system এটিকে গণ্য করবে না
- সব subfolder-সহ routing থেকে বাদ যাবে

---

### ✅ কেন ব্যবহার করবেন?

- UI logic ও routing logic আলাদা রাখতে
- Internal ফাইল সাজাতে
- Naming conflict এড়াতে
- কোড এডিটরে ভালোভাবে organize করতে

---

### 🧠 ভালো করে জানুন

- `_` দিয়ে private ফাইল মার্ক করা framework rule নয়, শুধু convention
- `_` দিয়ে শুরু হওয়া URL segment বানাতে চাইলে `%5FfolderName` ব্যবহার করতে হবে
- Private folder ব্যবহার না করলে special file naming ভালোভাবে জানা জরুরি

---

## 📦 Route Groups

Route group বানাতে ফোল্ডারের নাম `()` এর ভিতরে লিখতে হয়।


(folderName)


### 🔎 এর মানে

- শুধু organization-এর জন্য
- URL-এ এই নাম দেখাবে না

---

### 🎯 কেন ব্যবহার করবেন?

- marketing / admin / shop route আলাদা রাখতে
- একই level-এ multiple layout বানাতে
- নির্দিষ্ট route-এ layout প্রয়োগ করতে

---

## 📁 src Folder

চাইলে অ্যাপ কোড `src` ফোল্ডারের ভিতরে রাখতে পারো।

### সুবিধা:

- Root-এ config file
- `src`-এ application code
- Project পরিষ্কার দেখায়

---

## 🏗 Organization Strategy

Next.js কড়া নিয়ম দেয় না।  
Consistency-ই সবচেয়ে গুরুত্বপূর্ণ।

---

## ✅ Strategy 1: app-এর বাইরে project files


app/
components/
lib/


✔ বড় প্রজেক্টে পরিষ্কার separation

---

## ✅ Strategy 2: app-এর ভিতরে project files


app/
components/
lib/
blog/


✔ মাঝারি প্রজেক্টে সুবিধাজনক

---

## ✅ Strategy 3: Feature অনুযায়ী ভাগ


app/
blog/
page.js
_components/
shop/
page.js
_components/


✔ Scalable structure

---

## 🎨 Multiple Layout


(marketing)/layout.js
(shop)/layout.js


দুই সেকশনের UI সম্পূর্ণ আলাদা করা যায়।

---

## ⏳ নির্দিষ্ট Route-এ Loading


dashboard/
(overview)/
loading.tsx
page.tsx


শুধু `/dashboard/overview`-এ loading কাজ করবে।

---

## 🏠 Multiple Root Layout

1. Root `layout.js` সরিয়ে ফেলো
2. প্রতিটি route group-এ `layout.js` দাও
3. `<html>` ও `<body>` প্রতিটি root layout-এ থাকতে হবে

---

## 🔎 সহজ সারাংশ

| Feature | কাজ |
|----------|------|
| page.js | Route চালু করে |
| layout.js | Wrapper |
| [slug] | Dynamic route |
| _folder | Private |
| (folder) | Route group |
| src | আলাদা source directory |

---

## 📌 শেষ কথা

✔ একটি structure বেছে নাও  
✔ পুরো প্রজেক্টে consistent থাকো  
✔ Routing পরিষ্কার রাখো  
✔ UI ও logic আলাদা রাখো  
