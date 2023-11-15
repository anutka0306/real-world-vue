<script setup>
import { ref, onMounted } from 'vue'
import EventService from "@/services/EventService.js";

const event = ref(null)
const props = defineProps({
  id: {
    required:true
  }
})

onMounted(() => {
  EventService.getEvent(props.id)
      .then((response) => {
        event.value = response.data
      })
      .catch((error) => {
        console.log(error)
      })
})
</script>

<template>
  <div class="event-detail" v-if="event">
    <h1> {{ event.title }}</h1>
    <div id="nav">
      <router-link :to="{ name: 'event-details'}">Details</router-link>
      <router-link :to="{ name: 'event-register'}">Register</router-link>
      <router-link :to="{ name: 'event-edit'}">Edit</router-link>
    </div>
    <router-view :event="event" />
  </div>
</template>

<style scoped>
.event-detail {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>