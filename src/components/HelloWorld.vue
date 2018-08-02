<template>
  <div>
      <LineCanvas :canvasWidth="canvasWidth" :canvasHeight="canvasHeight"></LineCanvas>
  </div>
</template>

<script>
import LineCanvas from 'vue-line-canvas';

export default {
  name: 'HelloWorld',
  components: {
    LineCanvas,
  },
  data () {
    return {
      canvasWidth: window.innerWidth,
      canvasHeight: window.innerHeight,
    }
  }
}
</script>
