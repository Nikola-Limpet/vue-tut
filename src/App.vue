<script setup lang="ts">
import { computed, ref } from 'vue';
import TaskForm from './components/TaskForm.vue';
import type { Task } from './type';
import TaskList from './components/TaskList.vue';


const message = ref("Task APP!");
const tasks = ref<Task[]>([]);
const completedTasks = computed(() => {
  return tasks.value.filter((task) => task.done).length
})

function addTask(newTask: string) {
  tasks.value.push({
    id: Date.now().toString(),
    title: newTask,
    done: false
  })
}

function toggleTask(id: string) {
  const task = tasks.value.find((task) => task.id === id)
  if (task) {
    task.done = !task.done
  }
}

function removeTask(id: string) {
  tasks.value = tasks.value.filter(task => task.id !== id)
}

</script>


<template>
  <main>
    <h1 class="title"> {{ message }}</h1>
    <TaskForm @add-task ="addTask" />
    <h3 v-if="tasks.length === 0">Add a task to get started</h3>
    <h3 v-else> {{ completedTasks }} / {{ tasks.length }} tasks completed</h3>
    <section class="task-list">
      <TaskList @toggle-task="toggleTask" @remove-task="removeTask" :tasks="tasks" />
    </section>
  </main>
</template>


<style >
main {
  max-width: 800px;
  margin: 2rem auto;
  padding: 0 1rem;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
}

.title {
  color: #405e9b;
  font-size: 2.5rem;

}

.task-list {
  margin-top: 2rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  text-align: center;
  color: rgb(58, 176, 137);
}

h1 {
  font-size: 2.5rem;
  font-weight: 700;
  color: #405e9b;
  text-align: center;
  margin-bottom: 2rem;
  letter-spacing: -0.025em;
}

h3 {
  font-weight: 500;
  color: #64748b;
  margin-top: 2.5rem;
  margin-bottom: 1rem;
  font-size: 1.125rem;
}

.button-container {
  display: flex;
  justify-content: flex-end;
}

article {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.25rem 1.5rem;
  background-color: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  margin: 1rem 0;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.05), 0 1px 2px -1px rgba(0, 0, 0, 0.05);
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

article:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.08), 0 4px 6px -4px rgba(0, 0, 0, 0.05);
  border-color: #cbd5e1;
}

article p {
  margin: 0;
  font-size: 1.05rem;
  font-weight: 500;
  color: #334155;
}

input[type="checkbox"] {
  width: 1.25rem;
  height: 1.25rem;
  accent-color: #3b82f6;
  cursor: pointer;
  transition: transform 0.1s ease;
}

input[type="checkbox"]:active {
  transform: scale(0.9);
}
</style>
