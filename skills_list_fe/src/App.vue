<template>
  <!--<router-view/> required to route to views - add to each template
  unless included here-->
  <router-view />
</template>
<script>
import router from "./router";
export default {
  name: 'app',
  props: {
    msg: String
  },
  mounted() {
    router.push("/skill-list");
  }
}
</script>
<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
