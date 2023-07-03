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
      },
      states: {
        data: [],
        source: null,
        latestYear: null
      }
    }
  },
  mounted() {
    axios('https://datausa.io/api/data?drilldowns=Nation&measures=Population')
      .then((response) => {
        console.log(response);
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
    axios('https://datausa.io/api/data?drilldowns=State&measures=Population&year=latest')
      .then((response) => {
        response.data.data.forEach(el => {
          console.log(el);
          this.states.data.push({
            id: el['ID State'],
            state: el.State,
            population: el.Population
          });
        });
        this.states.data.sort((a, b) => {
          return a.state - b.state;
        });
        this.states.latestYear = response.data.data[0]['ID Year'];
        this.states.source = `${response.data.source[0].annotations.source_name} - ${response.data.source[0].annotations.source_description}`;
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
  <p>Note - the last recorded year with data available is <b>{{ states.latestYear }}</b>.</p>
  <ul>
    <li v-for="data in states.data" :key="data.id">
      {{ data.state }} - {{ new Intl.NumberFormat('en-US').format(data.population) }}
    </li>
  </ul>
</template>

<style scoped>

</style>
