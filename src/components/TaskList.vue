<script setup lang="ts">
import { ref, computed } from 'vue';
import type { Task } from '../type';

const props = defineProps<{
  tasks: Task[]
}>()

const emits = defineEmits<{
  toggleTask: [id: string],
  removeTask: [id: string]
}>()

const currentFilter = ref<'all' | 'active' | 'completed'>('all')

const filteredTasks = computed(() => {
  if (currentFilter.value === 'active') {
    return props.tasks.filter(t => !t.done)
  }
  if (currentFilter.value === 'completed') {
    return props.tasks.filter(t => t.done)
  }
  return props.tasks
})

function toggleTask(id: string) {
  emits("toggleTask", id)
}

function removeTask(id: string) {
  emits("removeTask", id)
}
</script>

<template>
  <div class="filter-bar">
    <button :class="{ active: currentFilter === 'all' }" @click="currentFilter = 'all'">All</button>
    <button :class="{ active: currentFilter === 'active' }" @click="currentFilter = 'active'">Active</button>
    <button :class="{ active: currentFilter === 'completed' }" @click="currentFilter = 'completed'">Completed</button>
  </div>
  <TransitionGroup name="task">
  <article v-for="task in filteredTasks" :key="task.id">
    <span :class="{ done: task.done }">
      <p> {{ task.title }} </p>
    </span>
    <div class="actions">
      <input 
        type="checkbox" 
        :checked="task.done" 
        @change="toggleTask(task.id)" 
      />
      <button class="remove-btn" @click="removeTask(task.id)">Remove</button>
    </div>
  </article>
  </TransitionGroup>
</template>

<style scoped>
.done {
  text-decoration: line-through;
  color: #94a3b8;
}

.filter-bar {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-bottom: 1rem;
}

.filter-bar button {
  padding: 0.5rem 1rem;
  border: 1px solid #e2e8f0;
  background: white;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s;
  color: #64748b;
  font-weight: 500;
}

.filter-bar button:hover {
  background: #f8fafc;
}

.filter-bar button.active {
  background: #3b82f6;
  color: white;
  border-color: #3b82f6;
}

.actions {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.remove-btn {
  padding: 0.4rem 0.8rem;
  background-color: #ef4444;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.9rem;
  transition: background-color 0.2s;
}

.remove-btn:hover {
  background-color: #dc2626;
}

.task-enter-active,
.task-leave-active {
  transition: all 0.5s ease;
}

.task-enter-from,
.task-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
