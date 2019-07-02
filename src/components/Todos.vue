<template>
  <v-card v-if="!todos.empty">
    <v-list subheader>

      <v-subheader v-if="todos.list.length">Todos</v-subheader>
      <todo-item
        v-for="item in todos.list"
        :key="item.id"
        :item="item"
        :del="()=>(todos.del(item.id))"
        />

      <v-subheader v-if="todos.doneList.length">Dones</v-subheader>
      <todo-item
        v-for="item in todos.doneList"
        :key="item.id"
        :item="item"
        :del="()=>(todos.del(item.id))"
        />

    </v-list>
  </v-card>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator'
import TodoItem, { TodoFields } from './TodoItem.vue'

export class TodoList {
  private todos: TodoFields[];
  private nextIdx = 0;

  constructor (todos: TodoFields[] = []) {
    this.todos = todos
    for (const t of todos) {
      this.nextIdx = Math.max(this.nextIdx, t.id + 1)
    }
  }

  get doneList () {
    return this.todos.filter(i => i.done)
  }

  get list () {
    return this.todos.filter(i => !(i.done))
  }

  get empty () {
    return this.todos.length === 0
  }

  add (text: string) {
    this.todos.push(new TodoFields(this.nextIdx, text))
    ++this.nextIdx
  }

  del (id: number) {
    this.todos = this.todos.filter(i => i.id !== id)
  }

  toJSON () {
    const todos = []
    for (const i of this.todos) {
      todos.push({
        id: i.id,
        name: i.name,
        done: i.done
      })
    }
    return { todos: todos }
  }

  static fromJSON (json: string) {
    const t = JSON.parse(json)
    const todos = []
    for (const i of t.todos) {
      todos.push(new TodoFields(i.id, i.name, i.done))
    }
    return new TodoList(todos)
  }
}

@Component({
  components: {
    TodoItem
  }
})
export default class Todos extends Vue {
  @Prop() todos!: TodoList
}
</script>

<style>
</style>
