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
