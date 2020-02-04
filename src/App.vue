<template>
  <div>
    <section class="todoapp">
      <todos-input :title="title" @newTodo="createTodo"/>
      <!-- This section should be hidden by default and shown when there are todos -->
      <section class="main">
        <!-- <input id="toggle-all" class="toggle-all" type="checkbox">
        <label for="toggle-all">Mark all as complete</label>-->
        <todos-list :todos="itemsFiltered" @deleteTodo="deleteTodo"/>
      </section>
      <!-- This footer should hidden by default and shown when there are todos -->
      <todos-actions
        v-show="todos.length>0"
        :items="itemsFiltered"
        @filterEvent="filterHandler"
        :filter="filter"
      />
    </section>
    <todos-footer/>
  </div>
</template>

<script>
import TodosInput from "./components/TodosInput";
import TodosActions from "./components/TodosActions";
import TodosList from "./components/TodosList";
import TodosFooter from "./components/TodosFooter";

export default {
  name: "App",
  components: {
    TodosInput,
    TodosActions,
    TodosList,
    TodosFooter
  },
  computed: {
    itemsFiltered() {
      if (this.filter === "all") return this.todos;

      if (this.filter === "completed")
        return this.todos.filter(x => x.isCompleted == true);

      return this.todos.filter(x => x.isCompleted == false);
    }
  },
  data() {
    return {
      title: "my todos",
      todos: [
        { id: 1, title: "Taste JavaScript", isCompleted: true },
        { id: 2, title: "Buy a unicorn", isCompleted: false },
        { id: 3, title: "Learn some vuejs ;)", isCompleted: false }
      ],
      filter: "all"
    };
  },
  methods: {
    createTodo(title) {
      if (title) {
        this.todos.push({
          id: this.todos.length + 1,
          title: title,
          isCompleted: false
        });
      }
    },
    deleteTodo(todo) {
      if (confirm("deleting ..." + todo.title))
        this.todos.splice(this.todos.indexOf(todo), 1);
    },
    filterHandler(filterValue) {
      this.filter = filterValue;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
