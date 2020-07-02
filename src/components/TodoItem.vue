<template>
  <div>
    <li>
      <span v-bind:class="{ done: todo.completed }">
        <input
          v-if="todo.completed"
          type="checkbox"
          v-on:change="todo.completed = !todo.completed"
          checked
        />
        <input
          v-else
          type="checkbox"
          v-on:change="todo.completed = !todo.completed"
        />
        <strong>{{ index + 1 }}</strong>
        {{ todo.title | uppercase }}
      </span>
      <button class="rm" v-on:click="$emit('remove-todo', todo.id)">
        &times;
      </button>
    </li>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component({
  filters: {
    uppercase(value: string): string {
      return value.toUpperCase();
    }
  }
})
export default class TodoItem extends Vue {
  @Prop({ required: true }) private index!: number;
  @Prop({ required: true }) private todo!: {
    id: number;
    title: string;
    completed: boolean;
  };
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
li {
  border: 1px solid #ccc;
  display: flex;
  justify-content: space-between;
  padding: 0.5rem 2rem;
  margin-bottom: 1rem;

  .rm {
    background: red;
    color: #fff;
    border-radius: 50%;
    font-weight: bold;
    width: 2rem;
    height: 2rem;
  }

  input {
    margin-right: 1rem;
  }

  .done {
    text-decoration: line-through;
  }
}
</style>
