<template>
  <div class="todo-app">
    <h2>{{ title }}</h2>
    <ul>
      <li v-for="item in filteredItems" :key="item.id" :class="{ 'completed': item.completed }">
        <span @click="toggleCompletion(item)">{{ item.text }}</span>
      </li>
    </ul>
    <div class="add-item">
      <input v-model="newItemText" type="text" placeholder="Add new item" />
      <button @click="addItem">Add</button>
    </div>
    <div class="folder-section">
      <input v-model="newFolderName" type="text" placeholder="New folder name" />
      <button @click="createFolder">Create Folder</button>
    </div>
    <div class="folder-selection">
      <select v-model="selectedFolder">
        <option value="">All</option>
        <option v-for="folder in folders" :key="folder.id" :value="folder.id">{{ folder.name }}</option>
      </select>
      <button @click="clearSelection">Clear Selection</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "To-Do List",
      items: [
        { id: 1, text: "Example item 1", completed: false, folderId: 1 },
        { id: 2, text: "Example item 2", completed: true, folderId: 2 },
        { id: 3, text: "Example item 3", completed: false, folderId: 1 },
      ],
      newItemText: "",
      newFolderName: "",
      folders: [
        { id: 1, name: "Folder 1" },
        { id: 2, name: "Folder 2" },
      ],
      selectedFolder: "",
    };
  },
  computed: {
    filteredItems() {
      if (this.selectedFolder) {
        const folderId = parseInt(this.selectedFolder);
        return this.items.filter((item) => item.folderId === folderId);
      }
      return this.items;
    },
  },
  methods: {
    addItem() {
      if (this.newItemText.trim() !== "") {
        this.items.push({
          id: Date.now(),
          text: this.newItemText,
          completed: false,
          folderId: this.selectedFolder ? parseInt(this.selectedFolder) : null,
        });
        this.newItemText = "";
      }
    },
    toggleCompletion(item) {
      item.completed = !item.completed;
    },
    createFolder() {
      if (this.newFolderName.trim() !== "") {
        this.folders.push({
          id: Date.now(),
          name: this.newFolderName,
        });
        this.newFolderName = "";
      }
    },
    clearSelection() {
      this.selectedFolder = "";
    },
  },
};
</script>

<style scoped>
.todo-app {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f7f7f7;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.todo-app h2 {
  margin-top: 50;
  margin-bottom: 20px;
  font-size: 24px;
  text-align: center;
  }
</style>