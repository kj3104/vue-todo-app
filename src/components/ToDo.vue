<template>
  <div class="todo">
    <div class="columns is-desktop">
      <div class="column is-three-fifths is-offset-one-fifth">
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
              class="field"
              v-for="task in tasks"
              v-bind:data="task"
              v-bind:key="task.text">
            <b-checkbox v-model="task.done">
              <span v-bind:class="{'done': task.done}">
                {{ task.message }}
              </span>
            </b-checkbox>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ToDo',
  data() {
    return {
      taskText: '',
      tasks: [],
      done: false
    }
  },
  methods: {
    addTask() {
      var addData = { message : this.taskText, done: false }
      this.tasks.push(addData)
      this.taskText = ''
    }
  }
}
</script>

<style scoped>
  .done {
  	text-decoration: line-through;
  	color: grey;
  }
</style>
