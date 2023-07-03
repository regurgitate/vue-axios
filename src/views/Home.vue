<script setup>
import axios from 'axios';
import Segment from "../components/Segment.vue";
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
  <Segment :dataObj="nation" per="year" id="year">
    <template #header>
      United States population throughout the years
    </template>
  </Segment>
  <Segment :dataObj="states" per="state" id="id">
    <template #header>
      Population in every state in last year
    </template>
    <template #optionalNote>
      Note - the last recorded year with data available is <b>{{ states.latestYear }}</b>.
    </template>
  </Segment>
</template>

<style scoped>

</style>