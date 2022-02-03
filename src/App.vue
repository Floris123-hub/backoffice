<template>
  <div id="app">
    <conversations></conversations>
    <router-view/>
  </div>
</template>

<style lang="scss">

</style>
<script>
  import Conversations from "./components/Conversations";
  export default {
    components: {Conversations}
  }
</script>