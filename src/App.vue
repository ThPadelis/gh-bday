<template>
  <div id="app">
    <birthday></birthday>
  </div>
</template>

<script>
export default {
  name: "app",
  components: {
    Birthday: () =>
      import(
        /* webpackChunkName: "birthday-component" */ "@/components/Birthday.vue"
      )
  }
};
</script>
