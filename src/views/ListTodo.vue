<template>
    <div class="container">
        <div class="todo__list">
            <div v-for="(item, index) in listToShow" :key="index" class="todo">
                <TodoItem :item="item" @toggle-status="toggleStatus" @delete-item="deleteItemTodo" />
            </div>
        </div>
    </div>
</template>

<script setup>
import TodoItem from '../components/TodoItem.vue'
import { ref, defineExpose, onMounted, watchEffect } from 'vue';
const listToShow = ref([])
const getDataToDoCurrent = () => {
    const todos = localStorage.getItem('todos');
    if (todos) {
        listToShow.value = JSON.parse(todos)
        console.log(listToShow.value);
    }
}

defineExpose({
    listToShow,
})

onMounted(() => {
    getDataToDoCurrent()
})

</script>