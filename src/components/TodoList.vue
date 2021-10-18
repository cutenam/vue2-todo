<template>
  <section>
    <transition-group name="list" tag="ul" class="listArea" >
      <li v-for="(item, idx) in listData" :key="idx"  @click="editMode(item, idx)" v-show="showItem(item, idx)">
        <span class="checkBtn fa fa-check-circle" :class="{'setCheck' : item.clear}" @click="setClear(item, idx)">
        </span>
        <span class="flagBtn fa fa-flag" :class="{'setFlag' : item.flag}" @click="setFlag(item, idx)">
        </span>
        <span class="inputBox">
          <input type="text" ref="listInput" v-model="item.content" @keyup.enter="editContent(item, idx)" @blur="editContent(item, idx)"  :disabled="item.editDisabled">
        </span>
        <span class="removeBtn fa fa-trash" @click="removeItem(item, idx)">
        </span>
      </li>
    </transition-group>
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
  name: "TodoList",
  components: {
    Modal
  },
  props: ['listData', 'sortCondition'],
  data() {
    return {
      name: "TodoList",
      flag: false,
      clear: false,
      showModal: false,
      focusInputIdx: 0
    }
  },
  methods: {
    showItem: function (item, idx) {
      let show = "A";
      switch (this.sortCondition) {
        case "A":
          show = true;
          break;
        case "P":
          show = item.flag;
          break;
        case "C":
          show = item.clear;
          break;
        case "U":
          show = !item.clear;
          break;
      }
      return show;
    },
    setClear (item, idx) {
      item.clear = !item.clear;
      this.$emit('evEditTodo', item, idx);
    },
    setFlag(item, idx) {
      item.flag = !item.flag;
      this.$emit('evEditTodo', item, idx);
    },
    editMode(item, idx) {
      item.editDisabled = false;
    },
    editContent (item, idx){
      let todoContent = item.content;

      if(todoContent !== ""){
        item.content = todoContent && todoContent.trim();
        this.$emit('evEditTodo', item, idx);
        item.editDisabled = true;

      } else {
        this.showModal = !this.showModal;
        this.focusInputIdx = idx;
      }
    },
    removeItem(item, idx) {
      this.$emit('evRemoveTodo', item, idx);
    },
    closeModal() {
      this.showModal = false;
      this.$nextTick(() => {
        this.$refs.listInput[this.focusInputIdx].focus();
      });
    }
  }
}
</script>

<style scoped>

.listArea {
  list-style-type: none;
  margin: 20px auto;
  padding: 0;
}

.listArea li {
  height: 50px;
  line-height: 50px;
  margin-bottom: 5px;
  background-color: #fff;
  border-radius: 5px;
  border: 1px solid #e7e7e7;
}

.listArea span {
  display: inline-block;
  height: 50px;
  line-height: 50px;
  text-align: center;
}

.inputBox {
  width: 70%;
  margin-left: 10px;
}

.inputBox input {
  width: 100%;
  padding: 0 5px;
  line-height: 45px;
  border-style: none;
  font-weight: bold;
  color: #777474;
  font-size: 16px;
}

.inputBox input:focus {
  outline: none;
  color: #333;
}

.inputBox input:disabled {
  background-color: #fff;
  color: #777474;
}

.checkBtn {
  width: 50px;
  color: #b8b8b8;
  cursor: pointer;
}

.setCheck {
  color: #057ac9;
}

.flagBtn {
  width: 50px;
  color: #b8b8b8;
  cursor: pointer;
}

.setFlag {
  color: #de8a1e;
}

.removeBtn {
  float: right;
  width: 50px;
  color: #5BB368;
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

.list-enter-active,
.list-leave-active {
  transition: all 1s;
}

.list-enter,
.list-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

</style>