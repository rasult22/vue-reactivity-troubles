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
      
      // Solving approach:
      // 1. Use this.$set  -  only for a spicific index
      // this.$set(this.arr, 0, this.arr[0]);

      // 2. Reassign a copy of itself  (It'll make everying inside an array reactive again)
      // this.arr = this.arr.map((x) => x);
    },
  },

  created() {
   // Imagine this happened somewhere in the Vuex commits/dispatches
    this.arr[0] = {
      increment: 0,
    };
    this.arr[1] = 2;
  },
};
</script>

```
