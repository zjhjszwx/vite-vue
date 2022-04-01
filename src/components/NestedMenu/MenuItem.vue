<template lang="">
  <div class="wrap">
    <div class="menu-wrap">
      <div
        class="menu-item"
        v-for="menuItem in data"
      >
        {{ menuItem.name }}
      </div>
    </div>
    <MenuItem
      v-if="subMenu"
      :data="subMenu"
    >
    </MenuItem>
  </div>
</template>
<script>
import { ref, watch, watchEffect, computed } from 'vue'
export default {
  name: 'MenuItem',
  props: ['data'],
  setup(props) {
    const { data } = props
    const activeId = ref(null)
    watch(() => props.data,
      (newData) => {
        if(!activeId.value){
          if(newData && newData.length){
            activeId.value = newData[0].id
          }
        }
      },
      {
        immediate: true,
        flush: 'sync' // 强制触发
      }
    )


    const getActiveSubMenu = () => {
      const node = data.find((item) => item.id === activeId.value)
      console.log(node, activeId.value)
      return node && node._child
    }
    const subMenu = computed(getActiveSubMenu)

    return {
      subMenu
    }
    
  }

}
</script>
<style lang="scss" scoped>
.wrap {
  .menu-wrap {
    display: flex;
    flex-wrap: wrap;
  }
  .menu-item {
    padding: 10px;
    cursor: pointer;
  }
  
}
</style>