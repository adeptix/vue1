<template>
  <div class="counter">
    Баланс: {{balance}}
  </div>
</template>

<script>
export default {
  name: "Counter",
  props: ['balance']
}
</script>

<style scoped>

</style>