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
    <li
      class="list-group-item d-flex align-item-center justify-content-between"
      v-for="item in todos"
      :key="item.id"
      @click.stop="check(item)"
    >
      <div class="form-check">
        <input
          type="checkbox"
          class="form-checkinput"
          :id="item.id"
          :checked="item.state === TodoItemState.DONE"
          :disabled="item.state === TodoItemState.DELETE"
        />
        <label
          :for="item.id"
          @click.stop.prevent="check(item)"
          :class="{
            'text-black-50 line-through': item.state == TodoItemState.Done
          }"
          >{{ item }}</label
        >
      </div>
      <div
        class="float-right ctrls"
        :class="{ 'd-none': item.state !== TodoItemState.OPEN }"
      >
        <div class="btn btn-warning btn-sm mr-2 text-light">编辑</div>
        <div class="btn btn-danger btn-sm">删除</div>
      </div>
    </li>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, reactive, ref } from 'vue'
import store from '@/store'
import { TodoItemState } from '@/common/const'
import { TodoItem } from '@/common/interface'

export default defineComponent({
  setup() {
    const inputValue = ref('')
    const add = () => {
      console.log(inputValue.value)
      store.commit('add', inputValue.value)
    }
    const check = (item: TodoItem) => {
      store.commit('check', item.id)
    }
    return reactive({
      inputValue,
      add,
      TodoItemState,
      check,
      todos: computed(() => store.state.todos)
    })
  }
})
</script>

<style lang="scss" scoped>
.line-through {
  text-decoration: line-through;
}
.list-group-item {
  user-select: none;
  &:hover {
    .ctrls {
      display: block;
    }
  }
  .ctrls {
    display: none;
  }
}
</style>
