<template>
  <v-container>
    <v-layout>
      <v-text-field
        box outline
        v-model="newTodoText"
        label="Write a Todo."
        :append-icon="addEnabled ? 'add':''"
        @click:append="add()"
        @keyup.enter="add()"
      ></v-text-field>
    </v-layout>
    <todos :todos="todos"></todos>
  </v-container>
</template>

<script lang="ts">
import { Vue, Component, Watch } from 'vue-property-decorator'
import Todos, { TodoList } from '../components/Todos.vue'

const storage = localStorage

@Component({
  components: { Todos }
})
export default class TodoHome extends Vue {
  todos: TodoList = new TodoList();
  newTodoText: string = '';

  get addEnabled (): boolean {
    return this.newTodoText !== ''
  }

  add () {
    if (this.addEnabled) {
      this.todos.add(this.newTodoText)
      this.newTodoText = ''
    }
  }

  created () {
    const cache = storage.getItem('todos')
    if (cache) this.todos = TodoList.fromJSON(cache)
  }

  @Watch('todos.todos', { deep: true })
  saveTodos () {
    storage.setItem('todos', JSON.stringify(this.todos))
  }
}
</script>
