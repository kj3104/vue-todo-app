<template>
  <div class="todo">
    <div class="columns is-desktop">
      <div class="column is-three-fifths is-offset-one-fifth flexbox-container-vertical-center">
        <b-field>
          <b-input
                v-model="taskText"
                minlength="1"
                maxlength="30"
                placeholder="タスクを入力してください"></b-input>
          <p class="control">
            <button
                class="button is-primary"
                v-on:click="addTask()">＋</button>
          </p>
        </b-field>

        <section>
          <div
              class="field task-field"
              v-for="(task, index) in tasks"
              v-bind:data="task"
              v-bind:key="task.text">
            <b-checkbox v-model="task.done">
              <span v-bind:class="{'done': task.done}">
                {{ task.message }}
              </span>
            </b-checkbox>
            <button
                class="button is-primary"
                v-on:click="editTask(task.message, index)">Edit</button>
          </div>
          <b-modal :active.sync="isModalActive">
              <edit-modal v-bind="formProps" v-on:edited="editedTask" v-on:delete="deleteTask"></edit-modal>
          </b-modal>
        </section>
      </div>
    </div>
  </div>
</template>

<script>
import EditModal from '@/components/EditModal'

export default {
  name: 'ToDo',
  components: {
    'edit-modal': EditModal
  },
  data() {
    return {
      taskText: '',
      tasks: [],
      isModalActive: false,
      editingIndex: 0,
      formProps: {
          task: ''
      }
    }
  },
  mounted() {
    this.tasks = JSON.parse(localStorage.getItem('tasks')) || []
  },
  watch: {
    tasks: {
      handler: function(){
        this.storeTasks()
      },
      deep: true,
    }
  },
  methods: {
    addTask() {
      var addData = { message : this.taskText, done: false }
      this.tasks.push(addData)
      this.taskText = ''
      this.storeTasks()
    },
    storeTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    editTask(task, index) {
      this.formProps.task = task
      this.editingIndex = index
      this.isModalActive = true
    },
    editedTask(task) {
      var editedData = { message : task, done: this.tasks[this.editingIndex].done}
      this.tasks[this.editingIndex] = editedData
      this.storeTasks()
      this.isModalActive = false
    },
    deleteTask() {
      this.tasks.splice(this.editingIndex, 1)
      this.storeTasks()
      this.isModalActive = false
    }
  }
}
</script>

<style scoped>
  .done {
  	text-decoration: line-through;
  	color: grey;
  }
  .flexbox-container-vertical-center {
    display: flex; /* 子要素をflexboxで揃える */
    flex-direction: column; /* 子要素をflexboxにより縦方向に揃える */
    justify-content: center; /* 子要素をflexboxにより中央に配置する */
    align-items: center;  /* 子要素をflexboxにより中央に配置する */
  }

  .left {
    float: right;
  }

  .task-section {
    min-width: 100%;
  }
</style>
