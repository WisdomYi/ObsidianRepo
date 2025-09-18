
## 响应式
### ref
- 把特定值包装在value属性中实现响应式
- 默认深层响应 ,可以用shallow ref 放弃深层响应性
- 
```vue
import {ref} from vue
const count = ref(0)
count.value //ref对象的value属性存放数据
```
```template
<div>{{ count }}</div>
<button @click="count++"> {{ count }} </button>
```

### reactive
使得对象本身具有响应性
默认深层响应  可以用`shallowReactive()`放弃深层响应性
reactive返回原始对象的Proxy  并不等于原始对象
只有代理对象是响应性的
仅支持对象,数组 不支持原始对象
```vue
import { reactive } from 'vue'
const state = reactive({count:0})
```
```template
<button @click="state.count++"> {{ state.count }} </button>
```
```
const state = reactive({ count: 0 }) 
// 当解构时，count 已经与 state.count 断开连接 
let { count } = state 
// 不会影响原始的 state.count++ 
// 该函数接收到的是一个普通的数字 
// 并且无法追踪 state.count 的变化 
// 我们必须传入整个对象以保持响应性 
callSomeFunction(state.count)
```