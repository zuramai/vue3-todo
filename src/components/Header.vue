<template>
  <h1>{{ msg }}</h1>
</template>

<script>
export default {
  name: 'Header',
  props: {
    msg: String
  },
  data() {
    return {
      count: 0
    }
  }
}
</script>
