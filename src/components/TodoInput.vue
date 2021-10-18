<template>
  <section class="inputArea">
    <span class="flagBtn fa fa-flag" :class="{'setFlag' : flag}" @click="setFlag"></span>
    <span class="inputBox">
      <input type="text" ref="mainInput" v-model="newItem" placeholder="やることを入力てください!" @click="clearInput" @keyup.enter="addItem">
    </span>
    <span class="addBtn fa fa-plus" @click="addItem"></span>
    <div class="modalArea">
      <modal v-if="showModal">
         <h3 slot="header" class="modalHeader">
          アラート
        </h3>
        <p slot="body">
          内容を入力してください。
        </p>
        <div slot="footer" class="modalFooter">
          <span class="modalCloseBtn" @click="closeModal">閉じる</span>
        </div>
      </modal>
    </div>
  </section>

</template>

<script>
import Modal from './Modal';

export default {
  name: "TodoInput",
  components: {
    Modal
  },
  data(){
    return {
      name: "TodoInput",
      newItem: '',
      flag: false,
      showModal: false
    }
  },
  methods:{
    setFlag() {
      this.flag = !this.flag;
    },
    addItem() {
      if(this.newItem !== ""){
        let item = {};
        item.content = this.newItem && this.newItem.trim();
        item.flag = this.flag;
        item.clear = false;

        this.$emit('evAddTodo', item);
        this.clearInput();

      } else {
        this.showModal = !this.showModal;
      }
    },
    clearInput() {
      this.newItem = '';
    },
    closeModal() {
      this.showModal = false;
      this.$refs.mainInput.focus();
    }
  }


}
</script>

<style scoped>

.inputArea {
  height: 50px;
  background: #fff;
  border: 1px solid #e7e7e7;
  border-radius: 5px;
  text-align: center;
  line-height: 50px;
}

.inputArea span {
  display: inline-block;
  line-height: 50px;
}

.flagBtn {
  color: #b8b8b8;
}

.flagBtn:hover {
  cursor: pointer;
}

.setFlag {
  color: #de8a1e;
}

.inputBox {
  width: 200px;
  margin-left: 20px;
}

.inputBox input {
  width: 100%;
  padding: 0 5px;
  line-height: 45px;
  border-style: none;
  font-weight: bold;
  font-size: 16px;
}

.inputBox input:focus {
  outline: none;
}

.addBtn {
  float: right;
  width: 50px;
  background: linear-gradient(to right, #6478fb, #8763fb);
  border-radius: 0 5px 5px 0;
  color: white;
  cursor: pointer;
}

.modalArea {
  text-align: left;
  line-height: 20px;
  color: #777474;
}

.modalHeader {
  margin-top: 0;
  color: #8763fb;
}

.modalFooter {
  text-align: right;
}

.modalFooter .modalCloseBtn {
  display: inline-block;
  width: 50px;
  line-height: 30px;
  text-align: center;
  color: #8763fb;
}

.modalCloseBtn:hover {
  cursor: pointer;
}

</style>