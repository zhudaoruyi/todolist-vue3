<template>
  <div>
    <li class="todo-item">
      <div class="check-box">
        <input type="checkbox" :checked="data.itemData.completed" @change="emit('complete-item', data.itemData.id)" />
      </div>
      <span class="content" :style="{'textDecoration': data.itemData.completed ? 'line-through' : 'none'}">{{data.itemData.content}}</span>
      <div class="btn-group">
        <button class="btn btn-primary" @click="emit('open-check-modal', data.itemData.id)">查看</button>
        <button class="btn btn-warning" @click="emit('open-edit-modal', data.itemData.id)">编辑</button>
        <button class="btn btn-danger" @click="emit('remove-item', data.itemData.id)">删除</button>
      </div>
    </li>    
  </div>
</template>

<script setup>
const data = defineProps({
  itemData: {
    type: Object,
    default: () => ({})
  }
})

const emit = defineEmits(["complete-item", "open-check-modal", "open-edit-modal", "remove-item"])
</script>

<style lang="less" scoped>
.todo-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  height: .7rem;
  border-bottom: 1px solid #ddd;

  .check-box {
    display: flex;
    justify-content: center;
    align-items: center;
    width: .5rem;
    height: .5rem;
  }

  .content {
    flex: 1;
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    min-width:0;
    font-size: .16rem;
  }

  .btn-group {
    display: flex;
    flex-direction: row;
    width: 1.7rem;

    .btn {
      margin-left: .1rem;
      font-size: .12rem;
    }
  }
}
</style>