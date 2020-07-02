<template>
  <form @submit.prevent="onSubmit">
    <input type="text" v-model="title" />
    <button type="submit">Create</button>
  </form>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class AddTodo extends Vue {
  @Prop() private title!: string;

  data() {
    this.title = "";

    return {
      title: this.title
    };
  }

  private onSubmit(): void {
    if (this.title.trim()) {
      const newTodo: { id: number; title: string; completed: boolean } = {
        id: Date.now(),
        title: this.title,
        completed: false
      };

      this.$emit("add-todo", newTodo);
      this.title = "";
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
form {
  display: flex;
  justify-content: center;

  input {
    width: 400px;
  }
}
</style>
