#  Vue 2.6 Reactivity Troubles
##### *This repo is created for collecting vue 2.6 reactivity issues and for collecting solving techniques for them.* 



#### Common approaches to break Vue's reactivity:

##### 1. Assign to an array by index:
```vue
<template>
  <div>
    <pre>{{ arr }}</pre>
    <button @click="mutate">increment</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [],
    };
  },
  methods: {
    mutate() {
      // Won't update the view, because an object inside an array is not reactive
      this.arr[0].increment += 1; // Reference assign
      this.arr[1] += 1; // Primitive assign
    },
  },

  created() {
    this.arr[0] = {
      increment: 0,
    };
    this.arr[1] = 2;
  },
};
</script>

```
