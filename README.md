# Snippets

Typescript code snippets &amp; utils.

## Sequential Promise Array Execution

Unlike `Promise.all`, you can sequentially execute promises using `Array.reduce`.

```ts
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

## Create a type with common properties of 2 interfaces

The resulting type will only accept properties `b` & `z`.

```ts
interface I1 {
  a: number;
  b: number;
  z: number;
}

interface I2 {
  b: number;
  c: number;
  z: number;
}

type CommonIntersection<T1, T2> = {
  [Property in keyof T1 & keyof T2]: (T1 & T2)[Property];
};

const X: CommonIntersection<I1, I2> = {
  b: 1,
  z: 2,
};
```
