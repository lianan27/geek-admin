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
// 使用watchEffect这个函数可以让我们在数据变化之后执行指定的函数，显式地声明同步的逻辑
import { computed, ref, watchEffect } from "vue";

// let count = ref(1)
// function add() {
//   count.value ++
// }

// let { title, todos, addTodo, clear, active, all, allDone } = useTodos();

// function useTodos() {
let title = ref("")
// let todos = ref([{title:'学习Vue',done:false}])
// 写法一：使用 watchEffect，数据变化之后会把数据同步到 localStorage 之上，这样我们就实现了 todolist 和本地存储的同步。
// let todos = ref(JSON.parse(localStorage.getItem('todos') || '[]'));
// watchEffect(() => {
//   localStorage.setItem('todos', JSON.stringify(todos.value))
// })

// 写法二：更进一步，我们可以直接抽离一个 useStorage 函数，在响应式的基础之上，把任意数据响应式的变化同步到本地存储。
function useStorage(name, value=[]){
  let data = ref(JSON.parse(localStorage.getItem(name) || value))
  watchEffect(() => {
  localStorage.setItem(name, JSON.stringify(todos.value))
})
return data
}
let todos = useStorage('todos', [])

function addTodo() {
  todos.value.push({
    title: title.value,
    done: false
  });
  title.value = "";
}

function clear() {
  todos.value = todos.value.filter((v) => !v.done);
}

let active = computed(() => {
  return todos.value.filter((v) => !v.done).length;
});

let all = computed(() => todos.value.length);
let allDone = computed({
  get: function () {
    return active === 0;
  },
  set: function (value) {
    todos.value.forEach((todo) => {
      todo.done = value;
    });
  },
});

// return { title, todos, addTodo, clear, active, all, allDone };
// }
</script>

<style>
h1 {
  color: red;
}
</style>