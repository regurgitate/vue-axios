<script setup>
import axios from 'axios';
</script>

<script>
export default {
  data() {
    return {
      nation: {
        data: [],
        source: null
      }
    }
  },
  mounted() {
    axios('https://datausa.io/api/data?drilldowns=Nation&measures=Population')
      .then((response) => {
        response.data.data.forEach(el => {
          this.nation.data.push({
            year: el['ID Year'],
            population: el['Population']
          })
        });
        this.nation.data.sort((a, b) => {
          return a.year - b.year;
        });
        this.nation.source = `${response.data.source[0].annotations.source_name} - ${response.data.source[0].annotations.source_description}`;
        
      });
  }
}
</script>

<template>
<h2>United States population throughout the years</h2>
<ul>
  <li v-for="data in nation.data" :key="data.year">
    {{ data.year }} - {{ new Intl.NumberFormat('en-US').format(data.population) }}
  </li>
</ul>
<p>Source: {{ nation.source }}</p>
<h2>Population in every state in last year</h2>
</template>

<style scoped>

</style>
