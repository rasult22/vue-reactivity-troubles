#  Vue 2.6 Reactivity Troubles
##### *This repo is created for collecting vue 2.6 reactivity issues and for collecting solving techniques for them.* 



#### Common cases when Vue's reactivity breaks:

##### 1. Assigning to an array by index:
Code Sandbox: https://codesandbox.io/s/youthful-river-rti6p?file=/src/components/Arrays.vue

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
      
      // Solving approaches:
      // 1. Use "this.$set" - only for a spicific index
      // this.$set(this.arr, 0, this.arr[0]);
      
      // OR USE:
      
      // 2. Reassign a copy of itself  (It'll make everying inside an array reactive again)
      // this.arr = this.arr.map((x) => x);
      
      
      // 3. Use only .push/.slice Array methods to manipulate an array instead.
    },
  },

  created() {
   // Imagine this happened somewhere in the Vuex commits/dispatches
   
   // This assign approach won't make an object or primitive inside an array reactive
    this.arr[0] = {
      increment: 0,
    };
    this.arr[1] = 2;
  },
};
</script>

```
##### -------------------------------------------------------------------------------------------------------------------------------------------------------------------

##### 2. Assining a value to an uninitialized object prop (Vue.set jokes) :
Sandbox: https://codesandbox.io/s/breaking-vue-reactivity-rti6p?file=/src/components/Objects.vue
```Vue
<template>
  <div>
    <pre> {{ person }} </pre>
    <button @click="mutate">Change last name</button>
    <br />
    <!-- Computed property based on our person object -->
    Computed prop:<b>
      {{ getFullName }}
    </b>
  </div>
</template>

<script>
export default {
  data() {
    return {
      person: {
        name: "Johnatan",
        age: 20,
      },
    };
  },
  methods: {
    mutate() {
      // Feel free to comment/uncomment code to check all the statements

      // This won't work (because "last" prop isn't reactive)
      this.person.last = "Beckham";

      // ! IMPORTANT to notice
      // Even this example won't work, because the property "last" was initialized before (in "created" hook or in line 91).
      // this.$set(this.person, "last", "Beckham"); // Works only if prop "last" wasn't initialized before

      // Solving approaches:
      // 1. Reassign the copy of an object to itself
      // this.person = { ...this.person, last: "Beckham" };

      // 2. Same as first, but with Object.assign
      // this.person = Object.assign({}, this.person, { last: "Beckham" });
      
    },
  },
  computed: {
    getFullName() {
      // Computed property won't react to changes in "last" property while it isn't reactive
      return this.person.name + " " + this.person.last;
    },
  },
  created() {
    // Imagine this could happen in Vuex state (in dispaches, mutations). And you using this object in computed property
    this.person.last = "Stone";
  },
};
</script>

```


