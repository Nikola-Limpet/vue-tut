<script setup lang="ts">
import { ref } from 'vue';


const message = ref("Task APP!");
const newTask = ref("")

</script>


<template>
  <main>
    <h1> {{ message }}</h1>
    <form>
      <label >New Task</label>
      <input name="newTask" v-model="newTask" placeholder="Enter a task" />
      <button type="submit">Add Task</button>
    </form>
    <p> {{ newTask }}</p>
  </main>
</template>


<style scoped>
main {
  max-width: 800px;
  margin: 1rem auto;

}
</style>
