<template>
  <div>{{name}}</div>
</template>

<script>
import { defineComponent } from '@nuxtjs/composition-api'

export default defineComponent({
  setup() {
    return {name: 'ABC'}
  }
})
</script>
