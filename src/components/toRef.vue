<template lang="">
  <div>{{refValue}} {{toRefValue}}</div>
</template>
<script>
import { ref, toRef, toRefs,reactive } from "vue";

export default {
  setup() {
    const obj = { age: 12 };
    //初始化设置个响应式变量tor,函数中读取值要tor.value
    let refValue = ref(0);
    //这里将对象转化成响应性，并设置key值,函数中读取值要toR._object
    let toRefValue = toRef(obj, "toR");
    const state = reactive({
      num: 1,
      name: "baby张",
    });
    const refsObj = toRefs(state)
    console.log(refValue, toRefValue, refsObj);
    return {
      refValue,
      toRefValue,
      //toRefs针对的是使用了reactive的响应式对象，可以理解为将对象拆分成多个响应式ref，外界可以读取到响应式的所有属性
      ...refsObj,
    };
  },
};
</script>
<style lang=""></style>
