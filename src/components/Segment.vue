<script setup>
const props = defineProps(['dataObj', 'per', 'id']);
</script>

<template>
  <h2>
    <slot name="header"></slot>
  </h2>
  <p>
    <slot name="optionalNote"></slot>
  </p>
  <ul>
    <li v-for="data in dataObj.data" :key="data[id]">
      {{ data[per] }} - {{ new Intl.NumberFormat('en-US').format(data.population) }}
    </li>
  </ul>
  <p>Source: {{ dataObj.source }}</p>
</template>

<style>

</style>