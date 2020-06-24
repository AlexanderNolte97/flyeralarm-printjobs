<template>
  <div id="app">
    <JsonRenderer />
  </div>
</template>

<script>
import JsonRenderer from "./components/JsonRenderer";
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

export default {
  name: 'App',
  components: {
    JsonRenderer
  }
}
</script>

<style>

</style>
