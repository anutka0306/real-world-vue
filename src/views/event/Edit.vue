<script setup>
const props = defineProps({
  event: {
    required:true
  }
})

</script>

<template>
  <p>Edit the event</p>
</template>

<style scoped>
.event-detail {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>