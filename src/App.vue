<template>
  <div id="app">
    <Modal
        v-if="showModal"
        @close="cancelEdit"
        :edit-fin-item="editFinItem"
        v-on:createFin="createFin"
        v-on:commitEdit="commitEdit"
    ></Modal>

    <div class="info_header">
      <Counter :balance="calcBalance"></Counter>
      <Category v-on:changeCategory="changeCategory"></Category>
    </div>

    <div class="fin_list">
      <FinanceItem
          v-for="finance in financesByCategory"
          :key="finance.id"
          :finance="finance"
          v-on:startEdit="startEdit"
          v-on:startDelete="startDelete">
      </FinanceItem>
    </div>
    <button class="add_btn" @click="showModal = true">Добавить расход/доход</button>
  </div>
</template>

<script>
import Counter from './components/Counter'
import FinanceItem from "./components/FinanceItem";
import Modal from "./components/Modal"
import {finType} from "./consts.js"
import Category from "@/components/Category";

const FIN_SAVE_KEY = "fin"

export default {
  name: 'App',
  components: {
    Category,
    Counter,
    FinanceItem,
    Modal
  },

  data() {
    return {
      finType,
      editFinItem: undefined,
      showModal: false,
      showingCategory: 'all',


      allFinances: []
    }
  },

  computed: {
    calcBalance() {
      let balance = 0

      this.allFinances.forEach(f => {
        balance += f.type === finType.PLUS ? f.amount : -f.amount
      })

      return balance
    },

    financesByCategory() {
      if (this.showingCategory === "all") return this.allFinances

      let showingCategory = this.showingCategory

      return this.allFinances.filter(function (fin) {
        return fin.category === showingCategory
      })
    }
  },

  watch: {
    allFinances: {
      handler: function () {
        localStorage.setItem(FIN_SAVE_KEY, JSON.stringify(this.allFinances))
      },
      deep: true
    },
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

    createFin(finance) {
      finance.id = this.getNewID()
      this.allFinances.push(finance)
      this.showModal = false
    },

    startDelete(id) {
      for (let i = 0; i < this.allFinances.length; i++) {
        if (this.allFinances[i].id === id) {
          this.allFinances.splice(i, 1)
          return
        }
      }
    },

    startEdit(id) {
      this.editFinItem = Object.assign({}, this.allFinances.find(value => value.id === id))
      this.showModal = true
    },

    cancelEdit() {
      this.showModal = false
      this.editFinItem = undefined
    },

    commitEdit(newItem) {
      let foundIndex = this.allFinances.findIndex(value => value.id === newItem.id)
      this.allFinances.splice(foundIndex, 1, newItem)

      this.showModal = false
      this.editFinItem = undefined
    },

    changeCategory(category) {
      this.showingCategory = category
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
  outline: none;
}

.add_btn:hover {
  background: whitesmoke;
}

.add_btn:active {
  background: lightgray;
}

.fin_list {
  max-height: 550px;
  min-height: 550px;
  height: 550px;
  overflow-y: auto;
  width: 45%;
  align-items: center;
  display: flex;
  flex-direction: column;
  border-bottom: 1px solid dodgerblue;
  border-top: 1px solid dodgerblue;
  margin: 5px;
}

.info_header {
  display: flex;
  width: 45%;
  justify-content: space-around;
}

</style>
