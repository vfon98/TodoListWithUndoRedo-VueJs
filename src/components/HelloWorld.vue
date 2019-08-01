<template>
  <div class="container">
    <h1>Todo App</h1>
    <form @submit.prevent class="row form-inline">
      <input v-model='newTodo' type="text" class="form-control" placeholder="Enter text" autofocus>
      <button @click='add()'  class="ml-2 btn btn-success">Add</button>
      <button @click='undo()' class="ml-2 btn btn-danger" :disabled='!canUndo'>Undo</button>
      <button @click='redo()' class="ml-2 btn btn-dark" :disabled='!canRedo'>Redo</button>

    </form>
    <div class="row mt-2">
      <ul>
        <li v-for='(todo, i) in todos'>
          {{ todo }}
          <button @click='del(i)' class="btn btn-outline-danger btn-sm">X</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data () {
      // const todos = ['NodeJs', 'ReactJs', 'AngularJs', 'VueJs'];
      const todos = [];
      const history = [[...todos]];
      const historyIndex = history.length - 1;
      return {
        todos,
        newTodo: undefined,
        history,
        historyIndex
      }
    },
    computed: {
      canUndo() {
        return this.historyIndex > 0;
      },
      canRedo() {
        return (this.history.length - 1) > this.historyIndex;
      }
    },
    methods: {
      add() {
        this.todos.push(this.newTodo);
        this.updateHistory();
        this.newTodo = undefined;
      },
      del(index) {
        this.todos.splice(index, 1);
        this.updateHistory();
      },
      updateHistory() {
        this.historyIndex++;
        this.history.splice(this.historyIndex);
        this.history.push([...this.todos]);
      },
      undo() {
        if(this.canUndo) {
          this.historyIndex--;
          this.todos = this.history[this.historyIndex];
        }
      },
      redo() {
        if(this.canRedo) {
          this.historyIndex++;
          this.todos = this.history[this.historyIndex];
        }
      }
    }
  }
</script>

<style lang="css" scoped>
</style>