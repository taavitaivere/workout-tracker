<template>
  <v-app>
    <v-app>
      <v-app-bar color="grey-lighten-2" name="app-bar" class="justify-center">
        <div class="d-flex justify-center align-center w-100">
          <v-btn @click="">Add New</v-btn>
        </div>
      </v-app-bar>
      <v-navigation-drawer width="200" color="grey-darken-1" permanent>
        <div
          class="drop-zone"
          @drop="onDrop($event, 1)"
          @dragenter.prevent
          @dragover.prevent
        >
          <div
            v-for="item in getList(1)"
            :key="item.id"
            class="drag-el"
            draggable="true"
            @dragstart="startDrag($event, item)"
          >
            {{ item.title }}
          </div>
        </div>
        <div
          class="drop-zone"
          @drop="onDrop($event, 2)"
          @dragenter.prevent
          @dragover.prevent
        >
          <div
            v-for="item in getList(2)"
            :key="item.id"
            class="drag-el"
            draggable="true"
            @dragstart="startDrag($event, item)"
          >
            {{ item.title }}
          </div>
        </div>
      </v-navigation-drawer>
      <v-app-bar height="48" color="grey" elevation="0"></v-app-bar>
      <v-navigation-drawer
        position="right"
        width="150"
        color="grey-lighten-1"
        permanent
      ></v-navigation-drawer>
      <v-app-bar
        position="bottom"
        height="48"
        color="grey-lighten-2"
        elevation="0"
      ></v-app-bar>
      <v-main>
        <v-card elevation="0" height="400px"></v-card>
      </v-main>
    </v-app>
  </v-app>
</template>
<script>
import { ref } from "vue";
export default {
  setup() {
    const items = ref([
      { id: 0, title: "Item A", list: 1 },
      { id: 1, title: "Item A", list: 1 },
      { id: 2, title: "Item A", list: 2 },
    ]);
    const getList = (list) => {
      return items.value.filter((item) => item.list == list);
    };
    const startDrag = (event, item) => {
      console.log(item);
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("itemID", item.id);
    };
    const onDrop = (event, list) => {
      const itemID = event.dataTransfer.getData("itemID");
      const item = items.value.find((item) => item.id == itemID);
      item.list = list;
    };
    return {
      getList,
      onDrop,
      startDrag,
    };
  },
};
</script>
<style>
.drop-zone {
  width: 100%;
  margin: 50px auto;
  background-color: #42b983;
  padding: 10px;
  min-height: 10px;
}
.drag-el {
  background-color: #2c3e50;
  color: white;
  padding: 5px;
  margin-bottom: 10px;
}
.drag-el:nth-last-of-type(1) {
  margin-bottom: 0;
}
</style>
