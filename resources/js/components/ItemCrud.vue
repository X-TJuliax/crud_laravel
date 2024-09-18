<template>
    <div>
      <h1>Items</h1>
      <div>
        <input v-model="newItem.name" placeholder="Name" />
        <textarea v-model="newItem.description" placeholder="Description"></textarea>
        <button @click="createItem">Create</button>
      </div>
      <ul>
        <li v-for="item in items" :key="item.id">
          {{ item.name }} - {{ item.description }}
          <button @click="deleteItem(item.id)">Delete</button>
          <button @click="editItem(item)">Edit</button>
        </li>
      </ul>
      <div v-if="editingItem">
        <input v-model="editingItem.name" />
        <textarea v-model="editingItem.description"></textarea>
        <button @click="updateItem">Update</button>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        items: [],
        newItem: { name: '', description: '' },
        editingItem: null,
      };
    },
    mounted() {
      this.fetchItems();
    },
    methods: {
      async fetchItems() {
        const response = await axios.get('/api/items');
        this.items = response.data;
      },
      async createItem() {
        await axios.post('/api/items', this.newItem);
        this.newItem = { name: '', description: '' };
        this.fetchItems();
      },
      async deleteItem(id) {
        await axios.delete(`/api/items/${id}`);
        this.fetchItems();
      },
      async editItem(item) {
        this.editingItem = { ...item };
      },
      async updateItem() {
        await axios.put(`/api/items/${this.editingItem.id}`, this.editingItem);
        this.editingItem = null;
        this.fetchItems();
      }
    }
  }
  </script>
  