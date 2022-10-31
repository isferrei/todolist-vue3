<template>
  <div>
    <h1>Todo list</h1>
    <input
      type="text"
      v-focus
      v-model="state.currentTask"
      @keyup.enter="addTask"
    />
    <ul>
      <li
        v-for="(task, index) in state.tasks"
        :key="`${task}-${index}`"
        :class="{ completed: task.isDone }"
      >
        <input type="checkbox" @click="complete(task)" />
        {{ task.name }}
        <button @click="remove(task)">&times;</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { reactive } from "@vue/reactivity";
const focus = {
  inserted: (el) => {
    el.focus();
  },
};
export default {
  directives: {
    focus,
  },
  setup() {
    const state = reactive({
      currentTask: "",
      tasks: [{ name: "Make my bed", isDone: false }],
    });

    function addTask() {
      state.tasks.push({ name: state.currentTask, isDone: false });
      state.currentTask = "";
    }

    function remove(task) {
      state.tasks = state.tasks.filter((t) => t.name !== task.name);
    }

    function complete(task) {
      state.tasks = state.tasks.map((t) => {
        if (t.name === task.name) {
          return { ...t, isDone: !t.isDone };
        }
        return { ...t };
      });
    }

    return {
      state,
      complete,
      addTask,
      remove,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 10px;
}
.completed {
  text-decoration: line-through;
  color: #42b983;
}
a {
  color: #42b983;
}
</style>
