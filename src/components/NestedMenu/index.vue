<template lang="">
  <div>
    递归菜单
    <MenuItem :data="data"></MenuItem>
  </div>
</template>
<script>
import data from './menu'
import MenuItem from './MenuItem.vue'
export default {
  setup() {
    let d = data

    setTimeout(() => {
      d = data
    },1000)
    return {
      data: d
    }
  },
  components: {
    MenuItem
  }
}
</script>
<style lang="">
  
</style>