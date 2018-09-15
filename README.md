# Quantum DOM

  Simple virtual DOM intended for custom elements

## API overview (ideas)

  Virtual node

```ts
interface VNode {
  // Private DOM node instance preservation
  _instance: Text | HTMLElement

  hooks: {
    // DOM node creation
    creating: Function
    created: Function

    // DOM node attach/append
    attaching: Function
    attached: Function

    // DOM node detach/remove
    detaching: Function
    detached: Function
  }

  // Extra data
  extra: {
    [key: string]: any
  }
}
```

  Virtual `text` nodes

```ts
text('Content!', hooks, extra)

// Result
interface VTextNode extends VNode {
  content?: string
}
```

  Virtual `element` nodes

```ts
element(
  'tag',
  { /* attrs */ },
  { /* props */ },
  [ /* children */ ],
  hooks,
  extra
)

// Result
interface VElementNode extends VNode {
  tag: string
  attrs: { [key: string]: string }
  props: { [key: string]: any }
  children: VElementNode[]
}
```

  Virtual `custom` nodes

```ts
custom(
  'custom-tag' | {
    definition: CustomElement /* <- Element constructor */,
    args: [ /* ... */ ] /* <- args to be passed to the constructor */
  },
  { /* attrs */ },
  { /* props */ },
  {
    shadow: [ /* shadow children */ ],
    children: [ /* children */ ]
  },
  hooks,
  extra
)

interface VCustomNode extends VElementNode {
  tag: string | {
    definition: Function
    args: any[]
  }

  shadow: any
}
```
