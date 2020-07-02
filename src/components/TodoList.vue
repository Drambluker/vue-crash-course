<template>
  <div>
    <ul>
      <TodoItem
        v-for="(todo, i) of todos"
        v-bind:key="todo.id"
        v-bind:index="i"
        v-bind:todo="todo"
        v-on:remove-todo="removeTodo"
      />
    </ul>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import TodoItem from "@/components/TodoItem.vue";

@Component({
  components: {
    TodoItem
  }
})
export default class TodoList extends Vue {
  @Prop() private todos!: { id: number; title: string; completed: boolean }[];

  private removeTodo(id: number): void {
    this.$emit("remove-todo", id);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
