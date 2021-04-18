<template>
  <div id="finModal" class="modal">
    <div class="modal-content">
      <div class="close" @click="$emit('close')">&times;</div>
      <h3>{{ isNewAction() ? 'Новая запись' : 'Редактирование' }}</h3>
      <div class="wrapper">
        <input placeholder="Название" v-model="editFinItem.name">
        <input placeholder="Дата" type="date" v-model="editFinItem.date">
        <input placeholder="Сумма" type="number" min="1" v-model.number="editFinItem.amount">
        <div class="radios">
          <label for="r_plus">Доход</label>
          <input type="radio" id="r_plus" :value="finType.PLUS" v-model="editFinItem.type">
          <label for="r_minus">Расход</label>
          <input type="radio" id="r_minus" :value="finType.MINUS" v-model="editFinItem.type">
        </div>
        <textarea placeholder="Описание" v-model="editFinItem.description"></textarea>
        <select v-model="editFinItem.category">
          <option v-for="c in finCategory"
                  v-bind:value="c"
                  v-bind:key="c">
            {{ c }}
          </option>
        </select>
      </div>
      <button class="btn_save" @click="save" :disabled='!isValid'>Сохранить</button>
    </div>
  </div>
</template>

<script>
import {finType, finCategory} from "@/consts";

export default {
  name: "Modal",
  props: {
    editFinItem: {
      type: Object,
      default: function () {
        return {
          type: finType.PLUS,
          category: finCategory.BASE,
        }
      }
    }
  },

  data() {
    return {
      finType,
      finCategory,
    }
  },

  computed: {
    isValid: function () {
      return this.editFinItem.amount > 0
          && this.editFinItem.name.length !== 0
          && this.editFinItem.date !== undefined
          && this.editFinItem.date.length !== 0
    }
  },

  methods: {
    isNewAction() {
      return this.editFinItem.id === undefined
    },

    save() {
      if (this.isNewAction()) {
        this.$emit('createFin', this.editFinItem)
      } else {
        this.$emit('commitEdit', this.editFinItem)
      }
    }
  }
}
</script>

<style scoped>
.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.4);
}


.modal-content {
  background-color: #fefefe;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin: 15% auto;
  padding: 10px 20px;
  border: 1px solid #888;
  border-radius: 5px;
  width: 50%; /* Could be more or less, depending on screen size */
}

.radios {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.radios input {
  margin-right: 10px;
}

.radios label {
  margin-left: 5px;
}

.wrapper {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-row-gap: 10px;
  grid-column-gap: 5px;
  width: 100%;
}

h3 {
  margin-block-start: 2px;
}

/* The Close Button */
.close {
  align-self: flex-end;
  color: #aaa;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

.btn_save {
  cursor: pointer;
  padding: 5px;
  margin: 5px;
  color: dodgerblue;
  background: white;
  border-radius: 5px;
  outline: none;
}

.btn_save:disabled {
  color: grey;
}

.btn_save:hover {
  background: whitesmoke;
}

.btn_save:active {
  background: lightgray;
}

</style>