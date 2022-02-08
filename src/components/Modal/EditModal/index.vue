<template>
  <Modal
    :is-show-modal="props.isShowEditModal"
    modal-title="编辑事件"
  >
    <p class='topic'>时间：{{data.formatedDateTime}}</p>      
    <p class='topic'>
      <textarea class='text-area' v-model="data.inputValue" :defaultValue="props.itemData.content" />
    </p>

    <p class='topic'>状态：
      <input v-model="data.checkValue" type="checkbox" :defaultChecked="props.itemData.completed ? true : false" />
    </p>
    <button class='btn btn-primary comfirm-btn' @click="formatNewData">提交</button>
  </Modal>
</template>

<script setup>
import { reactive, watch } from 'vue';
import Modal from "../index.vue"
import { formatDateTime } from "../../../libs/utils";

const props = defineProps({
  isShowEditModal: {
    type: Boolean,
    default: false
  },
  itemData: {
    type: Object,
    default: () => ({})
  }
})

const data = reactive({
  inputValue: '',
  checkValue: '',
  formatedDateTime: null
})

watch(() => props.itemData, (newVal, oldVal) => {
  data.inputValue = newVal.content
  data.checkValue = newVal.completed
  data.formatedDateTime = formatDateTime(newVal.id)
}, {deep: true})

const emit = defineEmits(["submit-edit"])

const formatNewData = () => {
  if (data.inputValue.trim() === 0) {
    return;
  }
  const newData = {
    id: new Date().getTime(),
    content: data.inputValue.trim(),
    completed: data.checkValue
  }
  emit("submit-edit", {newData, id: props.itemData.id})
  // emit("submit-edit", {nData: newData, id: props.itemData.id})
}
</script>

<style lang="less" scoped>
.topic{
  font-size: .14rem;
  margin-bottom: .1rem;
}

.comfirm-btn{
  width: 100%;
  margin-top: .4rem;
}

.text-area{
  width: 100%;
  min-height: 1rem;
  border: 1px solid #ddd;
  padding: .05rem;
}

</style>