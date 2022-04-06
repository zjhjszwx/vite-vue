<template lang="">
  <div>
    递归菜单
    <MenuItem :data="data" @change="activeIdsChange" :depth="0" :activeIds="ids"></MenuItem>
  </div>
</template>
<script>
import data from './menu'
import MenuItem from './MenuItem.vue'
import { ref, onMounted } from 'vue'
export default {
  setup() {
    // 假设默认选中 id 为 7
    const activeId = 7;

    const findPath = (menus, targetId) => {
      let ids;

      const traverse = (subMenus, prev) => {
        if (ids) {
          return;
        }
        if (!subMenus) {
          return;
        }
        subMenus.forEach((subMenu) => {
          if (subMenu.id === activeId) {
            ids = [...prev, activeId];
            return;
          }
          traverse(subMenu._child, [...prev, subMenu.id]);
        });
      };

      traverse(menus, []);

      return ids;
    };

    const ids = ref(findPath(data, activeId));

    const activeIdsChange = (newIds) => {
      ids.value = newIds;
      console.log("当前选中的id路径", newIds);
    };

    return {
      ids,
      activeIdsChange,
      data: data,
    };
  },
  components: {
    MenuItem
  },
  methods: {
    
  }
}
</script>
<style lang="">
  
</style>