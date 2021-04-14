<template>
  <div id="finModal" class="modal">
    <div class="modal-content">
      <div class="close" @click="$emit('close')">&times;</div>
      <input placeholder="название">
      <input placeholder="сумма">
      <span>
        <input type="radio" id="r_plus" :value="finType.PLUS" v-model="finItem.type">
        <label for="r_plus">Доход</label>
        <input type="radio" id="r_minus" :value="finType.MINUS" v-model="finItem.type">
        <label for="r_minus">Расход</label>
      </span>
      <textarea placeholder="описание"></textarea>
    </div>
  </div>
</template>

<script>
import {finType} from "@/consts";

export default {
  name: "Modal",
  props: ['editFinItem'],

  data() {
    return {
      finType,
      finItem: {}
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

/* Modal Content/Box */
.modal-content {
  background-color: #fefefe;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  border-radius: 5px;
  width: 55%; /* Could be more or less, depending on screen size */
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
</style>