<template>
    <div>
        <h1 class="todo__title">To-Do List</h1>
        <div class="todo__form">
            <div class="todo__add">
                <InputText type="text" v-model="contentToDo" style="margin-right: 10px;" class="todo__add--input"
                    required @keydown.enter="addToDo" />
                <i class="pi pi-plus todo__add--button" @click="addToDo"></i>
            </div>
            <div class="todo__filter">
                <Dropdown v-model="selectStatus" :options="status" optionLabel="name" placeholder="All"
                    class="w-full md:w-14rem title__select" @change="handleFilterList" />
            </div>
        </div>
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
import TodoItem from '../components/TodoItem.vue'
import Dropdown from 'primevue/dropdown'
import InputText from 'primevue/inputtext'
import { ref, onMounted } from 'vue'
const listToDo = ref([])
const listToShow = ref([])
const idToDo = ref(0)
const contentToDo = ref('')
const selectStatus = ref({ name: 'All' })
const statusItem = ref('')
const status = ref([
    { name: 'All' },
    { name: 'Completed' },
    { name: 'Uncompleted' }
])
const addToDo = () => {
    if (!contentToDo.value.trim()) {
        alert('Please enter a valid todo item.');
        return;
    }
    else {
        const data = {
            id: idToDo.value,
            name: contentToDo.value,
            status: 'Uncompleted'
        }
        listToDo.value.push(data)
        localStorage.setItem('todos', JSON.stringify(listToDo.value));
        idToDo.value++
        contentToDo.value = ''
    }
    listToShow.value = listToDo.value
    handleFilterList()
    console.log(selectStatus.value);
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
    // getDataToDoCurrent()
    handleFilterList()
    console.log(listToDo.value);
}

const deleteItemTodo = (id) => {
    const listDeleted = listToDo.value.filter(item => item.id !== id)
    console.log(listDeleted);
    listToDo.value = listDeleted
    localStorage.setItem('todos', JSON.stringify(listToDo.value));
    handleFilterList()
}

const getDataToDoCurrent = () => {
    const todos = localStorage.getItem('todos');
    if (todos) {
        listToShow.value = JSON.parse(todos)
        console.log(listToShow.value);
    }
}
const handleFilterList = () => {
    const listToFilter = ref([])
    const todos = localStorage.getItem('todos');
    if (todos) {
        listToFilter.value = JSON.parse(todos)
    }
    if (selectStatus.value.name === 'All') getDataToDoCurrent()
    else {
        const listFilter = listToFilter.value.filter(item => item.status === selectStatus.value.name)
        console.log(listFilter);
        listToShow.value = listFilter
    }
}

onMounted(() => {
    getDataToDoCurrent()
})


</script>