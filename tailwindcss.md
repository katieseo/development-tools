globals.css
```js
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  a {
    @apply text-blue-900;
  }

  a:hover {
    @apply text-red-500;
  }
}
```
