# Next.js Error Handling & Loading States: 

এখানে **Next.js application-এ কিভাবে Error Handle করতে হয়** তা বিস্তারিতভাবে ব্যাখ্যা করা হয়েছে।

Next.js-এ সাধারণত Error দুই ধরনের হয়:

1. **Expected Errors** (স্বাভাবিকভাবে ঘটতে পারে এমন Error)
2. **Uncaught Exceptions** (অপ্রত্যাশিত Bug বা Crash)

---

## 1. Expected Errors

Expected Error হলো এমন Error যেগুলো application ব্যবহার করার সময় স্বাভাবিকভাবে ঘটতে পারে।

### উদাহরণ

* Form validation fail
* API request fail
* Database response error

এই ধরনের Error সাধারণত **explicitly handle করে client-এ return করা উচিত।**

---

## Server Functions এ Expected Error Handle করা

Server Function ব্যবহার করলে **React hook `useActionState`** দিয়ে Error handle করা যায়।

গুরুত্বপূর্ণ নিয়ম:

❌ Expected Error এর জন্য `try/catch` ব্যবহার করা উচিত না
❌ `throw error` করা উচিত না

✅ Error কে **return value হিসেবে পাঠাতে হবে**

---

## Example: Server Action

```javascript
'use server'

export async function createPost(prevState, formData) {
  const title = formData.get('title')
  const content = formData.get('content')

  const res = await fetch('https://api.vercel.app/posts', {
    method: 'POST',
    body: { title, content },
  })

  const json = await res.json()

  if (!res.ok) {
    return { message: 'Failed to create post' }
  }
}
```

### এখানে কী হচ্ছে?

1. Form থেকে `title` এবং `content` নেওয়া হচ্ছে
2. API-তে POST request পাঠানো হচ্ছে
3. যদি request fail হয় তাহলে error message return করা হচ্ছে

```
return { message: 'Failed to create post' }
```

---

## Client Component এ Error দেখানো

Client component-এ `useActionState` ব্যবহার করে UI-তে error দেখানো যায়।

```javascript
'use client'

import { useActionState } from 'react'
import { createPost } from '@/app/actions'

const initialState = {
  message: '',
}

export function Form() {
  const [state, formAction, pending] = useActionState(createPost, initialState)

  return (
    <form action={formAction}>
      <label htmlFor="title">Title</label>
      <input type="text" id="title" name="title" required />

      <label htmlFor="content">Content</label>
      <textarea id="content" name="content" required />

      {state?.message && <p aria-live="polite">{state.message}</p>}

      <button disabled={pending}>Create Post</button>
    </form>
  )
}
```

### `useActionState` কী return করে?

```
state
formAction
pending
```

| Value      | কাজ                                   |
| ---------- | ------------------------------------- |
| state      | server থেকে আসা data বা error message |
| formAction | form submit handle করে                |
| pending    | loading state                         |

---

## Server Component এ Error Handle করা

Server Component-এ data fetch করার সময় response দেখে error handle করা যায়।

```javascript
export default async function Page() {
  const res = await fetch(`https://...`)
  const data = await res.json()

  if (!res.ok) {
    return 'There was an error.'
  }

  return '...'
}
```

যদি API request fail হয় তাহলে error message return করা হবে।

---

## 404 Page Handle করা (notFound)

Next.js-এ যদি কোন data না পাওয়া যায় তাহলে `notFound()` function ব্যবহার করা হয়।

---

## Example

```javascript
import { getPostBySlug } from '@/lib/posts'

export default async function Page({ params }) {
  const { slug } = await params
  const post = getPostBySlug(slug)

  if (!post) {
    notFound()
  }

  return <div>{post.title}</div>
}
```

যদি post না থাকে তাহলে `notFound()` call হবে।

---

## 404 UI

```javascript
export default function NotFound() {
  return <div>404 - Page Not Found</div>
}
```

---

## 2. Uncaught Exceptions

Uncaught Exceptions হলো এমন Error যা **unexpected bug বা crash নির্দেশ করে।**

উদাহরণ:

* undefined variable
* runtime error
* component crash

এই ধরনের error সাধারণত `throw` করা হয় এবং Next.js এগুলো **Error Boundary** দিয়ে catch করে।

---

## Error Boundary

Error Boundary হলো React component যা:

* Child component-এর error catch করে
* Fallback UI দেখায়
* পুরো application crash হওয়া থেকে বাঁচায়

---

## Error Boundary তৈরি করা

Route segment-এর ভিতরে `error.js` ফাইল তৈরি করতে হবে।

```
app/dashboard/error.js
```

---

## Example

```javascript
'use client'

import { useEffect } from 'react'

export default function ErrorPage({ error, reset }) {
  useEffect(() => {
    console.error(error)
  }, [error])

  return (
    <div>
      <h2>Something went wrong!</h2>

      <button onClick={() => reset()}>
        Try again
      </button>
    </div>
  )
}
```

### এখানে কী হচ্ছে?

| Property | কাজ                                   |
| -------- | ------------------------------------- |
| error    | ধরা পড়া error object                 |
| reset()  | component আবার render করার চেষ্টা করে |

---

## Nested Error Boundaries

Next.js-এ Error **nearest parent error boundary-তে bubble up হয়।**

উদাহরণ:

```
app
 ├ dashboard
 │   ├ page.js
 │   └ error.js
```

যদি dashboard page-এ error হয় তাহলে `dashboard/error.js` handle করবে।

---

## গুরুত্বপূর্ণ বিষয়

Error Boundary **event handler-এর error catch করতে পারে না।**

কারণ:

Event handler rendering এর পরে execute হয়।

---

## Event Handler Error Handle করা

এই ক্ষেত্রে manually `try/catch` ব্যবহার করতে হয়।

```javascript
'use client'

import { useState } from 'react'

export function Button() {
  const [error, setError] = useState(null)

  const handleClick = () => {
    try {
      throw new Error('Exception')
    } catch (reason) {
      setError(reason)
    }
  }

  if (error) {
    return <p>Error occurred</p>
  }

  return (
    <button type="button" onClick={handleClick}>
      Click me
    </button>
  )
}
```

---

## useTransition এর Error

যদি `startTransition` এর ভিতরে error হয় তাহলে error **nearest error boundary-তে bubble up হবে।**

```javascript
'use client'

import { useTransition } from 'react'

export function Button() {
  const [pending, startTransition] = useTransition()

  const handleClick = () =>
    startTransition(() => {
      throw new Error('Exception')
    })

  return (
    <button type="button" onClick={handleClick}>
      Click me
    </button>
  )
}
```

---

## Global Error Handling

Application-এর root level-এ error handle করতে `global-error.js` ব্যবহার করা যায়।

```
app/global-error.js
```

---

## Example

```javascript
'use client'

export default function GlobalError({ error, reset }) {
  return (
    <html>
      <body>
        <h2>Something went wrong!</h2>
        <button onClick={() => reset()}>
          Try again
        </button>
      </body>
    </html>
  )
}
```

### গুরুত্বপূর্ণ বিষয়

Global error UI-তে অবশ্যই থাকতে হবে:

```
<html>
<body>
```

কারণ এটি root layout-কে replace করে।

---

## Summary

| Situation                | Solution        |
| ------------------------ | --------------- |
| Form Action Error        | useActionState  |
| API Fetch Error          | res.ok check    |
| 404 Page                 | notFound()      |
| Component Crash          | error.js        |
| Event Handler Error      | try/catch       |
| Global Application Error | global-error.js |

---

## Conclusion

Next.js-এ Error Handling সঠিকভাবে implement করলে:

* Application crash কম হয়
* User experience ভালো হয়
* Debugging সহজ হয়

সঠিক Error Handling ব্যবহার করলে production application অনেক বেশি stable হয়।
