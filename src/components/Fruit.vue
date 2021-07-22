<template>
  <li> {{ name }}</li>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      default: '',
    }
  }
}
</script>

<style scoped lang="scss"> // scoped 를 작성하면 다른 컴포넌트에 영향을 주지 않는다.
  h1 {
    color: red !important;
  }
</style>
