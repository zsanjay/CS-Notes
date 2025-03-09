### Command to create next application

```jsx
npx create-next-app@latest
```

1. Link element

	 It replace anchor tag with Link and doesn't reload the page.

2. Image element

```jsx
import Image from 'next/image';
import logoImg from '@/assets/logo.png';

<Link className={classes.logo} href="/">
	<Image src={logoImg} alt="" priority />
		NextLevel Food
</Link>

```

#### priority prop in Image component

priority prop in Image component loads image eagerly.

Note - Use client code efficiently. Don't convert whole code into client instead convert only required code. Make a separate  component for client and inject into server component.

#### fill prop in Image component

Use fill prop instead of setting a width and height whenever you have an image where you don't know the dimensions in advance.

```jsx
<Image src={image} alt={title} fill />
```

#### Refer why we need to use key in list.

### Setting up a SQLite Database

```jsx
npm install better-sqlite3
```

Put the initdb.js file into the root folder.
### Script for creating db, table and inserting data.

#### initdb.js

```js
const sql = require('better-sqlite3');
const db = sql('meals.db');

const dummyMeals = [
  {
    title: 'Juicy Cheese Burger',
    slug: 'juicy-cheese-burger',
    image: '/images/burger.jpg',
    summary:
      'A mouth-watering burger with a juicy beef patty and melted cheese, served in a soft bun.',
    instructions: `
      1. Prepare the patty:
         Mix 200g of ground beef with salt and pepper. Form into a patty.

      2. Cook the patty:
         Heat a pan with a bit of oil. Cook the patty for 2-3 minutes each side, until browned.

      3. Assemble the burger:
         Toast the burger bun halves. Place lettuce and tomato on the bottom half. Add the cooked patty and top with a slice of cheese.

      4. Serve:
         Complete the assembly with the top bun and serve hot.
    `,
    creator: 'John Doe',
    creator_email: 'johndoe@example.com',
  },
  {
    title: 'Spicy Curry',
    slug: 'spicy-curry',
    image: '/images/curry.jpg',
    summary:
      'A rich and spicy curry, infused with exotic spices and creamy coconut milk.',
    instructions: `
      1. Chop vegetables:
         Cut your choice of vegetables into bite-sized pieces.

      2. Sauté vegetables:
         In a pan with oil, sauté the vegetables until they start to soften.

      3. Add curry paste:
         Stir in 2 tablespoons of curry paste and cook for another minute.

      4. Simmer with coconut milk:
         Pour in 500ml of coconut milk and bring to a simmer. Let it cook for about 15 minutes.

      5. Serve:
         Enjoy this creamy curry with rice or bread.
    `,
    creator: 'Max Schwarz',
    creator_email: 'max@example.com',
  },
  {
    title: 'Homemade Dumplings',
    slug: 'homemade-dumplings',
    image: '/images/dumplings.jpg',
    summary:
      'Tender dumplings filled with savory meat and vegetables, steamed to perfection.',
    instructions: `
      1. Prepare the filling:
         Mix minced meat, shredded vegetables, and spices.

      2. Fill the dumplings:
         Place a spoonful of filling in the center of each dumpling wrapper. Wet the edges and fold to seal.

      3. Steam the dumplings:
         Arrange dumplings in a steamer. Steam for about 10 minutes.

      4. Serve:
         Enjoy these dumplings hot, with a dipping sauce of your choice.
    `,
    creator: 'Emily Chen',
    creator_email: 'emilychen@example.com',
  },
  {
    title: 'Classic Mac n Cheese',
    slug: 'classic-mac-n-cheese',
    image: '/images/macncheese.jpg',
    summary:
      "Creamy and cheesy macaroni, a comforting classic that's always a crowd-pleaser.",
    instructions: `
      1. Cook the macaroni:
         Boil macaroni according to package instructions until al dente.

      2. Prepare cheese sauce:
         In a saucepan, melt butter, add flour, and gradually whisk in milk until thickened. Stir in grated cheese until melted.

      3. Combine:
         Mix the cheese sauce with the drained macaroni.

      4. Bake:
         Transfer to a baking dish, top with breadcrumbs, and bake until golden.

      5. Serve:
         Serve hot, garnished with parsley if desired.
    `,
    creator: 'Laura Smith',
    creator_email: 'laurasmith@example.com',
  },
  {
    title: 'Authentic Pizza',
    slug: 'authentic-pizza',
    image: '/images/pizza.jpg',
    summary:
      'Hand-tossed pizza with a tangy tomato sauce, fresh toppings, and melted cheese.',
    instructions: `
      1. Prepare the dough:
         Knead pizza dough and let it rise until doubled in size.

      2. Shape and add toppings:
         Roll out the dough, spread tomato sauce, and add your favorite toppings and cheese.

      3. Bake the pizza:
         Bake in a preheated oven at 220°C for about 15-20 minutes.

      4. Serve:
         Slice hot and enjoy with a sprinkle of basil leaves.
    `,
    creator: 'Mario Rossi',
    creator_email: 'mariorossi@example.com',
  },
  {
    title: 'Wiener Schnitzel',
    slug: 'wiener-schnitzel',
    image: '/images/schnitzel.jpg',
    summary:
      'Crispy, golden-brown breaded veal cutlet, a classic Austrian dish.',
    instructions: `
      1. Prepare the veal:
         Pound veal cutlets to an even thickness.

      2. Bread the veal:
         Coat each cutlet in flour, dip in beaten eggs, and then in breadcrumbs.

      3. Fry the schnitzel:
      Heat oil in a pan and fry each schnitzel until golden brown on both sides.

      4. Serve:
      Serve hot with a slice of lemon and a side of potato salad or greens.
 `,
    creator: 'Franz Huber',
    creator_email: 'franzhuber@example.com',
  },
  {
    title: 'Fresh Tomato Salad',
    slug: 'fresh-tomato-salad',
    image: '/images/tomato-salad.jpg',
    summary:
      'A light and refreshing salad with ripe tomatoes, fresh basil, and a tangy vinaigrette.',
    instructions: `
      1. Prepare the tomatoes:
        Slice fresh tomatoes and arrange them on a plate.
    
      2. Add herbs and seasoning:
         Sprinkle chopped basil, salt, and pepper over the tomatoes.
    
      3. Dress the salad:
         Drizzle with olive oil and balsamic vinegar.
    
      4. Serve:
         Enjoy this simple, flavorful salad as a side dish or light meal.
    `,
    creator: 'Sophia Green',
    creator_email: 'sophiagreen@example.com',
  },
];

db.prepare(`
   CREATE TABLE IF NOT EXISTS meals (
       id INTEGER PRIMARY KEY AUTOINCREMENT,
       slug TEXT NOT NULL UNIQUE,
       title TEXT NOT NULL,
       image TEXT NOT NULL,
       summary TEXT NOT NULL,
       instructions TEXT NOT NULL,
       creator TEXT NOT NULL,
       creator_email TEXT NOT NULL
    )
`).run();

async function initData() {
  const stmt = db.prepare(`
      INSERT INTO meals VALUES (
         null,
         @slug,
         @title,
         @image,
         @summary,
         @instructions,
         @creator,
         @creator_email
      )
   `);

  for (const meal of dummyMeals) {
    stmt.run(meal);
  }
}

initData();
```

### Run the script

```jsx
node initdb.js
```

The above command will create meals.db file into the root directory.

Note : We don't need to use useEffect hook to fetch the data from the database as components are rendered on server and component can directly talk to the database in Nextjs application.

#### run() vs all() vs get() in SQLite

run method is used to execute insert, delete and update commands

all method is used to get all the rows of a table from the db.

get method is used to get a single tuple or row of a table from the database.

### async and await in server components

You can use async and await in server components.

```jsx
import sql from 'better-sqlite3';

const db = sql('meals.db');

export async function getMeals() {
  await new Promise((resolve) => setTimeout(resolve, 2000));
  return db.prepare('SELECT * FROM meals').all();
}
```

### MealsPage

Use the getMeals function to get the meals data from the db without using useEffect hook.

```jsx
import Link from 'next/link';

import classes from './page.module.css';
import MealsGrid from '@/components/meals/meals-grid';
import { getMeals } from '@/lib/meals';

export default async function MealsPage() {
  const meals = await getMeals();

  return (
    <>
      <header className={classes.header}>
        <h1>
          Delicious meals, created{' '}
          <span className={classes.highlight}>by you</span>
        </h1>
        <p>
          Choose your favorite recipe and cook it yourself. It is easy and fun!
        </p>
        <p className={classes.cta}>
          <Link href="/meals/share">
            Share Your Favorite Recipe
          </Link>
        </p>
      </header>
      <main className={classes.main}>
        <MealsGrid meals={meals} />
      </main>
    </>
  );
}
```

### Caching

Nextjs caches the pages more aggressively even when running the website in production mode.

### loading is another reserved filename.

loading.js

```jsx
import classes from './loading.module.css';

export default function MealsLoadingPage() {
  return <p className={classes.loading}>Fetching meals...</p>;
}
```

### Suspense Component

loading.js file use suspense component under the hood, and wraps the component.

Import Suspense component from react and pass the fallback prop. Now, we don't need to use loading.js file.

```jsx
import { Suspense } from 'react';
import Link from 'next/link';

import classes from './page.module.css';
import MealsGrid from '@/components/meals/meals-grid';
import { getMeals } from '@/lib/meals';

async function Meals() {
  const meals = await getMeals();

  return <MealsGrid meals={meals} />;
}

export default function MealsPage() {
  return (
    <>
      <header className={classes.header}>
        <h1>
          Delicious meals, created{' '}
          <span className={classes.highlight}>by you</span>
        </h1>
        <p>
          Choose your favorite recipe and cook it yourself. It is easy and fun!
        </p>
        <p className={classes.cta}>
          <Link href="/meals/share">Share Your Favorite Recipe</Link>
        </p>
      </header>
      <main className={classes.main}>
        <Suspense fallback={<p className={classes.loading}>Fetching meals...</p>}>
          <Meals />
        </Suspense>
      </main>
    </>
  );
}
```

### error reserved filename for error handling

error.js

error must be a client component, it ensure that you application can handle both server and client side errors.

```jsx
import sql from 'better-sqlite3';

const db = sql('meals.db');

export async function getMeals() {
  await new Promise((resolve) => setTimeout(resolve, 2000));
  
  throw new Error('Loading meals failed');
  return db.prepare('SELECT * FROM meals').all();
}
```

```jsx
'use client';

export default function Error() {
  return (
    <main className="error">
      <h1>An error occurred!</h1>
      <p>Failed to fetch meal data. Please try again later.</p>
    </main>
  );
}
```

### not-found.js

This can be used when the route or resource is not defined.

```jsx
export default function NotFound() {
  return (
    <main className="not-found">
      <h1>Not found</h1>
      <p>Unfortunately, we could not find the requested page or resource.</p>
    </main>
  );
}
```

## What is `dangerouslySetInnerHTML`?

The `dangerouslySetInnerHTML` property in a React application is the equivalent of the `innerHTML` attribute in the browser DOM. In vanilla JavasScript, `innerHTML` is a property of DOM elements that allows you to get or set the HTML content inside an element. It’s a part of the standard DOM API, not specific to React.

`dangerouslySetInnerHTML` is React’s replacement for using `innerHTML`. `dangerouslySetInnerHTML` is a property that you can use on HTML elements in a React application to programmatically set their content. Instead of using a selector to grab the HTML element and then setting its `innerHTML`, you can use this property directly on the element.

When `dangerouslySetInnerHTML` is used, React also knows that the contents of that specific element are dynamic, and, for the children of that node, it simply skips the comparison against the virtual DOM to gain some extra performance.

As the name of the property suggests, it can be dangerous to use `dangerouslySetInnerHTML` because it makes your code vulnerable to cross-site scripting (XSS) attacks. This can become an especially big issue if you are fetching data from a third-party source or rendering content submitted by users.

```jsx
const App = () => {
  const data = 'lorem <b>ipsum</b>';

  return (
    <div
      dangerouslySetInnerHTML={{__html: data}}
    />
  );
}

export default App;
```

### Get Query

```jsx
export function getMeal(slug) {
	return db.prepare('SELECT * FROM meals WHERE slug = ?').get(slug);
}
```

### notFound component 

It will return the closest not-found.js file content. 

```jsx
import Image from 'next/image';
import { notFound } from 'next/navigation';

import { getMeal } from '@/lib/meals';
import classes from './page.module.css';

export default function MealDetailsPage({ params }) {
  const meal = getMeal(params.mealSlug);

  if (!meal) {
    notFound();
  }

  meal.instructions = meal.instructions.replace(/\n/g, '<br />');

  return (
    <>
      <header className={classes.header}>
        <div className={classes.image}>
          <Image src={meal.image} alt={meal.title} fill />
        </div>
        <div className={classes.headerText}>
          <h1>{meal.title}</h1>
          <p className={classes.creator}>
            by <a href={`mailto:${meal.creator_email}`}>{meal.creator}</a>
          </p>
          <p className={classes.summary}>{meal.summary}</p>
        </div>
      </header>
      <main>
        <p
          className={classes.instructions}
          dangerouslySetInnerHTML={{
            __html: meal.instructions,
          }}
        ></p>
      </main>
    </>
  );
}
```

### Server Action

Use 'use server' inside the function and async with the function.

```jsx
import ImagePicker from '@/components/meals/image-picker';
import classes from './page.module.css';
import { redirect } from 'next/navigation';

export default function ShareMealPage() {

  async function shareMeal(formData) {
    'use server';

    const meal = {
      title: formData.get('title'),
      summary: formData.get('summary'),
      instructions: formData.get('instructions'),
      image: formData.get('image'),
      creator: formData.get('name'),
      creator_email: formData.get('email')
    }

    console.log(meal);
    await saveMeal(meal);
    redirect('/meals');
  }

  return (
    <>
      <header className={classes.header}>
        <h1>
          Share your <span className={classes.highlight}>favorite meal</span>
        </h1>
        <p>Or any other meal you feel needs sharing!</p>
      </header>
      <main className={classes.main}>
        <form className={classes.form} action={shareMeal}>
          <div className={classes.row}>
            <p>
              <label htmlFor="name">Your name</label>
              <input type="text" id="name" name="name" required />
            </p>
            <p>
              <label htmlFor="email">Your email</label>
              <input type="email" id="email" name="email" required />
            </p>
          </div>
          <p>
            <label htmlFor="title">Title</label>
            <input type="text" id="title" name="title" required />
          </p>
          <p>
            <label htmlFor="summary">Short Summary</label>
            <input type="text" id="summary" name="summary" required />
          </p>
          <p>
            <label htmlFor="instructions">Instructions</label>
            <textarea
              id="instructions"
              name="instructions"
              rows="10"
              required
            ></textarea>
          </p>
          <ImagePicker label="Your image" name="image" />
          <p className={classes.actions}>
            <button type="submit">Share Meal</button>
          </p>
        </form>
      </main>
    </>
  );
}
```

# useFormStatus

`useFormStatus` is a Hook that gives you status information of the last form submission.

```jsx
const { pending, data, method, action } = useFormStatus();
```

```jsx
'use client';

import { useFormStatus } from 'react-dom';

export default function MealsFormSubmit() {
  const { pending } = useFormStatus();

  return (
    <button disabled={pending}>
      {pending ? 'Submitting...' : 'Share Meal'}
    </button>
  );
}
```

### Server Side Input Validation

```jsx
'use server';

import { redirect } from 'next/navigation';
import { saveMeal } from './meals';

function isInvalidText(text) {
  return !text || text.trim() === '';
}

export async function shareMeal(formData) {
  const meal = {
    title: formData.get('title'),
    summary: formData.get('summary'),
    instructions: formData.get('instructions'),
    image: formData.get('image'),
    creator: formData.get('name'),
    creator_email: formData.get('email'),
  };

  if (
    isInvalidText(meal.title) ||
    isInvalidText(meal.summary) ||
    isInvalidText(meal.instructions) ||
    isInvalidText(meal.creator) ||
    isInvalidText(meal.creator_email) ||
    !meal.creator_email.includes('@') ||
    !meal.image ||
    meal.image.size === 0
  ) {
    throw new Error('Invalid input');
  }

  await saveMeal(meal);
  redirect('/meals');
}
```

```jsx
'use client';

export default function Error() {
  return (
    <main className="error">
      <h1>An error occurred!</h1>
      <p>Failed to create meal.</p>
    </main>
  );
}
```

# useActionState

If you want to show error message on the same page and doesn't want to show error page. You can use useActionState to achieve.

In earlier React Canary versions, this API was part of React DOM and called `useFormState`.

`useActionState` is a Hook that allows you to update state based on the result of a form action.

```jsx
const [state, formAction, isPending] = useActionState(fn, initialState, permalink?);
```

```jsx
'use client';

import { useFormState } from 'react-dom';

import ImagePicker from '@/components/meals/image-picker';
import classes from './page.module.css';
import { shareMeal } from '@/lib/actions';
import MealsFormSubmit from '@/components/meals/meals-form-submit';

export default function ShareMealPage() {
  const [state, formAction] = useFormState(shareMeal, { message: null });

  return (
    <>
      <header className={classes.header}>
        <h1>
          Share your <span className={classes.highlight}>favorite meal</span>
        </h1>
        <p>Or any other meal you feel needs sharing!</p>
      </header>
      <main className={classes.main}>
        <form className={classes.form} action={formAction}>
          <div className={classes.row}>
            <p>
              <label htmlFor="name">Your name</label>
              <input type="text" id="name" name="name" required />
            </p>
            <p>
              <label htmlFor="email">Your email</label>
              <input type="email" id="email" name="email" required />
            </p>
          </div>
          <p>
            <label htmlFor="title">Title</label>
            <input type="text" id="title" name="title" required />
          </p>
          <p>
            <label htmlFor="summary">Short Summary</label>
            <input type="text" id="summary" name="summary" required />
          </p>
          <p>
            <label htmlFor="instructions">Instructions</label>
            <textarea
              id="instructions"
              name="instructions"
              rows="10"
              required
            ></textarea>
          </p>
          <ImagePicker label="Your image" name="image" />
          {state.message && <p>{state.message}</p>}
          <p className={classes.actions}>
            <MealsFormSubmit />
          </p>
        </form>
      </main>
    </>
  );
}
```

```jsx
'use server';

import { redirect } from 'next/navigation';
import { saveMeal } from './meals';

function isInvalidText(text) {
  return !text || text.trim() === '';
}

export async function shareMeal(prevState, formData) {
  const meal = {
    title: formData.get('title'),
    summary: formData.get('summary'),
    instructions: formData.get('instructions'),
    image: formData.get('image'),
    creator: formData.get('name'),
    creator_email: formData.get('email'),
  };

  if (
    isInvalidText(meal.title) ||
    isInvalidText(meal.summary) ||
    isInvalidText(meal.instructions) ||
    isInvalidText(meal.creator) ||
    isInvalidText(meal.creator_email) ||
    !meal.creator_email.includes('@') ||
    !meal.image ||
    meal.image.size === 0
  ) {
    return {
      message: 'Invalid input.',
    };
  }

  await saveMeal(meal);
  redirect('/meals');
}
```

### Building for production 

```jsx
npm run build
npm start
```

### Pre rendering static pages

Upside :

Nextjs pre renders all the non dynamic pages and it doesn't make any http calls again, returns the cached page to all the clients.

Downside:

If the data is updated in the db, but it will not be shown in page because the page is cached.

### Trigger Cache Revalidation

#### revalidatePath 

Revalidate the cache of the meals page.

```jsx
'use server';

import { redirect } from 'next/navigation';
import { saveMeal } from './meals';
import { revalidatePath } from 'next/cache';

function isInvalidText(text) {
  return !text || text.trim() === '';
}

export async function shareMeal(prevState, formData) {
  const meal = {
    title: formData.get('title'),
    summary: formData.get('summary'),
    instructions: formData.get('instructions'),
    image: formData.get('image'),
    creator: formData.get('name'),
    creator_email: formData.get('email'),
  };

  if (
    isInvalidText(meal.title) ||
    isInvalidText(meal.summary) ||
    isInvalidText(meal.instructions) ||
    isInvalidText(meal.creator) ||
    isInvalidText(meal.creator_email) ||
    !meal.creator_email.includes('@') ||
    !meal.image ||
    meal.image.size === 0
  ) {
    return {
      message: 'Invalid input.',
    };
  }

  await saveMeal(meal);
  revalidatePath('/meals');
  redirect('/meals');
}
```

```jsx
revalidatePath('/meals', 'page'); // default
```

By default, revalidatePath revalidates a single page. If you want to revalidate all the nested routes then use.

```jsx
revalidatePath('/meals', 'layout');
```

### Don't store files locally on the Filesystem!

# Static Assets in `public`

Next.js can serve static files, like images, under a folder called `public` in the root directory. Files inside `public` can then be referenced by your code starting from the base URL (`/`).

For example, the file `public/avatars/me.png` can be viewed by visiting the `/avatars/me.png` path. The code to display that image might look like:

```jsx
import Image from 'next/image'
 
export function Avatar({ id, alt }) {
  return <Image src={`/avatars/${id}.png`} alt={alt} width="64" height="64" />
}
 
export function AvatarOfMe() {
  return <Avatar id="me" alt="A portrait of me" />
}
```

## [Caching](https://nextjs.org/docs/app/building-your-application/optimizing/static-assets#caching)

Next.js cannot safely cache assets in the `public` folder because they may change. The default caching headers applied are:

```
Cache-Control: public, max-age=0
```

Good to know:

- The directory must be named `public`. The name cannot be changed and it's the only directory used to serve static assets.
- Only assets that are in the `public` directory at [build time](https://nextjs.org/docs/app/api-reference/cli/next#next-build-options) will be served by Next.js. Files added at request time won't be available. We recommend using a third-party service like [Vercel Blob](https://vercel.com/docs/storage/vercel-blob?utm_source=next-site&utm_medium=docs&utm_campaign=next-website) or AWS S3 for persistent file storage.

### Storing Uploaded Images In The Cloud (AWS S3)

As explained in the previous lecture, storing uploaded files (or any other files that are generated at runtime) on the local filesystem is not a great idea - because those files will simply not be available in the running NextJS applications.

Instead, it's recommended that you store such files (e.g., uploaded images) via some cloud file storage - like [AWS S3](https://aws.amazon.com/s3/).

AWS S3 is a service provided by AWS which allows you to store and serve (depending on its configuration) files. You can get started with this service for free but you should check out its [pricing page](https://aws.amazon.com/s3/pricing/) to avoid any unwanted surprises.

In this lecture, I'll explain how you could use AWS S3 to store uploaded users images & serve them on the NextJS website.

**1) Create an AWS account**

In order to use AWS S3, you need an AWS account. You can create one [here](https://aws.com/).

**2) Create a S3 bucket**

Once you created an account (and you logged in), you should navigate to the [S3 console](https://s3.console.aws.amazon.com/s3/home) to create a so-called "bucket".

"Buckets" are containers that can be used to store files (side-note: you can store any files - not just images).

Every bucket must have a globally unique name, hence you should become creative. You could, for example, use a name like _your-name-nextjs-demo-users-image_.

I'll use _maxschwarzmueller-nextjs-demo-users-image_ in this example here.

When creating the bucket, you can confirm all the default settings - the name's the only thing you should set.

**3) Upload the dummy image files**

Now that the bucket was created, you can already add some files to it => The dummy images that were previously stored locally in the `public/images` folder.

To do that, select your created bucket and click the "Upload" button. Then drag & drop those images into the box and confirm the upload.

![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2023-12-05_16-08-04-6bfba34e3c75a4d22777d0f78186caa4.jpg)

Thereafter, all those images should be in the bucket:

![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2023-12-05_16-08-04-10f2210199bbbd7e4e28dea2f86f8438.jpg)

**4) Configure the bucket for serving the images**

Now that you uploaded those dummy images, it's time to configure the bucket such that the images can be loaded from the NextJS website.

Because, by default, this is **not possible**! By default, S3 buckets are "locked down" and the files in there are secure & not accessible by anyone else.

But for our purposes here, we must update the bucket settings to make sure the images can be viewed by everyone.

To do that, as a first step, click on the "Permissions" tab and "Edit" the "Block public access" setting:

![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2023-12-05_16-16-13-182f4df18025905a2bb63d40d9649228.jpg)

Then, disable the "Block all public access" checkbox (and with it, all other checkboxes) and select "Save Changes".

Type "confirm" into the confirmation overlay once it pops up.

That's not all though - as a next (and final step), you must add a so-called "Bucket Policy". That's an AWS-specific policy document that allows you to manage the permissions of the objects stored in the bucket.

You can add such a "Bucket Policy" right below the "Block all public access" area, still on the "Permissions" tab:

Click "Edit" and insert the following bucket policy into the box:

1. {
2.     "Version": "2012-10-17",
3.     "Statement": [
4.         {
5.             "Sid": "PublicRead",
6.             "Effect": "Allow",
7.             "Principal": "*",
8.             "Action": [
9.                 "s3:GetObject",
10.                 "s3:GetObjectVersion"
11.             ],
12.             "Resource": [
13.                 "arn:aws:s3:::DOC-EXAMPLE-BUCKET/*"
14.             ]
15.         }
16.     ]
17. }

Replace `DOC-EXAMPLE-BUCKET` with your bucket name (_maxschwarzmueller-nextjs-demo-users-image_ in my case).

Then, click "Save Changes".

Now the bucket is configure to grant access to all objects inside of it to anyone who has a URL pointing to one of those objects.

Therefore, you should now of course not add any files into the bucket that you don't want to share with the world!

To test if everything works, click on one of the images you uploaded (in the bucket).

Then click on the "Object URL" - if opening it works (and you can see the image), you configured everything as needed.

![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2023-12-05_16-24-53-464554545d10936f87d523715350d1f0.jpg)

**5) Update the NextJS code to use those S3 images**

Now that the images are stored + served via S3, it's time to also load them from there in your NextJS app.

As a first step, you can delete the `public/images` folder (so that an empty `public/` folder remains).

Now, if you also delete the `.next` folder in the NextJS project and you then visit `localhost:3000/meals`, you should see a bunch of meals without images.

To bring them back, as a first step, edit the database data by updating the `initdb.js` file: Change all the image property values from `image: '/images/burger.jpg'`, to `image: 'burger.jpg'` (and do that for all meals).

Alternatively, you find an updated `initdb.js` file attached.

Next, go to the `components/meals/meal-item.js` file (which contains the `MealItem` component) and update the `<Image>` `src`:

```jsx
1. <Image
2.   src={`https://maxschwarzmueller-nextjs-demo-users-image.s3.amazonaws.com/${image}`}
3.   alt={title}
4.   fill
5. />
```


_Of course, use your S3 URL / bucket name!_

The new `src` value is a string that contains the S3 URL to your bucket objects (i.e., the URL you previously clicked for testing purposes - without the image file name at the end). The actual image name that should be loaded is then dynamically inserted via `${image}`.

_Note: This will only work if the images stored in the S3 bucket have the names referenced in the_ `_initdb.js_` _file!_

You should also update the `app/meals/[mealSlug]/page.js` file and make sure that the image on this page is also fetched from S3:

```jsx
1. <Image
2.   src={`https://maxschwarzmueller-nextjs-demo-users-image.s3.amazonaws.com/${meal.image}`}
3.   alt={meal.title}
4.   fill
5. />
```

_Of course, use your S3 URL / bucket name!_

Now, to reset the database data, you should delete your `meals.db` file (i.e., delete the SQLite database file) and re-run `node initdb.js` to re-initialize it (with the updated image values).

If you do that, and you then restart the development server (`npm run dev`), you'll notice that you now get an error when visiting the `/meals` page:

Error: Invalid src prop (https://maxschwarzmueller-nextjs-demo-users-image.s3.amazonaws.com/burger.jpg) on `next/image`, hostname "maxschwarzmueller-nextjs-demo-users-image.s3.amazonaws.com" is not configured under images in your `next.config.js`

**6) Allowing S3 as an image source**

You get this error because, by default, NextJS does not allow external URLs when using the `<Image>` component.

You explicitly have to allow such a URL in order to get rid of this error.

That's done by editing the `next.config.js file`:

1. const nextConfig = {
2.   images: {
3.     remotePatterns: [
4.       {
5.         protocol: 'https',
6.         hostname: 'maxschwarzmueller-nextjs-demo-users-image.s3.amazonaws.com',
7.         port: '',
8.         pathname: '/**',
9.       },
10.     ],
11.   },
12. };

_Of course, use your S3 URL / bucket name!_

This `remotePatterns` config allows this specific S3 URL as a valid source for images.

With the config file updated + saved, you should now be able to visit `/meals` and see all those images again.

**7) Storing uploaded images on S3**

Now that we can see those dummy images again, it's finally time to also "forward" user-generated (i.e., uploaded) images to S3.

This can be done with help of a package provided by AWS - the `@aws-sdk/client-s3` package. This package provides functionalities that allow you to interact with S3 - e.g., to store files in a specific bucket.

Install that package via `npm install @aws-sdk/client-s3`.

Then, go to your `lib/meals.js` file and import the AWS S3 SDK (at the top of the file):

1. import { S3 } from '@aws-sdk/client-s3';

Next, initialize it by adding this line (e.g., right above the line where the db object is created):

1. const s3 = new S3({
2.   region: 'us-east-1'
3. });
4. const db = sql('meals.db'); // <- this was already there!

Almost there!

Now, edit the `saveMeal()` function and remove all code that was related to storing the image on the local file system.

Instead, add this code:

1. s3.putObject({
2.   Bucket: 'maxschwarzmueller-nextjs-demo-users-image',
3.   Key: fileName,
4.   Body: Buffer.from(bufferedImage),
5.   ContentType: meal.image.type,
6. });

_Of course, use your S3 URL / bucket name!_

Also make sure to save the image filename under `meal.image`:

1. meal.image = fileName;

The final `saveMeal()` function should look like this:

1. export async function saveMeal(meal) {
2.   meal.slug = slugify(meal.title, { lower: true });
3.   meal.instructions = xss(meal.instructions);

5.   const extension = meal.image.name.split('.').pop();
6.   const fileName = `${meal.slug}.${extension}`;

8.   const bufferedImage = await meal.image.arrayBuffer();

10.   s3.putObject({
11.     Bucket: 'maxschwarzmueller-nextjs-demo-users-image',
12.     Key: fileName,
13.     Body: Buffer.from(bufferedImage),
14.     ContentType: meal.image.type,
15.   });

18.   meal.image = fileName;

20.   db.prepare(
21.     `
22.     INSERT INTO meals
23.       (title, summary, instructions, creator, creator_email, image, slug)
24.     VALUES (
25.       @title,
26.       @summary,
27.       @instructions,
28.       @creator,
29.       @creator_email,
30.       @image,
31.       @slug
32.     )
33.   `
34.   ).run(meal);
35. }

**8) Granting the NextJS backend AWS access permissions**

Now, there's just one last, yet very important, step missing: Granting your NextJS app S3 access permissions.

We did configure S3 to serve the bucket content to everyone.

But we did not (and should not!) configure it to allow everyone to write to the bucket or change the bucket contents.

But that's what our NextJS app (via the S3 AWS SDK) now tries to do!

To grant our app appropriate permissions, you must set up AWS access keys for your app.

This is done by adding a `.env.local` file to your root NextJS project. This file will automatically be read by NextJS and the environment variables configured in there will be made available to the backend (!) part of your app.

You can learn more about setting up environment variables for NextJS apps here: [https://nextjs.org/docs/app/building-your-application/configuring/environment-variables](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables).

In this `.env.local` file, you must add two key-value pairs:

```shell
1. AWS_ACCESS_KEY_ID=<your aws access key>
2. AWS_SECRET_ACCESS_KEY=<your aws secret access key>
```

You get those access keys from inside the AWS console (in the browser). You can get them by clicking on your account name (in the top right corner of the AWS console) and then "Security Credentials".

Scroll down to the "Access Keys" area and create a new Access Key. Copy and paste the values into your .env.local file and **never share these keys with anyone!** Don't commit them to Git or anything like that!

You can learn more about them here: [https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)

With all that done, finally, you should be able to create new meals, upload images and see them on `/meals`. Even in production! Because now, the images are stored on S3!

You find the finished, adjusted code attached to this lecture. Please note that the `.env.local` file is not included - you must add it (and use your own credentials) if you want to run the attached code.

### Static Metadata

We can add static metadata to our application, it helps web crawler or SEO to find the page.

https://nextjs.org/docs/app/api-reference/functions/generate-metadata

##### Adding metadata to root layout will be available to all the nested layout, we can override the metadata by defining the metadata in nested layout or page.

RootLayout.jsx

```jsx
import MainHeader from '@/components/main-header/main-header';
import './globals.css';

export const metadata = {
  title: 'NextLevel Food',
  description: 'Delicious meals, shared by a food-loving community.',
};

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        
        <MainHeader />
        {children}
      </body>
    </html>
  );
}
```

Adding metadata in MealsPage

```jsx
import { Suspense } from 'react';
import Link from 'next/link';

import classes from './page.module.css';
import MealsGrid from '@/components/meals/meals-grid';
import { getMeals } from '@/lib/meals';

export const metadata = {
  title: 'All Meals',
  description: 'Browse the delicious meals shared by our vibrant community.',
};

async function Meals() {
  console.log('Fetching meals');
  const meals = await getMeals();

  return <MealsGrid meals={meals} />;
}

export default function MealsPage() {
  return (
    <>
      <header className={classes.header}>
        <h1>
          Delicious meals, created{' '}
          <span className={classes.highlight}>by you</span>
        </h1>
        <p>
          Choose your favorite recipe and cook it yourself. It is easy and fun!
        </p>
        <p className={classes.cta}>
          <Link href="/meals/share">Share Your Favorite Recipe</Link>
        </p>
      </header>
      <main className={classes.main}>
        <Suspense fallback={<p className={classes.loading}>Fetching meals...</p>}>
          <Meals />
        </Suspense>
      </main>
    </>
  );
}
```

### Generate Dynamic Metadata

```jsx
 import Image from 'next/image';
import { notFound } from 'next/navigation';

import { getMeal } from '@/lib/meals';
import classes from './page.module.css';

export async function generateMetadata({ params }) {
  const meal = getMeal(params.mealSlug);

  if (!meal) {
    notFound();
  }

  return {
    title: meal.title,
    description: meal.summary,
  };
}

export default function MealDetailsPage({ params }) {
  const meal = getMeal(params.mealSlug);

  if (!meal) {
    notFound();
  }

  meal.instructions = meal.instructions.replace(/\n/g, '<br />');

  return (
    <>
      <header className={classes.header}>
        <div className={classes.image}>
          <Image src={meal.image} alt={meal.title} fill />
        </div>
        <div className={classes.headerText}>
          <h1>{meal.title}</h1>
          <p className={classes.creator}>
            by <a href={`mailto:${meal.creator_email}`}>{meal.creator}</a>
          </p>
          <p className={classes.summary}>{meal.summary}</p>
        </div>
      </header>
      <main>
        <p
          className={classes.instructions}
          dangerouslySetInnerHTML={{
            __html: meal.instructions,
          }}
        ></p>
      </main>
    </>
  );
}
```

The above contents are using App Router but you can also build nextjs application using pages router, which is the old way of creating full stack web application.
### Nextjs Pages Router

There is no index.html file present in the public folder of NextJs app, due to the pre rendering of the requested page.

In case of pages router, we will create the files or paths in pages directory.

Default page is "_app.js".

As there is no index.html file, for the root path we need to create index.js file, in which you can create the root component which is pre rendered by next js.
##### To create more routes

In the pages directory, we can create the file like news.js component. 

'/news' will be the path for that file.

#### Nested Routes

To declare nested route we need to create sub folder in the pages folder with the route name and specify index.js file in the nested folder

For example:

pages/news - inside this you can create index.js file or some other route by specifying the filename like something-imp.js then the route will be pages/news/something-imp.

#### Dynamic Route Segments

We can define dynamic routes using [something].js. in the specific path.
### useRouter Hook

We can use the useRouter hook to extract the information of the path and any path values.

```jsx
import { useRouter } from 'next/router';

// our-domain.com/news/something-important

function DetailPage() {
  const router = useRouter();

  const newsId = router.query.newsId;

  // send a request to the backend API
  // to fetch the news item with newsId

  return <h1>The Detail Page</h1>
}

export default DetailPage;
```
### _app.js file

```jsx
import Layout from '../components/layout/Layout';
import '../styles/globals.css';

function MyApp({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  );
}

export default MyApp;
```

The `Component` prop is the active `page`, so whenever you navigate between routes, `Component` will change to the new `page`. Therefore, any props you send to `Component` will be received by the `page`.

`pageProps` is an object with the initial props that were preloaded for your page by one of our [data fetching methods](https://nextjs.org/docs/pages/building-your-application/data-fetching), otherwise it's an empty object.

### References

https://github.com/academind/react-complete-guide-course-resources/tree/main

https://www.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/41718144#overview













