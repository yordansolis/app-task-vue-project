<template>
    <div class="container mt-5">
      <h2 class="text-center mt-5">App con Vue</h2>
  
      <div class="d-flex justify-content-center mt-5">
        <input 
          v-model="task"
          type="text" 
          class="form-control me-2" 
          placeholder="Escribe una tarea">
        <button @click="submitAddTask" class="btn btn-warning">Agregar</button>
      </div>
  
      <div class="table-responsive mt-4">
        <table class="table table-bordered table-striped">
          <thead>
            <tr>
              <th scope="col">Tarea</th>
              <th scope="col">Estado</th>
              <th scope="col">Estado (badge)</th>
              <th scope="col">Editar</th>
              <th scope="col">Eliminar</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(task, index) in tasks" :key="index">
              <td>{{ task.name }}</td>
              <td>
                <span  @click="changeStatus(index)" class="pointer text-primary btn-status-task ">
                  {{ task.status }}
                </span>
              </td>
              <td>
                <span v-if="task.status === 'completada'" class="badge bg-success">Completada</span>
                <span v-else class="badge bg-danger">Incompleta</span>
              </td>
              <td @click="editTask(index)" class="text-center btn-hover-edit btn-cursor">
                <i class="bi bi-pen-fill text-info"></i>
              </td>
              <td @click="deleteTask(index)" class="text-center btn-hover-delete btn-cursor">
                <i class="bi bi-trash-fill text-danger"></i>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "TodoList",
    props: {
      msg: String
    },
    data() {
      return {
        task: '',
        taskToEditIndex: null,
        availableStatus: ['completada', 'incompletada'],
        tasks: [
          { name: 'Agregar plátanos en la tienda', status: 'completada' },
          { name: 'Hacer la compra', status: 'incompletada' },
          { name: 'Limpiar la casa', status: 'incompletada' },
          { name: 'Hacer la comida', status: 'incompletada' },
        ],
      };
    },
    methods: {
      submitAddTask() {
        if (this.task.trim().length === 0) {
          alert('Escribe una tarea');
          return;
        }
  
        if (this.taskToEditIndex === null) {
          this.tasks.push({ name: this.task, status: 'incompletada' });
        } else {
          this.tasks[this.taskToEditIndex].name = this.task;
          this.taskToEditIndex = null;
        }
  
        this.task = '';
      },
  
      deleteTask(index) {
        if (confirm('¿Estás seguro de que deseas eliminar esta tarea?')) {
          this.tasks.splice(index, 1);
        }
      },
  
      editTask(index) {
        this.task = this.tasks[index].name;
        this.taskToEditIndex = index;
      },
  
      changeStatus(index) {
        const currentStatus = this.tasks[index].status;
        const currentIndex = this.availableStatus.indexOf(currentStatus);
        const nextIndex = (currentIndex + 1) % this.availableStatus.length;
        this.tasks[index].status = this.availableStatus[nextIndex];
      }
    }
  };
  </script>

