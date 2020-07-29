<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <div>
      <input type="text" v-model="title" />
      <button type="button" @click="createItem()">新增</button>
    </div>
    <div v-for="(item, index) in items" :key="index">
      <input
        type="checkbox"
        :id="item.id"
        :name="item.title"
        :value="item.finished"
        :checked="checked(item.finished)"
        true-value="1"
        false-value="0"
        @click="updateItem(item)"
      />
      <label :for="item.id">{{ item.title }}</label>
      <button type="button" @click="deleteItem(item)">刪除</button>
    </div>
  </div>
</template>

<script>
const axios = require("axios");

export default {
  name: "App",
  data() {
    return {
      items: [],
      title: "",
    };
  },
  created() {
    this.getList();
  },
  methods: {
    async getList() {
      await axios
        .get("https://todolist-node-example.herokuapp.com/items")
        .then((response) => {
          console.log(response.data);
          this.items = response.data;
        });
    },
    checked(finished) {
      return finished ? "checked" : "";
    },
    async updateItem(item) {
      await axios
        .put("https://todolist-node-example.herokuapp.com/items/" + item.id, {
          finished: !item.finished ? "1" : "0",
        })
        .then((response) => {
          console.log(response.data);
          item.finished = response.data.finished;
        });
    },
    async deleteItem(item) {
      await axios
        .delete("https://todolist-node-example.herokuapp.com/items/" + item.id)
        .then((response) => {
          console.log(response.data);
          this.getList();
        });
    },
    async createItem() {
      await axios
        .post("https://todolist-node-example.herokuapp.com/items", {
          title: this.title,
        })
        .then((response) => {
          console.log(response.data);
          this.getList();
        })
        .finally(() => {
          this.title = '';
        });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
