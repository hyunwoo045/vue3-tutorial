# vue-style-loader 적용 안되는 이슈

webpack.config.js 파일 내에 module 에서 css 파일에 대해 vue-style-loader 를 사용할 시 에러가 발생합니다.

```
Uncaught TypeError: Cannot read property 'locals' of undefined
    at eval (App.vue?./node_modules/vue-style-loader/index.js!./node_modules/style-loader/dist/cjs.js!./node_modules/css-loader/dist/cjs.js!./node_modules/vue-loader/dist/stylePostLoader.js!./node_modules/postcss-loader/dist/cjs.js!./node_modules/sass-loader/dist/cjs.js!./node_modules/vue-loader/dist/index.js??ruleSet[1].rules[6].use[0]:7)
    at Object../node_modules/vue-style-loader/index.js!./node_modules/style-loader/dist/cjs.js!./node_modules/css-loader/dist/cjs.js!./node_modules/vue-loader/dist/stylePostLoader.js!./node_modules/postcss-loader/dist/cjs.js!./node_modules/sass-loader/dist/cjs.js!./node_modules/vue-loader/dist/index.js??ruleSet[1].rules[6].use[0]!./src/App.vue?vue&type=style&index=0&id=7ba5bd90&scoped=true&lang=css (main.js:572)
    at __webpack_require__ (main.js:612)
    at fn (main.js:823)
    at eval (VM2330 App.vue:2)
    at Module../src/App.vue?vue&type=style&index=0&id=7ba5bd90&scoped=true&lang=css (main.js:562)
    at __webpack_require__ (main.js:612)
    at fn (main.js:823)
    at eval (VM2330 App.vue:4)
    at Module../src/App.vue (main.js:529)
```

vue-style-loader 를 제외하면 정상적으로 동작하는 것을 확인하였습니다.

<br/>

https://github.com/vuejs/vue-style-loader/issues/56

vue-loader@next 에서 더 이상 vue-style-loader 를 사용하지 않고 style-loader 만을 사용한다는 것으로 파악됩니다. (21.07.20 기준)

<br/><br/>

# 조건문

```html
<div v-if="count > 4">4보다 큽니다.</div>
```

<br/>

# 반복문

```html
<ul>
  <li v-for="fruit in fruits" :key="fruit">{{ fruit }}</li>
</ul>
```

:key 속성을 통해 각각의 태그가 고유하다는 것을 명시해주어야 한다.

</br>

# Style 태그에 SCSS 적용하기

```html
<style lang="scss">
  h1 {
    color: royalblue;
    font-size: 50px;
  }
  ul {
    li {
      font-size: 40px;
    }
  }
</style>
```

# Component 사용하기

Fruit.vue

```html
<template>
  <li>{{ name }}</li>
</template>

<script>
  export default {
    props: {
      name: {
        type: String,
        default: "",
      },
    },
  };
</script>

<style scoped lang="scss">
  h1 {
    color: red !important;
  }
</style>
```

- style 태그에 scoped 속성을 넣어 해당 컴포넌트에서만 스타일이 적용되도록 한다.

<br/>

App.vue

```html
<template>
  <ul>
    <Fruit v-for="fruit in fruits" :key="fruit" :name="fruit">
      {{ fruit }}
    </Fruit>
  </ul>
</template>

<script>
  import Fruit from "~/components/Fruit";

  export default {
    components: {
      Fruit,
    },

    data() {
      return {
        count: 0,
        fruits: ["Apple", "Banana", "Cherry"],
      };
    },
  };
</script>
```

- component 를 import 한다.
- export default 내에서 components 객체를 생성하여 실제 component 를 선언해준다.
- template 에서 컴포넌트 태그를 삽입하여 렌더링 한다.
