# Snippets

Typescript code snippets &amp; utils.

## Sequential Promise Array Execution

Unlike `Promise.all`, you can sequentially execute promises using `Array.reduce`.

```typescript
const dummyFn = (): Promise<void> =>
  new Promise((resolve) => {
    console.log("start");
    setTimeout(() => {
      console.log("end", new Date());
      resolve();
    }, 1500);
  });

const sequentialPromiseAll = (
  arr: (() => Promise<void>)[]
): Promise<void> => {
  return arr.reduce<Promise<void>>(
    (prev, curr) => prev.then(() => curr()),
    new Promise((resolve) => resolve())
  );
};

await sequentialPromiseAll([dummyFn, dummyFn, dummyFn]);
```

[Codepen](https://codepen.io/abelflopes/pen/PoKayWy)
