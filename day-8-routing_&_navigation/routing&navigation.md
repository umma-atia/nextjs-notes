# Routing and Navigation:
Routing মানে, কিভাবে ওয়েবসাইটের বিভিন্ন পাতায় যাওয়া যায়। 

- Nested Routing: একাধিক স্তরের পেজ বা সাব-অ্যাপ্লিকেশন তৈরি করা।
- Dynamic Routing: URL এর অংশ পরিবর্তন করে বিভিন্ন ডেটা দেখানো, যেমন /product/123 বা /blog/অ্যাডভেঞ্চার।
- 404 page: যদি কোন পেজ না থাকে, তখন "Not Found" দেখানো হয়। সেটি কিভাবে কাস্টমাইজ করবেন, তা শেখানো হয়েছে।

অর্থাৎ, কিভাবে ওয়েবসাইটের ভিতরে বিভিন্ন পেজ বা ডেটা অ্যাক্সেস করবেন, সেটি কিভাবে কাজ করে, তা এখানে বোঝানো হয়েছে।

Next.js-এ সাধারণত routes (pages) **server এ render হয়**। তাই নতুন page দেখানোর আগে client কে server response এর জন্য অপেক্ষা করতে হয়।

Next.js navigation দ্রুত করার জন্য কিছু built-in optimization ব্যবহার করে:

- Server Rendering
- Prefetching
- Streaming
- Client-side transitions

---

## 1. Server Rendering

Next.js এ **Layouts এবং Pages ডিফল্টভাবে React Server Components**।

User যখন নতুন page এ যায়:

Server → page render করে → client এ পাঠায় → browser দেখায়

## Server Rendering দুই ধরনের

### Static Rendering (Prerendering)
- Build time এ generate হয়
- Result cache হয়
- খুব দ্রুত load হয়

### Dynamic Rendering
- Request করার সময় server render করে
- Data dynamic হলে ব্যবহার হয়

---

## 2. Prefetching

**Prefetching মানে:** user click করার আগেই next page background এ load করা।

Next.js এ `<Link>` component ব্যবহার করলে automatic prefetch হয়।

```jsx
import Link from 'next/link'

<Link href="/blog">Blog</Link>
```

Link viewport এ আসলে prefetch হয়  
hover করলেও prefetch হয়

Static Route:  
পুরো page prefetch হয়

Dynamic Route:  
সাধারণত prefetch skip হয় বা partial হয়

## 3. Streaming

Streaming এর মাধ্যমে server পুরো page ready হওয়ার আগেই কিছু অংশ client এ পাঠায়।

এর জন্য route folder এ loading.tsx ব্যবহার করা হয়।
```
export default function Loading() {
  return <LoadingSkeleton />
}
```
Next.js automatically <Suspense> ব্যবহার করে loading UI দেখায়।

সুবিধা:  
- User সাথে সাথে feedback পায়
- Layout interactive থাকে
- Performance improve হয়

## 4. Client-side Transitions

- পুরনো server rendered website এ navigation করলে full page reload হয়।
- Next.js <Link> ব্যবহার করে client-side navigation করে।

এর ফলে:  
- page reload হয় না
- layout একই থাকে
- শুধু content update হয়

Example:  
```
<Link href="/blog">Blog</Link>
```
## 5. Navigation Slow হওয়ার কারণ
1. Dynamic route কিন্তু loading.tsx নেই  
User কে server response এর জন্য wait করতে হয়।

Solution:
```
app/blog/[slug]/loading.tsx  
```

2. generateStaticParams নেই  
Dynamic route build time এ generate না হলে request time এ render হয়।
Example:
```
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then(res => res.json())

  return posts.map((post) => ({
    slug: post.slug
  }))
}
```
3. Slow Network  
Prefetch complete হওয়ার আগেই user click করলে delay হতে পারে।  
Solution: useLinkStatus ব্যবহার করে loading indicator দেখানো।

## 6. Prefetch Disable করা  
অনেক link থাকলে prefetch disable করা যায়।
```
<Link prefetch={false} href="/blog">
  Blog
</Link>
```
## 7. Hover Prefetch  
সব link prefetch না করে শুধু hover করলে prefetch করা যায়।
```
<Link
  href="/blog"
  prefetch={active ? null : false}
  onMouseEnter={() => setActive(true)}
>
  Blog
</Link>
```
## 8. Hydration Issue  
<Link> একটি Client Component।  
Hydration complete না হলে prefetch শুরু হয় না।
Solution:

Bundle size কমানো  
Server Components বেশি ব্যবহার করা  

## 9. Native History API  
Next.js এ browser history manually update করা যায়।  
pushState:
নতুন history entry add করে।
```
window.history.pushState(null, '', '?sort=asc')
```
replaceState:
Current history replace করে।
```
window.history.replaceState(null, '', '/en/about')
```
## Summary:
- Next.js navigation fast করার জন্য ব্যবহার করে  
- Prefetching → আগে থেকে page load করা
- Streaming → page অংশ অংশ করে পাঠানো
- Client-side transitions → reload ছাড়া navigation
- loading.tsx → loading UI দেখানো

এগুলোর কারণে Next.js app SPA এর মতো fast feel দেয়, যদিও এটি server-rendered।
