<template>
  <v-container>
    <v-row align="center" justify="center">
      <v-col cols="12">
        <v-text-field
          color="#356859"
          label="ADD SOMETHING TODO..."
          v-model="message"
          append-icon="mdi-send"
          clearable
          clear-icon="mdi-close-circle"
          outlined
          type="text"
          @click:append="addTask"
          @click:clear="clearTask"
        >
        </v-text-field>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12">
        <v-card :elevation="0" outlined>
          <v-tabs v-model="tab" background-color="#fffbe6" color="#356859" grow>
            <v-tab v-for="(page, index) in pages" :key="index" :value="index">
              {{ page }}
            </v-tab>
          </v-tabs>

          <v-window v-model="tab">
            <v-window-item
              v-for="(page, index) in pages"
              :key="index"
              :value="index"
            >
              <v-card flat>
                <v-container style="background-color: #fffbe6">
                  <!-- tasks -->
                  <div v-for="(task, index) in selected_tasks" :key="index">
                    <v-row align="center" justify="center">
                      <v-col>
                        <v-checkbox
                          v-if="!task.edit"
                          color="#356859"
                          v-model="task.status"
                          :value="task.status"
                        >
                          <template v-slot:label>
                            <span
                              :style="
                                task.status === true
                                  ? `text-decoration: line-through`
                                  : null
                              "
                            >
                              {{ task.todo }}
                            </span>
                          </template>
                        </v-checkbox>
                        <v-text-field
                          v-else
                          color="#356859"
                          label="EDIT TASK"
                          v-model="task.todo"
                          outlined
                          type="text"
                          hide-details
                        >
                        </v-text-field>
                      </v-col>
                      <v-col style="text-align: end">
                        <v-icon
                          :style="main_style"
                          class="mr-3"
                          @click="editTask(task)"
                        >
                          {{ task.edit === true ? `mdi-check` : `mdi-pencil` }}
                        </v-icon>
                        <v-icon :style="main_style" @click="deleteTask(index)"
                          >mdi-delete</v-icon
                        >
                      </v-col>
                    </v-row>
                  </div>

                  <!-- overview -->
                  <v-col cols="12" style="text-align: end">
                    <span class="mr-3">
                      <span :style="main_style">{{ tasks_left }}</span>
                      MORE TASKS LEFT
                    </span>
                    <a :style="main_style" @click="clearTasks"> CLEAR ALL </a>
                  </v-col>
                </v-container>
              </v-card>
            </v-window-item>
          </v-window>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "TodoList",

  data: () => ({
    message: "",

    tab: null,
    pages: ["All", "Doing", "Done"],

    tasks: [
      {
        todo: "Do Homework",
        status: false,
        edit: false,
      },
      {
        todo: "Wash Dishes",
        status: true,
        edit: false,
      },
    ],

    //
    main_style: `color:#356859`,
  }),

  methods: {
    addTask() {
      // if empty, return error
      if (this.message === "") {
        alert("PLEASE ADD SOMETHING TODO...");
        return;
      }

      // else
      // 1. add to tasks
      this.tasks.push({
        todo: this.message,
        status: false,
        edit: false,
      });
      // 2. clear message
      this.clearTask();
    },
    clearTask() {
      this.message = "";
    },

    editTask(task) {
      task.edit = !task.edit;
      console.log(task);
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    clearTasks() {
      this.tasks = [];
    },
  },

  computed: {
    selected_tasks() {
      let selected_tasks =
        this.tab === 0
          ? this.tasks
          : this.tab === 1
          ? this.tasks.filter(function (task) {
              return task.status !== true;
            })
          : this.tasks.filter(function (task) {
              return task.status === true;
            });

      return selected_tasks;
    },
    tasks_left() {
      return this.tasks.filter(function (task) {
        return task.status !== true;
      }).length;
    },
  },
};
</script>

<style>
.bg-basil {
  background-color: #fffbe6 !important;
}
</style>
