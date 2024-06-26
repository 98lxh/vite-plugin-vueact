# vite-plugin-vueact

## Install

`pnpm`

```shell
pnpm add --save-dev vite-plugin-vueact
```

`npm`

```shell
npm install --save-dev vite-plugin-vueact
```

 `yarn`

```shell
yarn add --save-dev vite-plugin-vueact
```

## Using in vite

```ts
import { defineConfig } from "vite";

import vue from "@vitejs/plugin-vue";
import { vitePluginVueact } from "vite-plugin-vueact";
import AutoImport from "unplugin-auto-import/vite";
import VueJsx from "@vitejs/plugin-vue-jsx";

export default defineConfig({
  plugins: [
    vue(),
    vitePluginVueact(),
    VueJsx(),
    AutoImport({ imports: [ 'vue' ] })
  ]
})
```

## Examples

---

source

```tsx
import { VNode } from "vue";

type DefineProps = {
  num: number;
  messsage: string;
  children?: VNode | string;
}
 
function SingleComponent(props: DefineProps) {
  return (
    <div>
      <p>{props.messsage}</p>
      <p>{props.children}</p>
      <p>{props.num}</p>
    </div>
  )
}

export default SingleComponent;
```

---

resolved

```tsx
import { defineComponent } from "vue";

const SingleComponent = defineComponent({
  name: "SingleComponent",
  props: {
    "num": { type: Number, "required": true },
    "messsage": { type: String, "required": true },
    "children": { type: [Object, String] }
  },
  setup(props) {
    return () => (
      <div>
        <p>{props.messsage}</p>
        <p>{props.children}</p>
        <p>{props.num}</p>
      </div>
    )
  }
})

export default SingleComponent;
```

---

source

```tsx
// externally imported type definitions...

// ./types
export interface DefineProps {
  num: number;
  messsage: string;
  children?: string;
}


// SingleComponent.tsx
import { FC } from "vite-plugin-vueact"
import { DefineProps } from "./types";

const SingleComponent: FC<DefineProps> = function (props, { slots }) {
  const { messsage, num, children } = props;

  return (
    <div>
      {slots.default && slots.default()}
      <p>{messsage}</p>
      <p>{children}</p>
      <p>{num}</p>
    </div>
  );
}

export default SingleComponent;
```

---

resolved

```tsx
import { FC } from "vite-plugin-vueact"
import { DefineProps } from "./types";

const SingleComponent = defineComponent({
  name: "SingleComponent",
  props: {
    "num": { type: Number, "required": true },
    "messsage": { type: String, "required": true },
    "children": { type: String }
  },
  setup(props, { slots }) {
    const { messsage, num, children } = props;

    return () => (
      <div>
        {slots.default && slots.default()}
        <p>{messsage}</p>
        <p>{children}</p>
        <p>{num}</p>
      </div>
    );
  }
})

export default SingleComponent;
```
