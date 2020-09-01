<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-5">
        <h3 class="text-center">New Task</h3>
        <div class="card-body">
          <FormTask :on-submit="addNewTask" />
        </div>
      </div>
      <div class="col-md-7">
        <h3 class="text-center">Todo List</h3>
        <div class="card-body">
          <input type="text" class="form-control" placeholder="Search..." v-model="searchText"/>
          <div class="task-list">
            <div
              v-for="(task, index) in listTask.filter(t => !searchText || t.title.indexOf(searchText) > -1)"
              :key="index"
              class="task-item"
            >
              <div class="task-header">
                <div class="header-title">
                  <input type="checkbox" @change="checkTask(task)" :id="'checkTask' + index" :disabled="task.status"/>
                  <label :for="'checkTask' + index">
                    {{ task.title }}
                    {{ task.status ? '(done)' : '' }}
                  </label>
                </div>
                <div class="header-action">
                  <b-button class="btn btn-primary" v-b-toggle="'collapse-' + index" variant="primary">
                    Detail
                  </b-button>
                  <button class="btn btn-danger ml-3" @click="removeTask(task)">Remove</button>
                </div>
              </div>
              <b-collapse :id="'collapse-' + index">
                <div class="task-body">
                  <FormTask :task="{...task}" :on-submit="(newData) => updateTask(newData, index)" />
                </div>
              </b-collapse>
            </div>
          </div>
          <div v-if="checkedTask.length" class="bulk-action">
            <span>Bulk Action:</span>
            <div>
              <button class="btn btn-success" @click="doneCheckedTasks">
                Done
              </button>
              <button class="btn btn-danger" @click="removeCheckedTasks">
                Remove
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import FormTask from '@/components/FormTask'
export default {
  name: 'Main',
  components: { FormTask },
  watch: {
    searchText () {
      // blank checkedTask when searching
      this.checkedTask = []
    }
  },
  data () {
    return {
      taskId: 1,
      searchText: '',
      listTask: [],
      checkedTask: []
    }
  },
  methods: {
    addNewTask (task) {
      console.log('addNewTask', task)
      this.listTask.push({ ...task, id: this.taskId })
      this.taskId++
    },
    updateTask (newData, index) {
      this.listTask.splice(index, 1, newData)
    },
    removeTask (task) {
      let taskIndex = this.listTask.findIndex(t => t === task.id)
      this.listTask.splice(taskIndex, 1)
    },
    checkTask (task) {
      let taskIndex = this.checkedTask.findIndex(t => t === task.id)
      if (taskIndex === -1) {
        this.checkedTask.push(task.id)
      } else {
        this.checkedTask.splice(taskIndex, 1)
      }
      console.log(this.checkedTask)
    },
    doneCheckedTasks () {
      this.checkedTask.forEach(taskId => {
        const task = this.listTask.find(t => t.id === taskId)
        task.status = 'done'
      })
      this.checkedTask = []
    },
    removeCheckedTasks () {
      this.checkedTask.forEach(taskId => {
        const taskIndex = this.listTask.findIndex(t => t.id === taskId)
        this.listTask.splice(taskIndex, 1)
      })
    }
  }
}
</script>
