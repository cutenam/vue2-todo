<template>
  <div class="removeAllBtn" @click="removeAll">
    <span>すべて削除</span>
  </div>
</template>

<script>
export default {
  name: "TodoFooter",
  methods: {
    removeAll () {
      this.$emit('evRemoveAll');
    }
  }
}
</script>

<style scoped>
.removeAllBtn {
  margin: 0 auto;
  line-height: 50px;
  text-align: center;
}

.removeAllBtn span {
  width: 100px;
  height: 50px;
  display: inline-block;
  border: 1px solid #dc6056;
  border-radius: 5px;
  background-color: #fff;
  transition-duration: 0.4s;
  cursor: pointer;
  color: #dc6056;
}

.removeAllBtn :hover {
  background-color: #dc6056;
  color: #fff;
}

</style>