<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoDashboard :list-data="todoItems" :sort-condition="sortCondition" @evSetSortCondition="setSortCondition"></TodoDashboard>
    <TodoInput @evAddTodo="addTodo"></TodoInput>
    <TodoList :list-data="todoItems" :sort-condition="sortCondition" @evRemoveTodo="removeTodo" @evEditTodo="editTodo" ></TodoList>
    <TodoFooter @evRemoveAll="removeAllTodo" v-if="sortCondition === 'A'"></TodoFooter>
  </div>
</template>
<script>
import TodoHeader from './components/TodoHeader'
import TodoDashboard from "./components/TodoDashboard";
import TodoInput from './components/TodoInput'
import TodoList from './components/TodoList'
import TodoFooter from './components/TodoFooter'

export default {
  name: 'App',
  components: {
    TodoHeader,
    TodoDashboard,
    TodoInput,
    TodoList,
    TodoFooter
  },
  data(){
    return {
      name: 'App',
      todoItems:[],
      clearTodoItems:[],
      sortCondition:'U',
    }
  },
  methods: {
    setSortCondition(flag) {
      this.sortCondition = flag;
    },
    addTodo(item) {
      let timestamp = String(Date.now());
      item.timeStamp = timestamp;
      this.sortCondition = "U";
      localStorage.setItem(timestamp, JSON.stringify(item));
      item.editDisabled = true;
      this.todoItems.push(item);
    },
    editTodo(item, idx) {
      localStorage.setItem(item.timeStamp, JSON.stringify(item));
      this.todoItems[idx] = item;
    },
    removeTodo(item, idx) {
      localStorage.removeItem(item.timeStamp);
      this.todoItems.splice(idx, 1)
    },
    removeAllTodo: function(){
      localStorage.clear();
      this.todoItems = [];
    }
  },
  created() {
    if (localStorage.length > 0) {
      for (let i = 0; i < localStorage.length; i++) {
        let key = localStorage.key(i);
        let item;
        if (key !== "loglevel:webpack-dev-server") {
          item = JSON.parse(localStorage.getItem(key));
          this.todoItems.push(item);
        }
      }
    }
  }
}
</script>

<style>

body {
  box-sizing: border-box;
  background-color: #f6f6f8;
  font-size: 16px;
  font-family: Helvetica, Arial, sans-serif;
}

</style>
