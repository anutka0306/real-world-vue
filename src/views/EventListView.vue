<script setup>
import EventCard from "@/components/EventCard.vue";
import {ref, onMounted, computed, watchEffect} from "vue";
import EventService from "@/services/EventService.js";

const events = ref(null)
const totalEvents = ref(0)
const props = defineProps({
  page: {

  }
})

onMounted(() => {
  watchEffect(() => {
    events.value = null
    EventService.getEvents(2, props.page)
        .then((response) => {
          events.value = response.data
          totalEvents.value = response.headers['x-total-count']
        })
        .catch((error) => {
          console.log(error)
        })
  })
})

const hasNextPage = computed(() => {
    let totalPages = Math.ceil(totalEvents.value / 2)
    return props.page < totalPages
});


</script>

<template>
    <div class="events">
      <h1>Events For Good</h1>
        <EventCard v-for="event in events" :key="event.id" :event="event"/>
      <div class="pagination">
        <router-link id="prev-page" :to="{ name: 'event-list', query: {page: props.page - 1}}"
                     rel="prev"
                     v-if="props.page != 1">
          &#60; Prev
        </router-link>

        <router-link id="next-page" :to="{ name: 'event-list', query: {page: props.page + 1}}"
                     rel="next"
                     v-if="hasNextPage"
                     >
          Next &#62;
        </router-link>
      </div>
    </div>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.pagination {
  display: flex;
  width: 290px;
}
.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}
#prev-page {
  text-align: left;
}
#next-page {
  text-align: right;
}
</style>
