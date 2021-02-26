<template>
  <div class="form-group">
    <label for="">添加待办事项</label>
    <div class="row">
      <div class="col-8">
        <input
          type="text"
          class="form-control"
          v-model="inputValue"
          @keydown.enter="add()"
        />
      </div>
      <div class="col-4">
        <select name="" id="" class="form-control">
          <option value="">选择以过滤。。。</option>
          <option value="">待办中</option>
          <option value="">已完成</option>
          <option value="">已删除</option>
        </select>
      </div>
    </div>
    <small class="form-text text-muted">回车即可加入</small>
  </div>
  <div class="list-group">
    <li class="list-group-item" v-for="item in todos" :key="item.id">
      {{ item }}
    </li>
  </div>
</template>

<script>
import { computed, defineComponent, reactive, ref } from 'vue'
import store from '@/store'

export default defineComponent({
  setup() {
    const inputValue = ref('')
    const add = () => {
      console.log(inputValue.value)
      store.commit('add', inputValue.value)
    }
    return reactive({
      inputValue,
      add,
      todos: computed(() => store.state.todos)
    })
  }
})
</script>

<style></style>
