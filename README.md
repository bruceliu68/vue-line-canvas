# vue-line-canvas

# Install
npm install vue-line-canvas

# How to use

```html
<template>
  <div>
      <LineCanvas :canvasWidth="canvasWidth" 
                  :canvasHeight="canvasHeight"
                  :dotsNum="dotsNum"
                  :isColor="isColor">
    </LineCanvas>
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
      canvasWidth: window.innerWidth, // canvas width
      canvasHeight: window.innerHeight, // canvas height
      dotsNum: 50, // dot number
      isColor: true, // is color
    }
  }
}
</script>
```