<template>
  <div id="app">
    <MyHeader @open-input="data.isInputShow=!data.isInputShow" />
    <AddInput :is-input-show="data.isInputShow" @add-item="addItem($event)" />
    <ul class="todo-list" v-if="data.todoList.length">
      <TodoItem v-for="item, index in data.todoList" :key="index"
        :item-data="item"
        @complete-item="completeItem($event)"
        @open-check-modal="openCheckModal($event)"
        @open-edit-modal="openEditModal($event)"
        @remove-item="removeItem($event)"
      />
    </ul>
    <NoDataTip v-else />

    <CheckModal 
      :is-show-check-modal="data.isShowCheckModal" 
      :item-data="data.currentData" 
      @close-modal="data.isShowCheckModal=false" />
    <EditModal 
      :is-show-edit-modal="data.isShowEditModal" 
      :item-data="data.currentData" 
      @submit-edit="submitEdit($event)" />    
  </div>
</template>

<script setup>
import { reactive, watch } from "vue";
import MyHeader from "./components/Header/index.vue"
import AddInput from './components/AddInput/index.vue'
import TodoItem from './components/TodoItem/index.vue'
import CheckModal from './components/Modal/CheckModal/index.vue'
import EditModal from './components/Modal/EditModal/index.vue'
import NoDataTip from "./components/NoDataTip/index.vue"

const data = reactive({
  isInputShow: false,
  // todoList: [],
  todoList: JSON.parse(localStorage.getItem('todoData') || '[]'),
  isShowCheckModal: false,
  isShowEditModal: false,
  currentData: {}
})

// data.todoList = JSON.parse(localStorage.getItem('todoData') || '[]')

watch(() => data.todoList, (newVal, oldVal) => {
  localStorage.setItem('todoData', JSON.stringify(newVal)) // 监测todoList，有变化，就缓存
}, {deep: true})

const addItem = (value) => {
  const dataItem = {
    id: new Date().getTime(),
    content: value,
    completed: false
  };
  data.todoList.push(dataItem)
  data.isInputShow = false  
}

const completeItem = (id) => {
  data.todoList = data.todoList.map(item => {
    if (item.id === id) {
      item.completed = !item.completed
    }
    return item
  })  
}
const openCheckModal = (id) => {
  data.currentData = data.todoList.filter(item => item.id === id)[0]
  data.isShowCheckModal = true  
}
const openEditModal = (id) => {
  data.currentData = data.todoList.filter(item => item.id === id)[0]
  data.isShowEditModal = true
}
const removeItem = (id) => {
  data.todoList = data.todoList.filter(item => item.id !== id)
}

const submitEdit = (args) => {
  data.todoList = data.todoList.map(item => {
    if (item.id === args.id) {
      item = args.newData
    }
    return item
  })
  data.isShowEditModal = false
}
</script>

<style lang="less">
#app {
  .todo-list {
    margin-top: .44rem;
  }  
}
</style>
