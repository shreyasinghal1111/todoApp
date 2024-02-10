<template>
    <div class="container" style="max-width: 550px">
      <!-- Heading -->
      <h1 class="text-center mt-5" style="color:#000;">Todo List📝</h1>
  
      <!-- Input -->
      <div class="d-flex mt-5">
        <input
          type="text"
          v-model="task"
          placeholder="Enlist Tasks"
          class="w-100 form-control"
        />
        <button type="button" class="btn btn-info" @click="submitTask">SUBMIT</button>
      </div>
  
      <!-- Task -->
      <table class="table table-bordered mt-5">
        <thead>
          <tr>
            <th scope="col">Tasks ✔</th>
            <th scope="col" style="width: 120px">Status 📍</th>
            <th scope="col" class="text-center">Remove ❌</th>
            <th scope="col" class="text-center">Edit✏</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <td>
              <span :class="{ 'line-through': task.status === 'finished' }">
                {{ task.name }}
              </span>
            </td>
            <td>
              <span
                class="pointer noselect"
                @click="changeStatus(index)"
                :class="{
                  'text-danger': task.status === 'to-do',
                  'text-success': task.status === 'finished',
                  'text-warning': task.status === 'in-progress',
                }"
              >
                {{ capitalizeFirstChar(task.status) }}
              </span>
            </td>
            <td class="text-center">
              <div @click="deleteTask(index)">
                <span class="fa fa-trash pointer"></span>
              </div>
            </td>
            <td class="text-center">
              <div @click="editTask(index)">
                <p class="fa fa-pen pointer"></p>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    name: "HelloWorld",
    props: {
      msg: String,
    },
  
    data() {
      return {
        task: "",
        editedTask: null,
        statuses: ["to-do", "in-progress", "finished"],
  
        tasks: [
          {
            name: "Start a new project",
            status: "in-progress",
          },
          {
            name: "Tea Break",
            status: "finished",
          },
          {
            name: "Resume the project work",
            status: "in-progress",
          },
          {
            name: "Add modularities to project",
            status: "finished",
          },
          {
            name: "Reward - 1 chocolate :)",
            status: "to-do",
          },
        ],
      };
    },
  
    methods: {
      capitalizeFirstChar(str) {
        return str.charAt(0).toUpperCase() + str.slice(1);
      },
  
      /**
       * Change status of task by index
       */
      changeStatus(index) {
        let newIndex = this.statuses.indexOf(this.tasks[index].status);
        if (++newIndex > 2) newIndex = 0;
        this.tasks[index].status = this.statuses[newIndex];
      },
  
      /**
       * Deletes task by index
       */
      deleteTask(index) {
        this.tasks.splice(index, 1);
      },
  
      /**
       * Edit task
       */
      editTask(index) {
        this.task = this.tasks[index].name;
        this.editedTask = index;
      },
  
      /**
       * Add / Update task
       */
      submitTask() {
        if (this.task.length === 0) return;
  
        if (this.editedTask != null) {
          this.tasks[this.editedTask].name = this.task;
          this.editedTask = null;
        } else {
          
          this.tasks.push({
            name: this.task,
            status: "todo",
          });
        }
  
        this.task = "";
      },
    },
  };
  </script>
  
  <style scoped>
  .pointer {
    cursor: pointer;
  }
  .noselect {
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none; 
    -moz-user-select: none; 
    -ms-user-select: none; 
    user-select: none;
  }
  .line-through {
    text-decoration: line-through;
  }
  </style>