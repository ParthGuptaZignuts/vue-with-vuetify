<script setup lang="ts">
import { computed } from "vue";
const props = defineProps(["Output"]);
const temperatureInCelsius = computed(() => {
  if (props.Output && props.Output.main && props.Output.main.temp) {
    const temperatureInKelvin = props.Output.main.temp;
    const temperatureInCelsius = temperatureInKelvin - 273.15; // Conversion from Kelvin to Celsius
    return temperatureInCelsius.toFixed(2);
  }
  return null ; 
});
</script>

<template>
  <div v-if="Output">
    <VCard height="300" width="400" style="padding: 15px">
      <VRow>
        <VCol> 
        </VCol>
        <VCol>
          <VRow>
            <h1>City Name: {{ props.Output.name }}</h1>
          </VRow>
          <VRow>
            <h2>Temperature in C: {{ temperatureInCelsius }}</h2>
          </VRow>
          <VRow>
            <h3>
              Description About the Sky :
              {{ props.Output.weather[0].description }}
            </h3>
          </VRow>
        </VCol>
      </VRow>
    </VCard>
  </div>
</template>
