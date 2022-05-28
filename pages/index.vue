<template>
  <section>
    <v-row> <h1>Nuxt-Todo-App</h1></v-row>
    <v-row>
      <h3>
        <v-form @submit.prevent>
          Add task: <v-text-field v-model="task"></v-text-field>
          <v-btn @click="addTask">submit</v-btn>
        </v-form>
      </h3>
    </v-row>
    <v-row>
      <v-data-table :headers="headers" :items="tasks">
        <template v-slot:item.isDone="{ item }">
          <v-simple-checkbox v-model="item.isDone"></v-simple-checkbox>
        </template>
      </v-data-table>
    </v-row>
  </section>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs } from "@nuxtjs/composition-api";

interface Task {
  name: string;
  isDone: boolean;
}

export default defineComponent({
  setup() {
    const state = reactive({
      task: "",
      tasks: [] as Task[],
      headers: [
        { text: "taskname", value: "name" },
        { text: "進捗", value: "isDone" },
      ],
    });

    const addTask = () => {
      const taskObj: Task = { name: state.task, isDone: false };
      state.tasks.push(taskObj);
      state.task = "";
    };

    const removeTask = (index: number) => {
      state.tasks.splice(index, 1);
    };

    return {
      ...toRefs(state),
      addTask,
      removeTask,
    };
  },
});
</script>
