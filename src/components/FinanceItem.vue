<template>
  <div class="fin_item">
    <div class="fin_main">
      <div class="fin_header">
        <div>{{ finance.name }}</div>
        <div v-bind:class="isMinus ? 'minus' : 'plus'">{{ isMinus ? '-' : '+' }}{{ finance.amount }}</div>
      </div>
      <div class="fin_additional">
        <div class="date">{{ finance.date }}</div>
        <div>{{ finance.category }}</div>
      </div>
      <div class="description">{{ finance.description }}</div>
    </div>
    <div class="fin_actions">
      <button>
        <img src="../assets/edit.svg" alt="edit" class="icon"/>
      </button>
      <button>
        <img src="../assets/delete.svg" alt="delete" class="icon">
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FinanceItem',
  props: ['finance', 'isMinus']
}
</script>

<style scoped>
.fin_item {
  width: 45%;
  display: flex;
  align-items: center;
  border-radius: 5px;
  border: 1px solid black;
  margin: 5px;
}

.fin_main {
  flex: 4;
}

.fin_header {
  display: flex;
  justify-content: space-between;
  padding: 5px;
  margin: 5% 15%;
}

.fin_additional {
  display: flex;
  justify-content: start;
  padding: 5px;
}

.date {
  color: grey;
  font-size: 12px;

}

.description {
  text-align: start;
  font-size: 13px;
  padding: 5px;
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