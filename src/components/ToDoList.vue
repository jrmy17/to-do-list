<script setup>
import { ref } from 'vue'
const task = ref('')
const editedTask = ref(null)
const availableStatus = ['A faire', 'En cours', 'Terminé']
const tasks = ref([
  {
    id: 1,
    name: 'Ma première tâche',
    status: 'A faire'
  },
  {
    id: 2,
    name: 'Ma deuxième tâche',
    status: 'A faire'
  },
  {
    id: 3,
    name: 'Ma troisième tâche',
    status: 'En cours'
  }
])
function submitTask() {
  if (task.value.length > 0) {
    if (editedTask.value === null) {
      tasks.value.push({
        id: Date.now(),
        name: task.value,
        status: 'A faire'
      })
    } else {
      tasks.value = tasks.value.map((item) => {
        if (item.id === editedTask.value) {
          return {
            ...item,
            name: task.value
          }
        }
        return item
      })
      editedTask.value = null
    }
    task.value = ''
  }
}
function deleteTask(id) {
  tasks.value = tasks.value.filter((task) => task.id !== id)
}
function editTask(id) {
  task.value = tasks.value.find((task) => task.id === id).name
  editedTask.value = id
}
function changeStatus(id) {
  tasks.value = tasks.value.map((task) => {
    if (task.id === id) {
      const index = availableStatus.indexOf(task.status)
      const nextStatus = availableStatus[index + 1]
      return {
        ...task,
        status: nextStatus ? nextStatus : availableStatus[0]
      }
    }
    return task
  })
}
</script>

<template>
  <div class="container">
    <h2 class="text-center mt-5">Ma To Do List en VueJS</h2>

    <!-- input + button -->
    <div class="d-flex gap-2">
      <input v-model="task" type="text" placeholder="Votre tâche" class="form-control" />
      <button @click="submitTask" class="btn btn-primary">
        {{ editedTask ? 'Modifier' : 'Ajouter' }}
      </button>
    </div>

    <!-- liste des tâches -->
    <table class="table table-bordered mt-4">
      <thead>
        <tr class="text-center">
          <th scope="col">Tâche</th>
          <th scope="col">Statut</th>
          <th scope="col" class="text-center">Modifier</th>
          <th scope="col" class="text-center">Supprimer</th>
        </tr>
      </thead>
      <tbody>
        <tr class="text-center align-middle" v-for="task in tasks" :key="task.id">
          <td>
            <span :class="task.status === 'Terminé' ? 'termine' : ''"> {{ task.name }}</span>
          </td>
          <td class="text-center">
            <button @click="changeStatus(task.id)" class="btn btn-light">{{ task.status }}</button>
          </td>
          <td>
            <button class="text-center btn btn-primary" @click="editTask(task.id)">
              <span class="fa fa-pen"></span>
            </button>
          </td>
          <td>
            <button class="text-center btn btn-danger" @click="deleteTask(task.id)">
              <span class="fa fa-trash"></span>
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.termine {
  text-decoration: line-through;
}
</style>
