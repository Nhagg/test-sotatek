<template>
  <form @submit.prevent="onSubmitForm">
    <div class="form-group">
      <input type="text" class="form-control" placeholder="Add new task..." required v-model="myTask.title"/>
    </div>
    <div class="form-group">
      <label for="description">Description</label>
      <textarea type="text" class="form-control" id="description" rows="7"  v-model="myTask.description" />
    </div>
    <div class="form-group row">
      <div class="col-md-6">
        <label for="dueDate">Due Date</label>
        <DatePicker
          type="date"
          id="dueDate"
          v-model="myTask.dueDate"
          :disabled-date="disabledDate"
        />
      </div>
      <div class="col-md-6">
        <label for="piority">Piority</label>
        <select class="form-control" name="piority" id="piority" v-model="myTask.piority">
          <option>Low</option>
          <option>Normal</option>
          <option>High</option>
        </select>
      </div>
    </div>
    <button type="submit" class="mt-4 btn btn-block btn-success">
      {{ isUpdated ? 'Update' : 'Add' }}
    </button>
  </form>
</template>

<script>
import DatePicker from 'vue2-datepicker'
const DEFAULT_TASK = {
  dueDate: new Date(),
  piority: 'Normal'
}
export default {
  name: 'FormTask',
  components: { DatePicker },
  props: {
    task: {
      type: Object,
      default: Object
    },
    onSubmit: {
      type: Function,
      default: Function
    }
  },
  computed: {
    isUpdated () {
      return this.task.id
    }
  },
  data () {
    return {
      myTask: this.task.title ? { ...this.task } : { ...DEFAULT_TASK }
    }
  },
  methods: {
    disabledDate (date) {
      if (date.getTime() <= new Date().getTime()) {
        return true
      }
      return false
    },
    onSubmitForm () {
      this.onSubmit(this.myTask)
      if (this.isUpdated) {
        return
      }
      this.myTask = { ...DEFAULT_TASK }
    }
  }
}
</script>
