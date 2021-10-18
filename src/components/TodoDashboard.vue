<template>
  <section class="dashboard">
    <div class="dashboardItem" :class="{'selectA':sortCondition === 'A'}" @click="setSortCondition('A')">
        <span class="fa fa-list-alt fa-2x" :class="{'iconA':sortCondition !== 'A'}"></span>
        <span class="title">すべて({{countAllItem}})</span>
    </div>
    <div class="dashboardItem" :class="{'selectP':sortCondition === 'P'}" @click="setSortCondition('P')">
        <span class="far fa-flag fa-2x" :class="{'iconP':sortCondition !== 'P'}"></span>
        <span class="title">フラグ付き({{countPriorityItem}})</span>
    </div>
    <div class="dashboardItem" :class="{'selectC':sortCondition === 'C'}" @click="setSortCondition('C')">
        <span class="fa fa-check-circle fa-2x" :class="{'iconC':sortCondition !== 'C'}"></span>
        <span class="title">完了({{countClearItem}})</span>
    </div>
    <div class="dashboardItem" :class="{'selectU':sortCondition === 'U'}" @click="setSortCondition('U')">
        <span class="fa fa-check-circle fa-2x" :class="{'iconU':sortCondition !== 'U'}"></span>
        <span class="title">未完了({{countUnclearItem}})</span>
    </div>
  </section>
</template>

<script>
export default {
  name: "TodoDashboard",
  props: ['listData', 'sortCondition'],
  data(){
    return {
      name: 'TodoDashboard'
    }
  },
  computed: {
    countAllItem : function(){
      return this.listData.length;
    },
    countPriorityItem: function(){
      let count = 0;
      count = (this.listData).filter(s => s.flag).length;
      return count;
    },
    countClearItem: function(){
      let count = 0;
      count = (this.listData).filter(s => s.clear).length;
      return count;
    },
    countUnclearItem: function(){
      let count = 0;
      count = (this.listData).filter(s => !s.clear).length;
      return count;
    }
  },
  methods: {
    setSortCondition (flag) {
      this.$emit('evSetSortCondition', flag);
    },
  }
}
</script>

<style scoped>

.dashboard {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  margin: 20px auto;
}

.dashboardItem {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  width: 145px;
  height: 50px;
  border-radius: 5px;
  background-color: #e4e4e4;
  color: #777;
}

.dashboardItem span {
  display: inline-block;
  text-align: center;
}

.dashboardItem span:hover {
  cursor: pointer;
}

.dashboardItem .title {
  width: 70%;
}

.iconA {
  color: #8763fb;
}
.iconP {
  color: #de8a1e;
}
.iconC {
  color: #057ac9;
}
.iconU {
  color: #8b8b8b;
}

.selectA {
  background: linear-gradient(to right, #6478fb, #8763fb);
  color: #fff;
}

.selectP {
  background: linear-gradient(to right, #f6aa44, #de8a1e);
  color: #fff;
}

.selectU {
  background-color: #9b9a9a;
  color: #fff;
}

.selectC {
  background: linear-gradient(to right, #62acde, #057ac9);
  color: #fff;
}
</style>