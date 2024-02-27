<script setup lang="ts">
import { defineProps, ref, Ref } from 'vue';

interface WeatherItem {
  [key: string]: string | number | Date;
}

const props = defineProps({
  Output: {
    type: Object as () => any,
    default: () => ({}),
  },
});

const items: Ref<WeatherItem[]> = ref([
  { "Wind": props.Output.wind?.speed ?? "N/A" },
  { "Humidity": props.Output.main?.humidity ?? "N/A" },
  { "Real Feel": props.Output.main?.feels_like ?? "N/A" },
  { "Max Temp": props.Output.main?.temp_max ?? "N/A" },
  { "Min Temp": props.Output.main?.temp_min ?? "N/A" },
  { "Sun Rise": props.Output.sys?.sunrise ? new Date(props.Output.sys.sunrise * 1000).toLocaleTimeString() : "N/A" },
  { "Sun Set": props.Output.sys?.sunset ? new Date(props.Output.sys.sunset * 1000).toLocaleTimeString() : "N/A" },
  { "Sky": props.Output.weather?.[0]?.description ?? "N/A" }
]);
</script>

<template>
    <v-container>
      <v-row>
        <v-col v-for="(item, index) in items" :key="index" cols="6">
          <v-card>
            <v-card-title>{{ Object.keys(item)[0] }}</v-card-title>
            <v-card-text>
              {{ Object.values(item)[0] }}
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  