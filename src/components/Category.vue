<template>
  <select id="category_select" v-model=selected @change="$emit('changeCategory', selected)">
    <option value="all">Все</option>
    <option v-for="c in finCategory"
            v-bind:value="c"
            v-bind:key="c">
      {{ c }}
    </option>
  </select>
</template>

<script>
import {finCategory} from "@/consts";

export default {
  name: "Category",

  data() {
    return {
      finCategory,
      selected: "all"
    }
  }
}
</script>

<style scoped>
#category_select {
  margin: 5px;
}
</style>