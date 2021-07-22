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

## 인스턴스와 라이프사이클

https://v3.ko.vuejs.org/images/lifecycle.svg

- 중요한 것은 created, mounted
- created 는 인스턴스가 생성된 후에 동작하는 메서드
- mounted 는 HTML 과 연결이 된 후 동작하는 메서드

beforeCreate - created - beforeMount - mounted

- beforeCreate 전에는 data 부분이 동작하기 전이므로 data 에서 설정한 스테이트들을 출력하면 undefined 가 출력된다.

```html
<script>
  export default {
    data() {
      return {
        count: 2
      }
    }
    beforeCreate() {
      console.log('Before Create!', this.count) // -> 'Before Create!' undefined
    }
    created() {
      console.log('Created!', this.count) // 'Created!', 2
    }
  }
</script>
```

- beforeMount 전에는 HTML 과 연결이 되기 전이므로 template 부분에 작성한 태그들을 출력해보면 null 이 출력된다.

```html
<template>
  <h2>Hello</h2>
</template>
<script>
  export default {
    beforeMount() {
      console.log(document.querySelector('h2')) // -> null
    }
    mounted() {
      console.log(document.querySelector('h2')) // -> <h2> Hello</h2>
    }
  }
</script>
```

- 당연히 beforeCreate, created 에서 querySelector 를 통해 태그를 가져오더라도 null 이 출력된다.
