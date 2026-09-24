<template>
  <div id="app">
<!--    <emp-view></emp-view>-->
    <router-view/>
  </div>
</template>

<script>
import EmpView from './views/tail/EmpView.vue'

export default {
  components: {EmpView },
  data(){
    return {
      "message":"hello world"
    }
  }
}
</script>

<style>

</style>
