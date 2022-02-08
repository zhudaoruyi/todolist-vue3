<template>
  <div class="input-wrapper" v-if="props.isInputShow">
    <input type="text" ref="inputRef" v-model="data.inputValue" placeholder="请输入待办事件" />
    <button class="btn btn-primary" @click="submitValue">增加</button>
  </div>  
</template>

<script setup>
import { nextTick, reactive, ref, watch } from 'vue'

const props = defineProps({
  isInputShow: {
    type: Boolean,
    default: false
  }
})

const data = reactive({
  inputValue: ''
})

const inputRef = ref(null)

watch(() => props.isInputShow, (newVal, oldVal) => {
  if (newVal) {
    nextTick(() => {
      inputRef.value.focus()
    })
  }
})

const emit = defineEmits(["add-item"])

const submitValue = () => {
  data.inputValue = data.inputValue.trim()
  if (data.inputValue.length === 0) {
    return;
  }
  emit("add-item", data.inputValue)
  data.inputValue = ''
}

</script>

<style lang="less" scoped>
.input-wrapper {
  display: flex;
  flex-direction: row;
  position: fixed;
  top: .44rem;
  left: 0;
  width: 100%;
  height: .38rem;
  padding: .04rem .1rem;
  box-sizing: border-box;
  border-bottom: 1px solid #ddd;
  background-color: #fff;

  input {
    flex: 1;
    display: block;
    height: .3rem;
    border: 1px solid #ddd;
    font-size: .14rem;
    text-indent: .1rem;
  }
  button {
    margin-left: .1rem;
  }
}
</style>