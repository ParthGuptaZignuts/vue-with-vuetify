<script setup lang="ts">
import { computed} from "vue";

const props = defineProps({
  Output: {
    type: Object as () => any,
    default: null,
  },
});

const temperatureInCelsius = computed(() => {
  if (props.Output && props.Output.main && props.Output.main.temp) {
    const temperatureInKelvin = props.Output.main.temp;
    const temperatureInCelsius = temperatureInKelvin - 273.15; // Conversion from Kelvin to Celsius
    return temperatureInCelsius.toFixed(2);
  }
  return null;
});

const date = new Date();
const currentDate = date.getDate();
const currentMonth = date.getMonth();
const currentYear = date.getFullYear();
const fullDate = `${currentDate} - ${currentMonth} - ${currentYear}`;

const getDayName = () => {
  const dayOfWeek = new Date().getDay();

  switch (dayOfWeek) {
    case 0:
      return 'Sunday';
    case 1:
      return 'Monday';
    case 2:
      return 'Tuesday';
    case 3:
      return 'Wednesday';
    case 4:
      return 'Thursday';
    case 5:
      return 'Friday';
    case 6:
      return 'Saturday';
    default:
      return '';
  }
};

const getCurrentTime = () => {
  const options = { hour: 'numeric', minute: 'numeric', hour12: true };
  return new Date().toLocaleTimeString('en-US', options);
};

</script>

<template>
    <div style="text-align: center;">
        <h1 style="font-size:50px;">{{ temperatureInCelsius }}</h1>
        <hr style="background-color: black;">
        <div>
            <p>{{ fullDate }}</p>
            <h1 style="font-size: 30px;">{{ getDayName() }} , {{ getCurrentTime() }} </h1>
        </div>
        <div>
            <p>{{ props.Output.name }} , {{ props.Output.sys.country }}</p>
        </div>
    </div>
</template>

<style scoped>

</style>
