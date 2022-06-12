<template>
  <v-layout>
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
          <template v-slot:item.name="props">
            <v-edit-dialog :return-value.sync="props.item.name"
              >{{ props.item.name }}
              <template v-slot:input>
                <v-text-field
                  v-model="props.item.name"
                  label="Edit"
                  single-line
                  counter
                ></v-text-field>
              </template>
            </v-edit-dialog>
          </template>
          <template v-slot:item.isDone="{ item }">
            <v-simple-checkbox v-model="item.isDone"></v-simple-checkbox>
          </template>
          <template v-slot:item.actions="{ item }">
            <v-icon small class="mr-2" @click="editTask(item)">
              mdi-pencil
            </v-icon>
            <v-icon small @click="removeTask(item)"> mdi-delete </v-icon>
          </template>
        </v-data-table>
      </v-row>
    </section>
  </v-layout>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs } from "@nuxtjs/composition-api";
import Vue from "vue";

interface Task {
  name: string;
  isDone: boolean;
}

export default defineComponent({
  setup() {
    const state = reactive({
      task: "",
      tasks: [] as Task[],
      snack: false,
      headers: [
        { text: "taskname", value: "name" },
        { text: "進捗", value: "isDone" },
        { text: "CRUD", value: "actions" },
      ],
    });

    const addTask = () => {
      const taskObj: Task = { name: state.task, isDone: false };
      state.tasks.push(taskObj);
      state.task = "";
    };

    const removeTask = (task: Task) => {
      state.tasks.splice(state.tasks.indexOf(task), 1);
    };

    return {
      ...toRefs(state),
      addTask,
      removeTask,
    };
  },
});
</script>
