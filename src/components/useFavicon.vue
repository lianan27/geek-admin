<template>
  <button @click="loading">123</button>
</template>


 <script setup>
 import useFavicon from '../utils/favicon.vue'
 let {favicon}  = useFavicon()
 function loading(){
   favicon.value = '/geek.png'
 }
</script>