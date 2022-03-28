<template>
  <h1>{{ msg }}</h1>
  <button @click="increment">
    count: {{ state.count }}, double: {{ state.double }},three：{{
      three
    }},refnum：{{ refnum }}
  </button>
</template>

<script>
import { ref, reactive, computed, watchEffect, watch, getCurrentInstance } from "vue";
export default {
  props: {
    msg: String,
  },
  setup(props, ctx) {
    // console.log(props, ctx, getCurrentInstance());
    const state = reactive({
      count: 0,
      double: computed(() => {
        return state.count * 2;
      }),
    });

    // 计算属性更加灵活
    const three = computed(() => {
      return state.count * 3;
    });

    const refnum = ref();

    // 1. watch需要参数，watchEffect不需要传入参数
    // 2. watch只有监听属性发生变化才执行，watchEffect第一次就会自行
    // 3. 都无法监听未被绑定的属性
    watchEffect(() => {
      refnum.value = state.count;
      console.log("watchEffect==", state);
    });

    watch(refnum, (current, prev) => {
      console.log("watch==", current, prev);
    })

    function increment() {
      state.count++;
    }

    return {
      state,
      refnum,
      three,
      increment,
    };
  },
};
</script>

<style lang="scss" scoped>
.h1 {
  color: #42b983;
}
</style>
