<template>
  <div id="app">
    <Modal v-if="showModal" @close="showModal = false" :finance="allFinances[0]"></Modal>
    <Counter :balance="calcBalance"></Counter>
    <FinanceItem
        v-for="finance in allFinances"
        :key="finance.id"
        :finance="finance"
        :is-minus="finance.type === finType.MINUS">
    </FinanceItem>
    <button class="add_btn" @click="addFinance">Добавить расход/доход</button>
  </div>
</template>

<script>
import Counter from './components/Counter'
import FinanceItem from "./components/FinanceItem";
import Modal from "./components/Modal"
import {finType} from "./consts.js"

const FIN_SAVE_KEY = "fin"

export default {
  name: 'App',
  components: {
    Counter,
    FinanceItem,
    Modal
  },

  data() {
    return {
      finType,
      showModal: false,

      categories: [
        "Еда",
        "Транспорт",
        "Одежда"
      ],

      allFinances: [
        {
          id: 1,
          name: "some",
          amount: 124,
          date: "2021",
          type: finType.PLUS,
          description: "akjshgklasjriwquotqnk"
        },
        {
          id: 2,
          name: "some",
          amount: 12,
          date: "2021",
          type: finType.MINUS,
          description: "akjshgklasjriwquotqnk"
        }
      ]
    }
  },

  computed: {
    calcBalance() {
      let balance = 0

      this.allFinances.forEach(f => {
        balance += f.type === finType.PLUS ? f.amount : -f.amount
      })

      return balance
    }
  },

  watch: {
    allFinances: function (val) {
      localStorage.setItem(FIN_SAVE_KEY, JSON.stringify(val))
    }
  },

  created() {
    let allFinances = localStorage.getItem(FIN_SAVE_KEY)
    if (allFinances == null) {
      return
    }

    this.allFinances = JSON.parse(allFinances)
  },

  methods: {
    getNewID() {
      let id = -1
      this.allFinances.forEach(f => {
        if (f.id > id) id = f.id
      })

      return ++id
    },

    addFinance() {
      this.showModal = true
      // this.allFinances.push({
      //   id: this.getNewID(),
      //   name: "some",
      //   amount: 12125,
      //   date: new Date().toLocaleString(),
      //   type: finType.MINUS,
      //   description: "akjshgklasjriwquotqnk"
      // })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.add_btn {
  padding: 5px;
  margin: 5px;
  color: dodgerblue;
  background: white;
  border-radius: 5px;
}

</style>
