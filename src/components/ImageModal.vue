<template>
  <div class="modal" v-if="isOpen" @click="closeModal">
    <div class="modal-content" @click.stop>
      <span class="close" @click="closeModal">&times;</span>
      <img :src="imageSrc" alt="Full Size" class="full-image" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';

const emit = defineEmits(['close']);

const props = defineProps({
  isOpen: {
    type: Boolean,
    required: true,
  },
  imageSrc: {
    type: String,
    required: true,
  },
});

const closeModal = () => {
  emit('close');
};
</script>

<style scoped>
.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.8);
  z-index: 1000;
}

.modal-content {
  position: relative;
  max-width: 90%;
  max-height: 90%;
}

.close {
  position: absolute;
  top: 10px;
  right: 20px;
  color: white;
  font-size: 28px;
  cursor: pointer;
}

.full-image {
  max-width: 100%;
  max-height: 100%;
  border-radius: 8px;
}
</style>
