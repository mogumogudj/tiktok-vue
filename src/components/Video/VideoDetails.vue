<script setup>
// define props
const props = defineProps(['description']);

</script>

<template>
  <div>
    <h2 class="username">Username</h2>
    <h3> {{ description }}</h3>
  </div>
</template>

<style scoped>
.video-details {
  padding: 16px;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  margin-top: 16px;
}

.username {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 8px;
}

.description {
  font-size: 16px;
  line-height: 1.4;
  color: #333;
}
</style>
