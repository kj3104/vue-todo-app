<template>
    <form action="">
        <div class="modal-card" style="width: auto">
            <header class="modal-card-head">
                <p class="modal-card-title">Edit</p>
            </header>
            <section class="modal-card-body">
                <b-field label="Task">
                    <b-input
                        v-model="taskText"
                        type="task"
                        :value="task"
                        placeholder="タスクを入力してください"
                        required>
                    </b-input>
                </b-field>
            </section>
            <footer class="modal-card-foot">
                <button class="button" type="button" v-on:click="deleteTask()">削除</button>
                <button class="button is-primary" v-on:click="editedTask()">完了</button>
            </footer>
        </div>
    </form>
</template>

<script>
export default {
  name: 'EditModal',
  props: {
    'task': String
  },
  data() {
    return {
      taskText: this.task
    }
  },
  methods: {
    editedTask() {
      this.$emit('edited', this.taskText)
    },
    deleteTask() {
      this.$emit('delete')
    }
  }
}
</script>

<style scoped>

</style>
