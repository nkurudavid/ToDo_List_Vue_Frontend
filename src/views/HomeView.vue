<template>
  <div class="container">
    
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><RouterLink to="/">Home</RouterLink></li>
        <li class="breadcrumb-item active" aria-current="page">Vuengo ToDo App</li>
      </ol>
    </nav>

    <div class="row mt-2 mb-2">
      <div class="col-md-4 col-sm-12 mb-3">
        <div class="form-container bg-light p-4">
          <h4 class="text-success">Add New Task</h4>
          <hr>
          <form action="" @submit.prevent="addTask">
            <div class="mb-3">
              <label for="title" class="form-label">Task Title</label>
              <input type="text" class="form-control" name="title" id="title" placeholder="Task title" v-model="title" />
            </div>
            <div class="mb-3">
              <label for="description" class="form-label">Task Description</label>
              <textarea class="form-control" name="description" id="description" rows="3" v-model="description"></textarea>
            </div>
            <div class="mb-3">
              <label for="status" class="form-label">Task Title</label>
              <select class="form-select" aria-label="Select Status" id="status" name="status" v-model="status">
                <option selected>Select Status</option>
                <option value="todo">ToDo</option>
                <option value="done">Done</option>
              </select>
            </div>
            <div class="mb-3">
              <button class="btn btn-outline-primary" type="submit">Submit</button>
            </div>
          </form>
        </div>
      </div>

      <div class="col-md-8 col-sm-12 p-4">
        <h4 class="text-success">Tasks</h4>
        <hr>
        <div class="row">
          <div class="col-12 mb-3">
            <h5 class="text-success">ToDo List</h5>
            <div class="card mb-2" v-for="task in todoTasks" :key="task.id">
              <div class="card-body">
                <h5 class="card-title">{{ task.title }}</h5>
                <p class="card-text">{{ task.description }}.</p>
                <span>Date: {{ formatDate(task.created_date) }}</span>
              </div>
              <div class="card-footer">
                Click '<a href="" class="card-footer-item" @click.prevent="setStatus(task.id, 'done')">Done</a>' to change status
              </div>
            </div>
          </div>

          <div class="col-12 mb-3">
            <h5 class="text-success">Done List</h5>
            <div class="card mb-2" v-for="task in doneTasks" :key="task.id">
              <div class="card-body">
                <h5 class="card-title">{{ task.title }}</h5>
                <p class="card-text">{{ task.description }}.</p>
                <span>Date: {{ formatDate(task.created_date) }}</span>
              </div>
              <div class="card-footer">
                Click '<a href="" class="card-footer-item" @click.prevent="setStatus(task.id, 'todo')">To Do</a>' to change status
              </div>
            </div>
          </div>

        </div>
      </div>

    </div>

  </div>
  
</template>


<script>
  import axios from 'axios'

  export default {
    name: 'Home',
    data() {
      return {
        tasks: [],

        title: '',
        description: '',
        status: 'todo',
      }
    },
    computed: {
      todoTasks() {
        return this.tasks.filter(task => task.status === 'todo');
      },
      doneTasks() {
        return this.tasks.filter(task => task.status === 'done');
      }
    },
    mounted() {
      this.getTasks()
    },
    methods: {
      formatDate(dateString) {
        const date = new Date(dateString);
        const options = { year: 'numeric', month: 'long', day: 'numeric', hour: 'numeric', minute: 'numeric'};
        return date.toLocaleDateString('en-US', options);
      },
      getTasks() {
        axios({
          method: 'get',
          url: 'http://127.0.0.1:8000/tasks/',
          auth: {
            username: 'admin',
            password: 'admin',
          }
        }).then(response => this.tasks = response.data);
      },
      addTask() {
        if(this.title || this.description) {
          axios({
            method: 'post',
            url: 'http://127.0.0.1:8000/tasks/',
            data: {
              title: this.title,
              description: this.description,
              status: this.status,
            },
            auth: {
              username: 'admin',
              password: 'admin',
            }
          }).then(response => {
            let newTask = {
              'id': response.data.id,
              'title': this.title,
              'description': this.description,
              'status': this.status,
            }
            this.tasks.push(newTask);

            this.title = '';
            this.description = '';
            this.status = 'todo';
          }).catch((error) => {
            console.log(error);
          })
        }else{
          alert("All fields are required! Please fill the form correctly.");
        }
      },
      setStatus(task_id, status) {
        const task = this.tasks.filter(task => task.id === task_id)[0];
        const title = task.title
        const description = task.description

        axios ({
          method: 'put',
          url: 'http://127.0.0.1:8000/tasks/' + task_id + '/',
          headers: {
            'Content-Type': 'application/Json',
          },
          data: {
            title: title,
            description: description,
            status: status,
          },
          auth: {
            username: 'admin',
            password: 'admin',
          }
        }).then(()=> {
          task.status = status
        })
      }
    }
  }

</script>
