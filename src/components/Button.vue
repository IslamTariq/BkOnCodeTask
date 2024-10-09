<script lang="ts" setup>
const props = defineProps({
  label: String,
  iconPosition: {
    type: String,
    default: 'before',
  },
  variant: {
    type: String,
    default: 'primary',
  },
  href: String,
  onClick: Function,
});

const emit = defineEmits(['click']);

const handleClick = (event: Event) => {
  emit('click', event);
};
</script>

<template>
  <a :href="href" :class="['button', variant]" @click.prevent="handleClick">
    <slot v-if="props.iconPosition === 'before'" name="icon"></slot>
    <span v-if="label" class="button-label">{{ label }}</span>
    <slot v-if="props.iconPosition === 'after'" name="icon"></slot>
  </a>
</template>

<style scoped>
.button {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  text-decoration: none;
  transition:
    background-color 0.3s,
    transform 0.3s;
}

.button.primary {
  background-color: #007bff;
  color: white;
}

.button.secondary {
  background-color: #6c757d;
  color: white;
}

.button:hover {
  background-color: #0056b3;
  transform: translateY(-1px);
}

.button-label {
  margin-left: 5px;
}

.material-icons {
  font-size: 20px;
  vertical-align: middle;
}
</style>
