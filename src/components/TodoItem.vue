<template>
<v-list-tile @click="toggleCheck">
  
  <v-list-tile-action @click="toggleCheck">
    <v-checkbox v-model="item.done"></v-checkbox>
  </v-list-tile-action>
  
  <v-list-tile-content>
    <v-list-tile-title>
      <s v-if="item.done">{{item.name}}</s>
      <template v-else>{{item.name}}</template>
      </v-list-tile-title>
    </v-list-tile-content>

    <v-list-tile-action>
      <v-icon @click="del()">close</v-icon>
    </v-list-tile-action>

  </v-list-tile>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator'

export class TodoFields {
  public id: number;
  public name: string;
  public done = false;

  constructor (id: number, name: string, done: boolean = false) {
    this.id = id
    this.name = name
    this.done = done
  }
}

@Component
export default class TodoItem extends Vue {
  @Prop() item!: TodoFields;
  @Prop() del!: Function;

  public toggleCheck () {
    this.item.done = !this.item.done
  }
}
</script>

<style>
</style>
