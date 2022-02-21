<template>
  <form @submit.prevent="handleSubmit">
    <label>Title:</label>
    <input type="text" v-model="title" required />
    <label>Details:</label>
    <textarea v-model="details" required></textarea>
    <button>Update Project</button>
  </form>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router';

const props = defineProps(['id'])
const router = useRouter()

const title = ref('')
const details = ref('')
const uri = 'http://localhost:3001/projects/' + props.id

onMounted(() => {
  fetch(uri)
    .then((res) => res.json())
    .then((data) => {
      title.value = data.title;
      details.value = data.details;
    });
})
let handleSubmit = () => {
  fetch(uri, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ title: title.value, details: details.value }),
  })
    .then(() => {
      router.push('/');
    })
    .catch((err) => console.log(err));
}

</script>

<style></style>