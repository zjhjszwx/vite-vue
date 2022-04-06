<template lang="">
  <div class="wrap">
    <div class="menu-wrap" :class="`menu-wrap-${depth}`">
      <div
        class="menu-item"
        v-for="menuItem in data"
        :class="activeId === menuItem.id ? 'actived' : ''"
        @click="onMenuItemClick(menuItem)"
      >
        {{ menuItem.name }}
      </div>
    </div>
    <MenuItem
      v-if="subMenu && subMenu.length"
      :data="subMenu"
      :depth="depth + 1"
      @change="onSubActiveIdChange"
    >
    </MenuItem>
  </div>
</template>
<script>
import { ref, toRefs, watch, watchEffect, computed } from 'vue'
export default {
  name: 'MenuItem',
  props: ['data', 'depth', 'activeIds'],
  setup(props, context) {
    // context 不是响应式，可以解构，只能访问 props attrs slots emit
    const { data, depth = 0, activeIds } = props
    const activeId = ref(null)

    console.log(data)

    // watch(
    //   () => activeIds,
    //   (newActiveIds) => {
    //     if (newActiveIds) {
    //       const newActiveId = newActiveIds[depth];
    //       if (newActiveId) {
    //         activeId.value = newActiveId;
    //       }
    //     }
    //   },
    //   {
    //     immediate: true,
    //     flush: 'sync'
    //   }
    // );

    // 因为 props 是响应式的，你不能使用 ES6 解构，它会消除 prop 的响应性。
    watch(() => props.data,
      (newData) => {
        if(!activeId.value){
          if(newData && newData.length){
            activeId.value = newData[0].id
          }
          
        }
        // 如果当前层级的 data 中遍历无法找到 `activeId` 的值 说明这个值失效了
        // 把它调整成数据源中第一个子菜单项的 id
        if (!props.data.find(({ id }) => id === activeId.value)) {
          activeId.value = props.data?.[0].id;
        }
      },
      {
        immediate: true,
        flush: 'sync' // 强制触发
      }
    )

    console.log(activeId.value)

    // 获取当前菜单
    const getActiveSubMenu = () => {
      const node = data.find((item) => item.id === activeId.value)
      return node && node._child
    }
    const subMenu = computed(getActiveSubMenu)

    const getSubIds = (child) => {
      const subIds = []
      const traverse = (data) => {
        if (data && data.length) {
          const first = data[0]
          subIds.push(first.id)
          traverse(first._child)
        }
      }
      traverse(child)
      return subIds
    }

    const onMenuItemClick = (menuItem) => {
      const newActiveId = menuItem.id
      if(newActiveId !== activeId.value) {
        // 获取整个链路的id，用来数据请求
        activeId.value = menuItem.id
        const child = getActiveSubMenu()
        const subIds = getSubIds(child)
        context.emit('change', [newActiveId, ...subIds])
      }
    }

    // 如果在中间菜单，还需要继续传递
    const onSubActiveIdChange = (ids) => {
      context.emit('change', [activeId].concat(ids))
    }

    return {
      subMenu,
      activeId,
      onMenuItemClick,
      onSubActiveIdChange,
      depth
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

  .actived {
    color: red;
  }

  .menu-wrap-0 {
    background: #ffccc7;
  }

  .menu-wrap-1 {
    background: #fff7e6;
  }

  .menu-wrap-2 {
    background: #fcffe6;
  }
  
}
</style>