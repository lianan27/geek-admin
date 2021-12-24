<template>
<div>
  <!-- <h1 @click="add">{{count}}</h1> -->
  <input type="text" v-model="title" @keydown.enter="addTodo" />
  <button v-if="active < all" @click="clear">清理</button>
  <ul v-if="todos.length">
    <li v-for="todo in todos">
      <input type="checkbox" v-model="todo.done" />
      <span :class="{ done: todo.done }">{{ todo.title}}</span>
    </li>
  </ul>
  <div v-else>暂无数据</div>
  <div>
    全选<input type="checkout" v-model="allDone" />
    <span> {{ active }} / {{ all }}</span>
  </div>
</div>

</template>

<script setup>
import { ref } from "vue";
let title = ref("")
let todos = ref([{title:'学习Vue',done:false}])
// let count = ref(1)
// function add() {
//   count.value ++
// }

function addTodo() {
  todos.value.push({
    title: title.value,
    done: false
  });
  title.value = " "
}
</script>

<style>
h1 {
  color: red;
}
</style>