
**Good to know**: If you are using the App Router, you can use [Server Components](https://nextjs.org/docs/app/building-your-application/rendering/server-components) or [Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers) instead of API Routes.

API routes provide a solution to build a **public API** with Next.js.

Any file inside the folder `pages/api` is mapped to `/api/*` and will be treated as an API endpoint instead of a `page`. They are server-side only bundles and won't increase your client-side bundle size.

For example, the following API route returns a JSON response with a status code of `200`:

```tsx
import type { NextApiRequest, NextApiResponse } from 'next'
 
type ResponseData = {
  message: string
}
 
export default function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseData>
) {
  res.status(200).json({ message: 'Hello from Next.js!' })
}
```

**Good to know**:

- API Routes [do not specify CORS headers](https://developer.mozilla.org/docs/Web/HTTP/CORS), meaning they are **same-origin only** by default. You can customize such behavior by wrapping the request handler with the [CORS request helpers](https://github.com/vercel/next.js/tree/canary/examples/api-routes-cors).

API Routes can't be used with [static exports](https://nextjs.org/docs/pages/building-your-application/deploying/static-exports). However, [Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers) in the App Router can.

## [Parameters](https://nextjs.org/docs/pages/building-your-application/routing/api-routes#parameters)

```ts
export default function handler(req: NextApiRequest, res: NextApiResponse) {  // ...}
```

- `req`: An instance of [http.IncomingMessage](https://nodejs.org/api/http.html#class-httpincomingmessage)
- `res`: An instance of [http.ServerResponse](https://nodejs.org/api/http.html#class-httpserverresponse)

## [HTTP Methods](https://nextjs.org/docs/pages/building-your-application/routing/api-routes#http-methods)

To handle different HTTP methods in an API route, you can use `req.method` in your request handler, like so:

```ts
import type { NextApiRequest, NextApiResponse } from 'next'
 
export default function handler(req: NextApiRequest, res: NextApiResponse) {
  if (req.method === 'POST') {
    // Process a POST request
  } else {
    // Handle any other HTTP method
  }
}
```
