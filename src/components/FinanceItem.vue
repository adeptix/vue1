<template>
  <div class="fin_item">
    <div class="fin_main">
      <div>{{ finance.name }}</div>
      <div v-bind:class="isMinus(finance.type) ? 'minus' : 'plus'">
        {{ isMinus(finance.type) ? '-' : '+' }}
        {{ finance.amount }}
      </div>
      <div class="date">{{ finance.date }}</div>
      <div v-bind:style="{ background: categoryColor(finance.category) }" class="category">{{ finance.category }}</div>
      <div class="description">{{ finance.description }}</div>
    </div>
    <div class="fin_actions">
      <button @click="$emit('startEdit', finance.id)">
        <img src="../assets/edit.svg" alt="edit" class="icon"/>
      </button>
      <button @click="$emit('startDelete', finance.id)">
        <img src="../assets/delete.svg" alt="delete" class="icon">
      </button>
    </div>
  </div>
</template>

<script>
import {finType, finCategory} from "@/consts";

export default {
  name: 'FinanceItem',
  props: ['finance'],

  methods: {
    isMinus(type) {
      return type === finType.MINUS
    },

    categoryColor(category){
      switch (category){
        case finCategory.CLOTHES:
          return '#E57373'
        case finCategory.FOOD:
          return '#66BB6A'
        case finCategory.TRANSPORT:
          return '#80DEEA'
        default:
          return '#E0F7FA'
      }
    }
  }
}
</script>

<style scoped>
.fin_item {
  width: 90%;
  display: flex;
  align-items: center;
  border-radius: 5px;
  border: 1px solid black;
  margin: 5px;
  box-sizing: border-box;
}

.fin_main {
  flex: 4;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  margin: 5% 5px 5px 5px;
  grid-row-gap: 10px;
}

.date {
  color: grey;
  font-size: 12px;
}


.category {
  font-size: 12px;
  padding: 5px;
  border-radius: 5px;
  display: inline-block;
  margin: auto;
}

.description {
  font-size: 13px;
  padding: 5px;
  grid-column: 1 / -1
}

.fin_actions {
  flex: 1;
  border-left: 1px solid lightgray;
  padding: 15px 5px;
}

.fin_actions button {
  border: none;
  background: none;
  cursor: pointer;
  border-radius: 5px;
  outline: none;
}

.fin_actions button:hover {
  background: lightgray;
}

.fin_actions button:active {
  background: darkgray;
}

.icon {
  max-width: 15px;
}

.minus {
  color: red;
}

.plus {
  color: green;
}
</style>