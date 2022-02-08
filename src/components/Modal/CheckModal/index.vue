<template>
  <Modal
    :is-show-modal="props.isShowCheckModal"
    modal-title="查看事件"
  >
    <p class='topic'>时间：{{data.formatedDateTime}}</p>
    <p class='topic'>内容：{{itemData.content}}</p>
    <p class='topic'>状态：{{itemData.completed ? '已完成' : '未完成'}}</p>
    <button class='btn btn-primary confirm-btn' @click="emit('close-modal')">确定</button>
  </Modal>
</template>

<script setup>
import { reactive, watch } from 'vue';
import Modal from "../index.vue"
import { formatDateTime } from "../../../libs/utils";

const props = defineProps({
  isShowCheckModal: {
    type: Boolean,
    default: false
  },
  itemData: {
    type: Object,
    default: () => ({})
  }
})

const data = reactive({
  formatedDateTime: null
})

watch(() => props.itemData, (newVal, oldVal) => {
  data.formatedDateTime = formatDateTime(newVal.id)
})

const emit = defineEmits(["close-modal"])
</script>

<style lang="less" scoped>
.topic{
  font-size: .14rem;
  margin-bottom: .1rem;
}

.confirm-btn{
  width: 100%;
  margin-top: .4rem;
}
</style>