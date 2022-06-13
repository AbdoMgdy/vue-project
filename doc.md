# Vue Notes

## General

- Documentation is Great and pretty much covers it all, there is a link to a section related to typing and typescript on most pages.

- there are a lot of courses and resources out there.

- SSR Support

## App level

- App Level Error Handling makes sure we don't get these cases where one component breaks the whole page : <https://vuejs.org/guide/essentials/application.html#app-configurations>

- Support for multi-apps to co-exist together out of the box : <https://vuejs.org/guide/essentials/application.html#multiple-application-instances>

## Reactivity (Managing State and View )

- Similar to svelte's $ (i think svelte's syntax is a little better you don't have to import from a package to use it)

- Vue's taken this to the next level, reactivity is a separate package part of the vue core packages (<https://github.com/vuejs/core/tree/main/packages/reactivity#readme>)

- It can be used outside components to create stores (Similar to React's context API) besides using it inside the component ( so it combines useState and useContext)

- Deep Reactivity: This means you can expect changes to be detected even when you mutate nested objects or arrays unlike svelte's having to redeclare the variable again for it to work.

- In this page they list a few limitations to the original reactive function and how they address it with another function ref : <https://vuejs.org/guide/essentials/reactivity-fundamentals.html>

- there are a few gotchas that needs a little getting used to, ( some are related to proxies and other are related to how vue uses them) but this is expected with every new technology.

- There is an experimental feature that uses compile-time transforms to enhance the syntax : <https://vuejs.org/guide/essentials/reactivity-fundamentals.html#reactivity-transform>

### Basic counter example in Vue.js

```js
<script setup>
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}
</script>

<template>
  <button @click="increment">{{ count }}</button>
</template>
```

## Forms

- Forms handling is easier in vue thanks to two-way data binding.

- there are a few handy modifier like the lazy modifier to sync changes once you are out of focus : <https://vuejs.org/guide/essentials/forms.html#lazy>

## Component Lifecycle

- There are lifecycle hooks that will run during different times same as React and Svelte.

- you can run these hooks in external functions, have the same concern as Moatey which is if i have multiple external functions with the onMounted Hook how will this be handled ?

## Computed

- Computed is a way to put complex logic away from your template into the script something like  

```js
author.books.length > 0 ? 'Yes' : 'No'
```

- not sure how useful will this be in practice.

## Watchers

- watch() combined with reactive objects is very strong.

- This can trigger on deep reactive changes but since it has performance implications is not the default behavior.

- watchEffect() is similar to useEffect (not quite), but you don't define the dependencies it detects them from the used reactive objects inside: <https://vuejs.org/guide/essentials/watchers.html#watcheffect>

- you can choose wether to access the dom before or after updating inside the callback.

## Components

- hav mixed feelings about The syntax of defining component props (it combines the props and types in the same object)

```js
defineProps<{
  title?: string
  likes?: number
}>()
```

 and the way we emit events ( maybe i'm just attached the way we do it in react)

 ```js
 // runtime
const emit = defineEmits(['change', 'update'])

// type-based
const emit = defineEmits<{
  (e: 'change', id: number): void
  (e: 'update', value: string): void
}>()
```

- currently there is no way to add default values when using type-based declarations, but this could be resolved the experimental Reactivity Transform.

```js
interface Props {
  foo: string
  bar?: number
}

// reactive destructure for defineProps()
// default value is compiled to equivalent runtime option
const { foo, bar = 100 } = defineProps<Props>()
```

- slot support : <https://vuejs.org/guide/components/slots.html>

- Dynamic Components support with an option to keep component's state cached between mounts/unmounts : <https://vuejs.org/guide/essentials/component-basics.html#dynamic-components>

- Fallthrough attributes meaning undeclared props will be automatically passed to the component's root element : <https://vuejs.org/guide/components/attrs.html#fallthrough-attributes>

- Dependency Injection Support as an alternative for Prop Drilling, this can be combined with reactive objects to create the same experience as React Context.

- AsyncComponents import support (i think this is only useful when doing SSR though but this needs more exploration on the use-cases). this is usually found on libraries like next.js.

## Composables ( Hooks )

- this was the main motivation for the new Composition API, sharing the same logic between components ( we all love hooks.)

- this FAQ is a good staring point to read about the Compositions API : <https://vuejs.org/guide/extras/composition-api-faq.html>