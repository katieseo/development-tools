#### Head meta
components/meta.js
```js

import Head from 'next/head'

const Meta = ({ title, keywords, description }) => {
  return (
    <Head>
      <meta name='viewport' content='width=device-width, initial-scale=1' />
      <meta name='keywords' content={keywords} />
      <meta name='description' content={description} />
      <meta charSet='utf-8' />
      <link rel='icon' href='/favicon.ico' />
      <title>{title}</title>
    </Head>
  )
}

Meta.defaultProps = {
  title: 'Site Title',
  keywords: 'keyword1, keyword2',
  description: 'site description',
}

export default Meta
```



#### Article Detail Page

pages/article/[id]/index.js

```js
import { useRouter } from "next/router";

const Article = () => {
  const router = useRouter();
  const { id } = router.query;

  return <div>This is an Article {id}</div>;
};

export default Article;
```

OR

```js
const Article = ({ article }) => {
  return <div>This is an Article {article.id}</div>;
};

export default Article;

export const getServerSideProps = async (context) => {
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/posts/${context.params.id}`
  );
  const article = await res.json();

  return {
    props: {
      article,
    },
  };
};
```

OR

getStaticProps + getStaticPaths

This way is faster

```js
import Link from "next/link";

const Article = ({ article }) => {
  return (
    <div>
      This is an Article {article.id} <Link href="/">Go Back</Link>
    </div>
  );
};

export default Article;

export const getStaticProps = async (context) => {
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/posts/${context.params.id}`
  );
  const article = await res.json();

  return {
    props: {
      article,
    },
  };
};

export const getStaticPaths = async () => {
  const res = await fetch(`https://jsonplaceholder.typicode.com/posts`);
  const articles = await res.json();
  const ids = articles.map((article) => article.id);
  const paths = ids.map((id) => ({ params: { id: id.toString() } }));

  return {
    paths,
    fallback: false,
  };
};
```

#### build API

*Articles

pages/api/articles/index.js

```js
import { articles } from "../../../data";

export default function handler(req, res) {
  res.status(200).json(articles);
}
```
http://localhost:3000/api/articles <-- data can be patched


*Single Article

pages/api/articles/[id].js

```js
import { articles } from "../../../data";

export default function handler(req, res) {
  const filtered = articles.filter((article) => article.id === req.query.id);

  if (filtered.lenght > 0) {
    res.status(200).json(filtered[0]);
  } else {
    res
      .status(404)
      .json({ message: `Article with the id of ${req.query.id} is not found` });
  }
}
```
http://localhost:3001/api/articles/1

*Using the API in the application

config/index.js

```js
const dev = process.env.NODE_ENV !== 'production'

export const server = dev ? 'http://localhost:3000' : 'https://mywebsite.com'
```

pages/index.js

```js
import { server } from "../config";

export default function Home({ articles }) {
  return (
    <div className="mx-auto max-w-7xl">
      ....
    </div>
  );
}

export const getStaticProps = async () => {
  const response = await fetch(`${server}/api/articles`);
  const articles = await response.json();

  return {
    props: {
      articles,
    },
  };
};
```



#### generating static sites

package.json
```
  "scripts": {
    "dev": "next dev",
    "build": "next build && next export",  <----next export
    "start": "next start",
    "lint": "next lint"
  },

npm run build > out folder: static websites

sudo npm i -g serve
serve -s out -p 8000
```
