<script setup>
import { ref } from 'vue'

defineProps({
  msg: String
})

const count = ref(0)
</script>

<template>
  <h1 class="h1">{{ count }}</h1>
</template>

<style lang="scss" scoped>
.h1 {
  color: #42b983;
}
</style>
