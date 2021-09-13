> composition api
1. setup
  由于在执行 setup 时尚未创建组件实例，因此在 setup 选项中没有 this
  参数:
    props: 响应式, 不能使用 ES6 解构, 因为它会消除 prop 的响应性
    context: attrs(非响应式对象), slots(非响应式对象), emit
  
2. onMounted, onUnmounted, onUpdated, onErrorCaptured
3. watch
```
const counter = ref(0)
watch(counter, (newValue, oldValue) => {
  console.log('The new counter value is: ' + counter.value)
})
```
4. computed
```
const counter = ref(0)
const twiceTheCounter = computed(() => counter.value * 2)
```
5. ref
6. toRefs
7. reactive
8. provid/inject & reactive

> 插槽
```
  1. 父组件 <template v-slot:operate="{ scope }"></template>
  2. 子组件 <slot name="operate" :scope="">
```

> Hooks

> 生命周期
