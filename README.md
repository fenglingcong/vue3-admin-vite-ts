# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## Recommended IDE Setup

- [VS Code](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar)

## vite

### import.meta.glob vite动态导入文件

该方法匹配到的文件默认是懒加载，通过动态导入实现，构建时会分离独立的 chunk，是异步导入，返回的是 Promise，需要做异步操作
```
const Components = import.meta.glob('../components/**/*.vue');

// 转译后：
const Components = {
  './components/a.vue': () => import('./components/a.vue'),
  './components/b.vue': () => import('./components/b.vue')
}
```

### import.meta.globEager

该方法是直接导入所有模块，并且是同步导入，返回结果直接通过 for...in循环就可以操作
```
const Components = import.meta.globEager('../components/**/*.vue');

// 转译后：
import * as __glob__0_0 from './components/a.vue'
import * as __glob__0_1 from './components/b.vue'
const modules = {
  './components/a.vue': __glob__0_0,
  './components/b.vue': __glob__0_1
}
```

### 异步导入vue3组件
defineAsyncComponent
```
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() =>
  import('./components/AsyncComponent.vue')
)

app.component('async-component', AsyncComp)
```

### vite别名配置 Typescript警告路径不存在
vite.config.js
```
export default defineConfig({
  base: './',
  resolve: {
    alias: {
      "@": path.join(__dirname, "./src")
    },
  }
  // 省略其他配置
})
```

tsconfig.js中 添加compilerOptions.paths的配置
```
{
  'compilerOptions': {
    'path': {
      '@': ['./src/*']
    }
  }
}
```

### vite配置全局css
```
export default defineConfig({
  base: './',
  css: {
    preprocessorOptions: {
      // 添加公共样式
      scss: {
        additionalData: '@import "./src/style/style.scss";'
      }

    }
  },
  plugins: [vue()]
  // 省略其他配置
})

export default defineConfig({
  css: {
    preprocessorOptions: {
      scss: {
        additionalData: '$primary: #993300'
      }
    }
  }
})
```