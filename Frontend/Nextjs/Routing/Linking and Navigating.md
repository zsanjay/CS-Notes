
The Next.js router allows you to do client-side route transitions between pages, similar to a single-page application.

A React component called `Link` is provided to do this client-side route transition.

```jsx
import Link from 'next/link'
 
function Home() {
  return (
    <ul>
      <li>
        <Link href="/">Home</Link>
      </li>
      <li>
        <Link href="/about">About Us</Link>
      </li>
      <li>
        <Link href="/blog/hello-world">Blog Post</Link>
      </li>
    </ul>
  )
}
 
export default Home
```

The example above uses multiple links. Each one maps a path (`href`) to a known page:

- `/` → `pages/index.js`
- `/about` → `pages/about.js`
- `/blog/hello-world` → `pages/blog/[slug].js`

Any `<Link />` in the viewport (initially or through scroll) will be prefetched by default (including the corresponding data) for pages using [Static Generation](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props). The corresponding data for [server-rendered](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props) routes is fetched _only when_ the `<Link />` is clicked.

### Linking to dynamic paths

You can also use interpolation to create the path, which comes in handy for [dynamic route segments](https://nextjs.org/docs/pages/building-your-application/routing/dynamic-routes). For example, to show a list of posts which have been passed to the component as a prop:

```jsx
import Link from 'next/link'
 
function Posts({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href={`/blog/${encodeURIComponent(post.slug)}`}>
            {post.title}
          </Link>
        </li>
      ))}
    </ul>
  )
}
 
export default Posts
```

[`encodeURIComponent`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent) is used in the example to keep the path utf-8 compatible.

Alternatively, using a URL Object:

```jsx
import Link from 'next/link'
 
function Posts({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link
            href={{
              pathname: '/blog/[slug]',
              query: { slug: post.slug },
            }}
          >
            {post.title}
          </Link>
        </li>
      ))}
    </ul>
  )
}
 
export default Posts
```

Now, instead of using interpolation to create the path, we use a URL object in `href` where:

- `pathname` is the name of the page in the `pages` directory. `/blog/[slug]` in this case.
- `query` is an object with the dynamic segment. `slug` in this case.

### Injecting the router

[Examples](https://github.com/vercel/next.js/tree/canary/examples/dynamic-routing)

To access the [`router` object](https://nextjs.org/docs/pages/api-reference/functions/use-router#router-object) in a React component you can use [`useRouter`](https://nextjs.org/docs/pages/api-reference/functions/use-router) or [`withRouter`](https://nextjs.org/docs/pages/api-reference/functions/use-router#withrouter).
In general we recommend using [`useRouter`](https://nextjs.org/docs/pages/api-reference/functions/use-router).

### Imperative Routing

[`next/link`](https://nextjs.org/docs/pages/api-reference/components/link) should be able to cover most of your routing needs, but you can also do client-side navigations without it, take a look at the [documentation for `next/router`](https://nextjs.org/docs/pages/api-reference/functions/use-router).

The following example shows how to do basic page navigations with [`useRouter`](https://nextjs.org/docs/pages/api-reference/functions/use-router):

```jsx
import { useRouter } from 'next/router'
 
export default function ReadMore() {
  const router = useRouter()
 
  return (
    <button onClick={() => router.push('/about')}>
      Click here to read more
    </button>
  )
}
```

### Shallow Routing

Shallow routing allows you to change the URL without running data fetching methods again, that includes [`getServerSideProps`](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props), [`getStaticProps`](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props), and [`getInitialProps`](https://nextjs.org/docs/pages/api-reference/functions/get-initial-props).

You'll receive the updated `pathname` and the `query` via the [`router` object](https://nextjs.org/docs/pages/api-reference/functions/use-router#router-object) (added by [`useRouter`](https://nextjs.org/docs/pages/api-reference/functions/use-router) or [`withRouter`](https://nextjs.org/docs/pages/api-reference/functions/use-router#withrouter)), without losing state.

To enable shallow routing, set the `shallow` option to `true`. Consider the following example:

```jsx
import { useEffect } from 'react'
import { useRouter } from 'next/router'
 
// Current URL is '/'
function Page() {
  const router = useRouter()
 
  useEffect(() => {
    // Always do navigations after the first render
    router.push('/?counter=10', undefined, { shallow: true })
  }, [])
 
  useEffect(() => {
    // The counter changed!
  }, [router.query.counter])
}
 
export default Page
```

The URL will get updated to `/?counter=10` and the page won't get replaced, only the state of the route is changed.

You can also watch for URL changes via [`componentDidUpdate`](https://react.dev/reference/react/Component#componentdidupdate) as shown below:

```jsx
componentDidUpdate(prevProps) {
  const { pathname, query } = this.props.router
  // verify props have changed to avoid an infinite loop
  if (query.counter !== prevProps.router.query.counter) {
    // fetch data based on the new query
  }
}
```

### Caveats

Shallow routing **only** works for URL changes in the current page. For example, let's assume we have another page called `pages/about.js`, and you run this:

```jsx
router.push('/?counter=10', '/about?counter=10', { shallow: true })
```

Since that's a new page, it'll unload the current page, load the new one and wait for data fetching even though we asked to do shallow routing.









