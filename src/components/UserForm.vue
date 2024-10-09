<template>
  <div v-if="isOpen" class="modal">
    <div class="modal-content">
      <span class="close" @click="closeModal">&times;</span>
      <h3 class="modal-title">Add New User</h3>

      <form class="form-grid" @submit.prevent="submitForm">
        <div class="form-group">
          <label for="firstName">First Name</label>
          <input type="text" id="firstName" v-model="firstName" required />
        </div>

        <div class="form-group">
          <label for="lastName">Last Name</label>
          <input type="text" id="lastName" v-model="lastName" required />
        </div>

        <div class="form-group">
          <label for="phone">Phone Number</label>
          <input type="tel" id="phone" v-model="phoneNumber" required />
        </div>

        <div class="form-group">
          <label for="email">Email</label>
          <input type="email" id="email" v-model="email" required />
        </div>

        <div class="form-group">
          <label for="imageUpload">Upload Image</label>
          <input type="file" id="imageUpload" @change="handleImageUpload" />
        </div>

        <div class="form-group">
          <label for="description">Description</label>
          <textarea
            id="description"
            v-model="description"
            rows="4"
            placeholder="Enter a brief description..."
            required
          ></textarea>
        </div>

        <div class="form-action">
          <button type="submit" class="submit-btn">Add User</button>
          <button type="button" class="cancel-btn" @click="closeModal">
            Cancel
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, defineProps, defineEmits } from 'vue';

const props = defineProps({
  isOpen: {
    type: Boolean,
    required: true,
  },
});

const emit = defineEmits(['close', 'submitUserData']);

const firstName = ref('');
const lastName = ref('');
const phoneNumber = ref('');
const email = ref('');
const description = ref('');
const imageUrl = ref(null);

const handleImageUpload = (event: any) => {
  const input = event.target;
  if (input.files && input.files.length > 0) {
    const file = input.files[0];
    const reader = new FileReader();
    reader.onload = (e) => {
      imageUrl.value = e.target.result;
    };
    reader.readAsDataURL(file);
  }
};

const submitForm = () => {
  const userData = {
    name: `${firstName.value} ${lastName.value}`,
    description: description.value,
    image: imageUrl.value,
    phone: phoneNumber.value,
    email: email.value,
  };

  emit('submitUserData', userData);
  resetForm();
};

const closeModal = () => {
  emit('close');
};

const resetForm = () => {
  firstName.value = '';
  lastName.value = '';
  phoneNumber.value = '';
  email.value = '';
  description.value = '';
  imageUrl.value = null;
};
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
  width: 400px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
  position: relative;
  color: #333;
}

.close {
  cursor: pointer;
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 24px;
  color: #888;
}

.close:hover {
  color: #d00;
}

.form-group {
  margin-bottom: 15px;
}

label {
  font-weight: bold;
  margin-bottom: 5px;
  display: block;
}

input[type='text'],
input[type='tel'],
input[type='email'],
input[type='file'],
textarea {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  background-color: white;
  color: #333;
  transition: border-color 0.3s;
}

textarea {
  resize: none;
}

input[type='text']:focus,
input[type='tel']:focus,
input[type='email']:focus,
textarea:focus {
  border-color: #007bff;
}

.form-action {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}

.submit-btn,
.cancel-btn {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.submit-btn {
  background-color: #007bff;
  color: white;
}

.submit-btn:hover {
  background-color: #0056b3;
}

.cancel-btn {
  background-color: #6c757d;
  color: white;
}

.cancel-btn:hover {
  background-color: #5a6268;
}
</style>
