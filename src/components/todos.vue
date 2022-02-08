/* eslint-disable indent */
<template>
  <div class="view">
    <!-- 完成复选框 -->
    <input type="checkbox" v-model="todo.completed" class="toggle"/>
    <!-- 待办事项 -->
    <input v-model="todo.title" :class="todo.completed ? 'todo-title-completed' : 'todo-title'">
    <!-- 删除按钮 -->
    <button @click="$emit('removeTodo')" class="destory">×</button>
  </div>
</template>

<script>
export default {
  name: 'todos',
  props: ['todo'],
  data () {
    return {
    }
  }
}
</script>
