## 一、基本功能

Bootstrap dropdown 样式文档地址： https://v5.getbootstrap.com/docs/5.0/components/dropdowns/

### 1、创建 Dropdown.vue 组件

```vue
<template>
  <div class="dropdowm">
    <a href="#" class="btn btn-outline-light my-2 dropdown-toggle">
      {{ title }}
    </a>
    <ul class="dropdown-menu">
      <li class="dropdown-item">
        <a href="#">新建文章</a>
      </li>
      <li class="dropdown-item">
        <a href="#">编辑资料</a>
      </li>
    </ul>
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name:'Dropdown',
  props:{
    title:{
      type:String,
      required:true
    }
  }
})
</script>
```

### 2、在 GlobalHeader 中引入组件

```javascript
import Dropdown from './Dropdown.vue'

export default defineComponent({
  name: 'GlobalHeader',
  components: { Dropdown },
  ...
})
```

### 3、修改页面内容

```html
<!--修改前-->
<ul v-else class="list-inline mb-0">
    <li class="list-inline-item">
        <a href="#" class="btn btn-outline-light my-2">你好 {{ user.name }}</a>
    </li>
</ul>
<!--修改后-->
<ul v-else class="list-inline mb-0">
    <li class="list-inline-item">
        <dropdown :title="`你好 ${user.name}`"></dropdown>
    </li>
</ul>
```

### 4、添加控制变量

```javascript
  setup() {
    const isOpen = ref(false)
    const toggleOpen = () => {
      isOpen.value = !isOpen.value
    }
    return {
      isOpen,
      toggleOpen
    }
  }
```

### 5、添加样式

```html
<!--添加点击事件-->
<a href="#" class="btn btn-outline-light my-2 dropdown-toggle" @click.prevent="toggleOpen">
    {{ title }}
</a>
<!--添加样式以及控制开关-->
<ul class="dropdown-menu" :style="{ display: 'block' }" v-if="isOpen">
	...
</ul>
```

## 二、添加 DropdownItem

### 1、创建 DropdownItem 组件

```vue
<template>
  <li class="dropdown-option" :class="{ 'is-disabled': disabled }">
    <slot></slot>
  </li>
</template>
<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    disabled: {
      type: Boolean,
      default: false
    }
  }
})
</script>
<style>
.dropdown-option.is-disabled * {
  color: #6c757d;
  pointer-events: none;
  background-color: transparent;
}
</style>
```

### 2、修改 Dropdown 组件内容

```html
<!--添加点击事件-->
<ul class="dropdown-menu" :style="{ display: 'block' }" v-if="isOpen">
    <li class="dropdown-item">
        <a href="#">新建文章</a>
    </li>
    <li class="dropdown-item">
        <a href="#">编辑资料</a>
    </li>
</ul>
<!--添加样式以及控制开关-->
<ul class="dropdown-menu" :style="{ display: 'block' }" v-if="isOpen">
    <slot></slot>
</ul>
```

### 3、在 GlobalHeader 中引入 DropdownItem

```javascript
import DropdownItem from './DropdownItem.vue'
```

### 4、页面添加组件

```html
<ul v-else class="list-inline mb-0">
    <li class="list-inline-item">
        <dropdown :title="`你好 ${user.name}`">
            <dropdown-item><a href="#" class="dropdown-item">新建文章</a></dropdown-item>
            <dropdown-item disabled><a href="#" class="dropdown-item">编辑资料</a></dropdown-item>
            <dropdown-item><a href="#" class="dropdown-item">退出登录</a></dropdown-item>
        </dropdown>
    </li>
</ul>
```

### 5、点击外部区域自动隐藏

**5.1 思路**

1. 在`mounted`时候添加`click`事件，在`unmounted`的时候将事件删除
2. 拿到`Dropdown`的`DOM`元素从而判断，点击的内容是否被这个元素包含

**5.2 给模版添加 ref 属性**

```html
<div class="dropdown" ref="dropdownRef">
```

**5.3 函数实现**

```javascript
const dropdownRef = ref<null | HTMLElement>(null)
const handler = (e: MouseEvent) => {
    if (dropdownRef.value) {
        if (!dropdownRef.value.contains(e.target as HTMLElement) && isOpen.value) {
            isOpen.value = false
        }
    }
}
onMounted(() => {
    document.addEventListener('click', handler)
})
onUnmounted(() => {
    document.removeEventListener('click', handler)
})
return {
    isOpen,
    toggleOpen,
    // 返回和 ref 同名的响应式对象，就可以拿到对应的 dom 节点
    dropdownRef
}
```