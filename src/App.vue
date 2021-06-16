<template>
  <v-app>
    <v-app-bar clipped-left flat app dense class="primary-nav">
      <v-toolbar-title>Diligent</v-toolbar-title>
    </v-app-bar>
    <v-main>
      <div class="pa-4">
        <router-view />
      </div>
    </v-main>
    <v-navigation-drawer clipped app width="300" fixed class="tasks-panel">
      <v-list flat>
        <v-subheader class="text-h5">Tasks</v-subheader>
      </v-list>
      <v-list-item-group>
        <v-list-item v-for="task in tasks" :key="task.id" two-line>
          <v-list-item-content>
            <v-list-item-title>{{ task.title }}</v-list-item-title>
            <v-list-item-subtitle>{{ task.description }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-list-item-group>
    </v-navigation-drawer>
  </v-app>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "App",

  data: () => ({
    drawer: true,
    tasks: [
      {
        title: "Task 1",
        id: "task1",
        description: "Blah Blah",
      },
      {
        title: "Task 2",
        id: "task2",
        description: "Another thing",
      },
    ],
  }),
});
</script>

<style lang="scss">
.tasks-panel {
  top: 48px !important;
  height: calc(100vh - 48px) !important;
}
</style>
