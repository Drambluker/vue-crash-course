<template>
  <div>
    <h1>Todo application</h1>
    <AddTodo @add-todo="addTodo" />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not Completed</option>
    </select>
    <hr />
    <Loader v-if="loading" />
    <TodoList
      v-else-if="filteredTodos.length"
      v-bind:todos="filteredTodos"
      @remove-todo="removeTodo"
    />
    <p v-else>No todos</p>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import AddTodo from "@/components/AddTodo.vue";
import TodoList from "@/components/TodoList.vue";
import Loader from "@/components/Loader.vue";

@Component({
  components: {
    AddTodo,
    TodoList,
    Loader
  },
  watch: {
    filter(value: string, oldValue: string): void {
      console.log("value:", value, "; oldValue:", oldValue);
    }
  }
})
export default class App extends Vue {
  @Prop() private todos: {
    id: number;
    title: string;
    completed: boolean;
  }[] = [];
  @Prop() private loading = true;
  @Prop() private filter = "all";

  data() {
    return {
      todos: this.todos,
      loading: this.loading,
      filter: this.filter
    };
  }

  mounted(): void {
    fetch("https://jsonplaceholder.typicode.com/todos?_limit=42")
      .then(response => response.json())
      .then(json => {
        setTimeout(() => {
          this.todos = json;
          this.loading = false;
        }, 1000);
      });
  }

  get filteredTodos(): { id: number; title: string; completed: boolean }[] {
    if (this.filter === "all") {
      return this.todos;
    } else if (this.filter === "completed") {
      return this.todos.filter(
        (t: { id: number; title: string; completed: boolean }) => t.completed
      );
    } else if (this.filter === "not-completed") {
      return this.todos.filter(
        (t: { id: number; title: string; completed: boolean }) => !t.completed
      );
    }

    return [];
  }

  private addTodo(todo: {
    id: number;
    title: string;
    completed: boolean;
  }): void {
    this.todos.push(todo);
  }

  private removeTodo(id: number): void {
    this.todos = this.todos.filter(
      (t: { id: number; title: string; completed: boolean }) => t.id !== id
    );
  }
}
</script>

<style lang="scss"></style>
