<template>
  <div id="app">
    <Map type="flowInto" my-ref="line1" />
    <Map type="outflow" my-ref="line2" />
<!--		<Ball />-->
  </div>
</template>

<script>
import Map from './components/Map.vue'
import Ball from './components/ball.vue'

export default {
  name: 'App',
  components: {
    Map,
		Ball
  }
}
</script>

<style lang="scss">
</style>
