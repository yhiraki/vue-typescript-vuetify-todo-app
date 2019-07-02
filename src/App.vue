<template>
<v-app>
  <v-toolbar app>
    <v-toolbar-title>
      <v-icon>check_circle</v-icon>
      <span class="font-weight-light"> Todo App</span>
    </v-toolbar-title>
    <v-spacer></v-spacer>
  </v-toolbar>
  <v-content>
    <todo-home/>
  </v-content>
</v-app>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator'
import TodoHome from './pages/TodoHome.vue'

@Component({
  components: {
    TodoHome
  }
})
export default class App extends Vue {}
</script>
