# How to get the __dirname in esm

```js
 global.ROOT_DIR = new URL(import.meta.url + '/..').pathname
```

or 

```
const __dirname =  new URL(import.meta.url + '/..').pathname
```
