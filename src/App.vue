<template>
  <div id="app">
    <img alt="Vue logo" src="@/assets/logo.png" />
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> |
      <router-link to="/todos">Todos</router-link>
    </div>
    <router-view />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import AddTodo from "@/components/AddTodo.vue";
import TodoList from "@/components/TodoList.vue";

@Component({
  components: {
    AddTodo,
    TodoList
  }
})
export default class App extends Vue {
  @Prop() private todos!: { id: number; title: string; completed: boolean }[];

  data() {
    return {
      todos: this.todos
    };
  }

  mounted(): void {
    fetch("https://jsonplaceholder.typicode.com/todos?_limit=42")
      .then(response => response.json())
      .then(json => (this.todos = json));
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

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
