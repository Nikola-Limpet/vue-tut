<script setup lang="ts">
import { ref } from 'vue';


const emit= defineEmits<{
  addTask: [newTask : string]
}>()

const newTask = ref("");
const error = ref("");

function formSubmitted() {
  if (newTask.value && newTask.value.trim() !== "") {
    emit("addTask", newTask.value)
    newTask.value = ""
  } else {
    error.value = "Please enter a task"
  } 
}
</script>


<template>
  <main>
    <form @submit.prevent="formSubmitted">
      <label>New Task</label>
      <input 
        name="newTask"  
        v-model="newTask" 
        :aria-invalid="error ? true : undefined" 
        @input="error = ''"
        placeholder="Enter a task" 
      />
      <small v-if="error" > {{ error }} </small>
      <div class="button-continer">
        <button type="submit">Add Task</button>
      </div>
    </form>
  </main>
</template>


<style scoped>
main {
    max-width: 800px;
    margin: 1rem auto;

}
</style>