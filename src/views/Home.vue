<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete" />
      </div>
    </div>

    <p>Project length: {{ projects.length }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { onMounted } from 'vue';
import SingleProject from '../components/SingleProject.vue';

let projects = ref([]);

let handleDelete = (id) => {
  projects.value = projects.value.filter((project) => {
    return project.id !== id;
  });
}
let handleComplete = (id) => {
  let p = projects.value.find((project) => {
    return project.id === id;
  });
  p.complete = !p.complete;
}
// Variant 2 on Promise
onMounted(() => {
  fetch('http://localhost:3001/projects')
    .then((res) => res.json())
    .then((data) => (projects.value = data))
    .catch((err) => console.log(err.message));
});



// const error = ref(null)

// const load = async () => {
//   try {

//     let data = await fetch('http://localhost:3001/posts');
//     if (!data.ok) {
//       throw Error('no data avaliable');
//     }
//     projects.value = await data.json();
//   } catch (err) {
//     error.value = err.message;
//     console.log(error.value);
//   }
// };
</script>