# vite-plugin-assemblyscript

[Vite](https://vitejs.dev/) plugin that turns [AssemblyScript](https://www.assemblyscript.org/) import into a [WebAssembly](https://webassembly.org/) [Data URL](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URLs).

Tested with [AssemblyScript](https://www.assemblyscript.org/) 0.21.x

## Motivation

 * [surma/rollup-plugin-assemblyscript](https://github.com/surma/rollup-plugin-assemblyscript) uses `emitFile` which it unsuitable as a Vite plugin in dev mode.
 * [olivierchatry/vite-plugin-assemblyscript](https://github.com/olivierchatry/vite-plugin-assemblyscript) does not work with AssemblyScript 0.21.x due to breaking API changes. Not possible to specifiy import object.

## Usage

1. Install [`assemblyscript`](https://www.npmjs.com/package/assemblyscript)
2. Copy `vite-plugin-assemblyscript.js` into your own project. There is no NPM release. The existing NPM package of the same name is not mine and does not work.
3. Add to `vite.config.js`

### `vite.config.js`

```js
import Assemblyscript from "./vite-plugin-assemblyscript";
export default {
  ...
  plugins: [Assemblyscript()],
};
```

### `add.as`

```ts
export function add(x: i32, y: i32): i32 {
  return x + y;
}
```

### `index.js`

```js
import wasmDataUrl from "./add.as";
const module = await WebAssembly.compileStreaming(fetch(wasmDataUrl))
const instance = await WebAssembly.instantiate(module, {})
console.log(instance.exports.add(1, 2))
```

StackBlitz demo: https://stackblitz.com/edit/vitejs-vite-dwpseg?file=index.html
