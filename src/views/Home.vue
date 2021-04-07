<template>
  <div class="home">
    <bars />
  </div>
</template>

<script>
// @ is an alias to /src
import Bars from '@/components/Bars.vue'

export default {
  name: 'Home',
  components: {
    Bars
  }
}
</script>
