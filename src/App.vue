<template>
  <div>
    <h1 class="todo__title">To-Do List</h1>
    <div class="todo__form">
      <div class="todo__add">
        <InputText type="text" v-model="contentToDo" style="margin-right: 10px;" class="todo__add--input" required />
        <i class="pi pi-plus todo__add--button" @click="addToDo"></i>
      </div>
      <div class="todo__filter">
        <Dropdown v-model="selectStatus" :options="status" optionLabel="name" placeholder="All" class="w-full md:w-14rem title__select" @change="handleFilterList" />
      </div>
    </div>
    <div class="container">
      <div class="todo__list">
        <div v-for="(item, index) in listToDo" :key="index" class="todo">
          <div class="item__content" v-if="item" :class="{ 'line-though': item.status === 'Completed' }">
            {{ item.name }}
          </div>
          <div class="item__option" v-if="item">
            <i class="pi pi-check" @click="toggleStatus(item.id)"></i>
            <i class="pi pi-trash" @click="deleteItemTodo(item.id)"></i>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script setup>
import Dropdown from 'primevue/dropdown'
import InputText from 'primevue/inputtext'
import { ref, onMounted } from 'vue'
const listToDo = ref([])
const idToDo = ref(0)
const contentToDo = ref('')
const selectStatus = ref('')
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
}

const toggleStatus = (id) => {
  for (let i in listToDo.value) {
    if (listToDo.value[i].id === id) {
      if (listToDo.value[i].status === 'Completed') {
        listToDo.value[i].status = 'Uncompleted'
      }
      else listToDo.value[i].status = 'Completed'
    }
  }
  localStorage.setItem('todos', JSON.stringify(listToDo.value));
  handleFilterList()
  console.log(listToDo.value);
}

const deleteItemTodo = (id) => {
  const listDeleted = listToDo.value.filter(item => item.id !== id)
  console.log(listDeleted);
  listToDo.value = listDeleted
  localStorage.setItem('todos', JSON.stringify(listToDo.value));
}

const getDataToDoCurrent = () => {
  const todos = localStorage.getItem('todos');
  if (todos) {
    listToDo.value = JSON.parse(todos)
    console.log(listToDo.value);
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
    listToDo.value = listFilter

  }

}

onMounted(() => {
  getDataToDoCurrent()
})


</script>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.line-though {
  text-decoration: line-through;
  color: gray;
}
</style>
