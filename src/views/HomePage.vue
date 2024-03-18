<template>
    <div>
        <h1 class="todo__title">To-Do List</h1>
        <div class="todo__form">
            <AddTodo @add-todo="addToDo"/>
            <FilterItem @filter-list="handleFilterList" ref="filter"/>
        </div>
        <!-- List -->
        <div class="container">
            <div class="todo__list">
                <div v-for="(item, index) in listToShow" :key="index" class="todo">
                    <TodoItem :item="item" @toggle-status="toggleStatus" @delete-item="deleteItemTodo" />
                </div>
            </div>
        </div>
    </div>

</template>

<script setup>
import FilterItem from '../components/FilterItem.vue'
import AddTodo from '../components/AddTodo.vue'
import TodoItem from '../components/TodoItem.vue'
import { ref, onMounted  } from 'vue'
const listToDo = ref([])
const listToShow = ref([])
const idToDo = ref(0)
const statusItem = ref('')
const filterChild = ref(null)
const selectStatusGeted = ref({ name: 'All' })
const addToDo = (content) => {
    if (!content.trim()) {
        alert('Please enter a valid todo item.');
        return;
    }
    else {
        const data = {
            id: idToDo.value,
            name: content,
            status: 'Uncompleted'
        }
        listToDo.value.push(data)
        localStorage.setItem('todos', JSON.stringify(listToDo.value));
        idToDo.value++
    }
    handleFilterList(selectStatusGeted.value.name)
    
}

const toggleStatus = (id) => {
    for (let i in listToDo.value) {
        if (listToDo.value[i].id === id) {
            if (listToDo.value[i].status === 'Uncompleted') listToDo.value[i].status = 'Completed'
            else listToDo.value[i].status = 'Uncompleted'
            console.log('true');
        }
    }
    localStorage.setItem('todos', JSON.stringify(listToDo.value));
    handleFilterList(selectStatusGeted.value.name)
    console.log(listToDo.value);
}

const deleteItemTodo = (id) => {
    const listDeleted = listToDo.value.filter(item => item.id !== id)
    console.log(listDeleted);
    listToDo.value = listDeleted
    localStorage.setItem('todos', JSON.stringify(listToDo.value));
    handleFilterList(selectStatusGeted.value.name)
}

const getDataToDoCurrent = () => {
    const todos = localStorage.getItem('todos');
    if (todos) {
        listToShow.value = JSON.parse(todos)
        listToDo.value = JSON.parse(todos)
        idToDo.value = listToDo.value[listToDo.value.length-1].id + 1
        console.log(listToShow.value);
    }
}
const handleFilterList = (category) => {
    selectStatusGeted.value.name = category
    const listToFilter = ref([])
    const todos = localStorage.getItem('todos');
    if (todos) {
        listToFilter.value = JSON.parse(todos)
    }
    if (category === 'All') getDataToDoCurrent()
    else {
        const listFilter = listToFilter.value.filter(item => item.status === category)
        console.log(listFilter);
        listToShow.value = listFilter
    }
}

onMounted(() => {
    getDataToDoCurrent()
})


</script>