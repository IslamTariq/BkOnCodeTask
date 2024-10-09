<template>
  <div>
    <DataView :users="users" @addUser="addUser" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import DataView from './components/DataView.vue';

const users = ref([
  {
    id: 1,
    name: 'John Doe',
    description: 'Software Engineer',
    image: 'https://picsum.photos/200/300?random=1',
    phone: '123-456-7890',
    email: 'john.doe@example.com',
  },
  {
    id: 2,
    name: 'Jane Smith',
    description: 'Product Manager',
    image: 'https://picsum.photos/200/300?random=2',
    phone: '987-654-3210',
    email: 'jane.smith@example.com',
  },
  {
    id: 3,
    name: 'Alice Johnson',
    description: 'UX Designer',
    image: 'https://picsum.photos/200/300?random=3',
    phone: '456-789-0123',
    email: 'alice.johnson@example.com',
  },
  {
    id: 4,
    name: 'Bob Brown',
    description: 'Data Scientist',
    image: 'https://picsum.photos/200/300?random=4',
    phone: '321-654-9870',
    email: 'bob.brown@example.com',
  },
  {
    id: 5,
    name: 'Emily Davis',
    description: 'Web Developer',
    image: 'https://picsum.photos/200/300?random=5',
    phone: '789-123-4560',
    email: 'emily.davis@example.com',
  },
  {
    id: 6,
    name: 'Michael Wilson',
    description: 'Graphic Designer',
    image: 'https://picsum.photos/200/300?random=6',
    phone: '654-321-9870',
    email: 'michael.wilson@example.com',
  },
  {
    id: 7,
    name: 'Sarah Miller',
    description: 'Project Coordinator',
    image: 'https://picsum.photos/200/300?random=7',
    phone: '159-753-4862',
    email: 'sarah.miller@example.com',
  },
  {
    id: 8,
    name: 'David Lee',
    description: 'SEO Specialist',
    image: 'https://picsum.photos/200/300?random=8',
    phone: '852-963-7410',
    email: 'david.lee@example.com',
  },
  {
    id: 9,
    name: 'Jessica Taylor',
    description: 'Content Writer',
    image: 'https://picsum.photos/200/300?random=9',
    phone: '321-789-6543',
    email: 'jessica.taylor@example.com',
  },
  {
    id: 10,
    name: 'Chris Martin',
    description: 'Marketing Specialist',
    image: 'https://picsum.photos/200/300?random=10',
    phone: '123-321-1234',
    email: 'chris.martin@example.com',
  },
]);

const addUser = (userData: any) => {
  console.log(userData, 'UserData');
  const newUser = { ...userData, id: Date.now() };
  users.value.push(newUser);
};
</script>
