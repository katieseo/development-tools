Article Detail Page

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
