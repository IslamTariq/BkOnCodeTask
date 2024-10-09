<template>
  <div class="data-view-container">
    <div style="display: flex; margin: 12px; justify-content: flex-end">
      <Button label="Add New User" @click="openUserForm" />
    </div>

    <div class="controls">
      <input
        style="background-color: #ffffff; color: #333"
        type="text"
        placeholder="Search person by name..."
        v-model="searchQuery"
      />
      <div class="icon-container">
        <span
          class="material-icons"
          @click="setGridView"
          :class="{ active: isGridView }"
          title="Grid View"
          >grid_view</span
        >
        <span
          class="material-icons"
          @click="setListView"
          :class="{ active: !isGridView }"
          title="List View"
          >view_list</span
        >
      </div>
      <Button label="Select All / Deselect All" @click="toggleSelectAll" />
    </div>

    <div v-if="isGridView" class="grid">
      <div class="item" v-for="user in filteredData" :key="user.id">
        <div class="item-content">
          <img
            :src="user.image"
            alt="User Image"
            class="user-image"
            @click="openModal(user.image)"
          />
          <h4 class="user-name">{{ user.name }}</h4>
          <p class="user-description">{{ user.description }}</p>
          <p class="user-phone">{{ user.phone }}</p>
          <p class="user-email">{{ user.email }}</p>
          <div class="actions">
            <Button
              style="margin: 2px"
              iconPosition="before"
              @click="editUser(user)"
            >
              <template #icon>
                <span class="material-icons">edit</span>
              </template>
            </Button>
            <Button
              style="margin: 2px"
              iconPosition="before"
              @click="confirmDelete(user.id)"
            >
              <template #icon>
                <span class="material-icons">delete</span>
              </template>
            </Button>
          </div>
          <input type="checkbox" v-model="user.selected" />
        </div>
      </div>
    </div>

    <table v-else class="table">
      <thead>
        <tr>
          <th>
            <input
              type="checkbox"
              v-model="allSelected"
              @change="toggleSelectAll"
            />
          </th>
          <th>
            Name
            <span
              class="material-icons"
              style="cursor: pointer"
              @click.stop="toggleSortDirection('name')"
            >
              {{
                sortDirection.name === 'asc' ? 'arrow_upward' : 'arrow_downward'
              }}
            </span>
          </th>
          <th>Description</th>
          <th>Phone</th>
          <th>Email</th>
          <th>Image</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in filteredData" :key="user.id">
          <td><input type="checkbox" v-model="user.selected" /></td>
          <td>{{ user.name }}</td>
          <td>{{ user.description }}</td>
          <td>{{ user.phone }}</td>
          <td>{{ user.email }}</td>
          <td>
            <img
              :src="user.image"
              alt="User Image"
              class="img-small"
              @click="openModal(user.image)"
            />
          </td>
          <td>
            <Button
              style="margin: 2px"
              iconPosition="before"
              @click="editUser(user)"
            >
              <template #icon>
                <span class="material-icons">edit</span>
              </template>
            </Button>
            <Button
              style="margin: 2px"
              iconPosition="before"
              @click="confirmDelete(user.id)"
            >
              <template #icon>
                <span class="material-icons">delete</span>
              </template>
            </Button>
          </td>
        </tr>
      </tbody>
    </table>

    <ImageModal
      :isOpen="isModalOpen"
      :imageSrc="currentImage"
      @close="closeModal"
    />

    <UserForm
      v-if="isUserFormOpen"
      :isOpen="isUserFormOpen"
      :users="props.users"
      @close="closeUserForm"
      @submitUserData="handleUserFormSubmit"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, computed, defineProps, defineEmits } from 'vue';
import ImageModal from './ImageModal.vue';
import UserForm from './UserForm.vue';
import Button from './Button.vue';

interface User {
  id: number;
  name: string;
  description: string;
  image: string;
  phone: string;
  email: string;
  selected?: boolean;
}

const props = defineProps<{
  users: User[];
}>();

const emit = defineEmits(['addUser']);

const searchQuery = ref('');
const isGridView = ref(true);
const isModalOpen = ref(false);
const isUserFormOpen = ref(false);
const currentImage = ref<string>('');
const sortDirection = ref<{ name: 'asc' | 'desc' }>({ name: 'asc' });

const filteredData = computed(() => {
  let users = [...props.users].filter((user) => {
    return (
      user.name &&
      user.name.toLowerCase().includes(searchQuery.value.toLowerCase())
    );
  });

  // Sort the filtered data
  if (sortDirection.value.name === 'asc') {
    users.sort((a, b) => a.name.localeCompare(b.name));
  } else {
    users.sort((a, b) => b.name.localeCompare(a.name));
  }

  return users;
});

const allSelected = computed(() => {
  return props.users.every((user) => user.selected);
});

const openUserForm = () => {
  isUserFormOpen.value = true;
};

const closeUserForm = () => {
  isUserFormOpen.value = false;
};

const handleUserFormSubmit = (userData: Omit<User, 'id'>) => {
  emit('addUser', { ...userData, id: Date.now() });
  closeUserForm();
};

const toggleSelectAll = () => {
  const selectStatus = !allSelected.value;
  props.users.forEach((user) => {
    user.selected = selectStatus;
  });
};

const toggleView = () => {
  isGridView.value = !isGridView.value;
};

const setGridView = () => {
  isGridView.value = true;
};

const setListView = () => {
  isGridView.value = false;
};

const toggleSortDirection = (key: keyof User) => {
  sortDirection.value.name =
    sortDirection.value.name === 'asc' ? 'desc' : 'asc';
};

const editUser = (user: User) => {
  const newName = prompt('Edit user name:', user.name);
  if (newName && newName.trim()) {
    user.name = newName.trim();
  }
};

const confirmDelete = (id: number) => {
  if (confirm('Are you sure you want to delete this user?')) {
    deleteUser(id);
  }
};

const deleteUser = (id: number) => {
  const userIndex = props.users.findIndex((user) => user.id === id);
  if (userIndex !== -1) {
    props.users.splice(userIndex, 1);
  }
};

const openModal = (image: string) => {
  currentImage.value = image;
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};
</script>

<style scoped>
.data-view-container {
  max-width: 800px;
  margin: 20px auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.controls {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  flex-wrap: wrap;
}

.controls input {
  flex-grow: 1;
  padding: 10px;
  margin-right: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  min-width: 200px;
}

.icon-container {
  display: flex;
  align-items: center;
}

.icon-container .material-icons {
  font-size: 24px;
  margin-right: 15px;
  cursor: pointer;
  color: #007bff;
}

.icon-container .material-icons:hover {
  color: #0056b3;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}

.item {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 10px;
  text-align: center;
  transition: box-shadow 0.3s ease;
  background-color: #f9f9f9;
}

.item:hover {
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

.user-image {
  width: 100%;
  height: 150px;
  object-fit: cover;
}

.user-name {
  color: #666;
  font-size: 1.1rem;
  margin: 5px 0;
}

.user-description {
  font-size: 0.9rem;
  color: #666;
}

.user-phone,
.user-email {
  font-size: 0.8rem;
  color: #999;
}

.img-small {
  width: 50px;
  height: 50px;
  border-radius: 50%;
}

.table {
  width: 100%;
  border-collapse: collapse;
}

.table th,
.table td {
  color: #666;
  padding: 10px;
  border: 1px solid #ddd;
}

.table th {
  background-color: #f5f5f5;
}

.table tbody tr:hover {
  background-color: #f9f9f9;
}

@media (max-width: 768px) {
  .controls {
    flex-direction: column;
  }

  .controls input {
    margin-bottom: 10px;
    margin-right: 0;
  }

  .grid {
    grid-template-columns: 1fr;
  }

  .user-image {
    height: 120px;
  }
}

@media (max-width: 480px) {
  .data-view-container {
    padding: 10px;
  }
}
</style>
