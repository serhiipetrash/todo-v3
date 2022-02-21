<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
      <div class="icons">
        <!-- <router-link :to="{ name: 'EditProject', params: { id: project.id } }">
          <span class="material-icons">edit</span>
        </router-link>-->

        <span class="material-icons" @click="deleteProject">delete</span>
        <span class="material-icons tick" @click="toggleComplete">done</span>
      </div>
    </div>
    <div v-if="showDetails" class="details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps(['project'])
const emit = defineEmits(['delete', 'complete'])

const showDetails = ref(false)
const uri = 'http://localhost:3001/projects/' + props.project.id

let deleteProject = () => {
  fetch(uri, { method: 'DELETE' })
    .then(() => emit('delete', props.project.id))
    .catch((err) => console.log(err));
}

let toggleComplete = () => {
  fetch(uri, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ complete: !props.project.complete }),
  })
    .then(() => {
      emit('complete', props.project.id);
    })
    .catch((err) => console.log(err));
}

</script>

<style>
.project {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
}
h3 {
  cursor: pointer;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.material-icons {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}
.material-icons:hover {
  color: #777;
}
.project.complete {
  border-left: 4px solid #00ce89;
}
.project.complete .tick {
  color: #00ce89;
}
</style>