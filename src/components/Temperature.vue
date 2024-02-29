<script setup lang="ts">
import { computed } from "vue";
import { ref, onMounted } from 'vue';
import { format } from 'date-fns';

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

const currentDate = ref(new Date());
const currentDateFormatted = ref('');

onMounted(() => {
      // Format the date in (dd-mm-yyyy) format
      currentDateFormatted.value = format(currentDate.value, 'dd-MM-yyyy');
    });

const getDayName = () => {
  const dayOfWeek = new Date().getDay();

  switch (dayOfWeek) {
    case 0:
      return "Sunday";
    case 1:
      return "Monday";
    case 2:
      return "Tuesday";
    case 3:
      return "Wednesday";
    case 4:
      return "Thursday";
    case 5:
      return "Friday";
    case 6:
      return "Saturday";
    default:
      return "";
  }
};

const getCurrentTime = () => {
  const options = { hour: "numeric", minute: "numeric", hour12: true };
  return new Date().toLocaleTimeString("en-US", options);
};
</script>

<template>
  <div class="main" style="text-align: center">
    <div>
      <h1 style="font-size: 70px; font-weight: bolder">
        {{ temperatureInCelsius }} C
      </h1>
      <hr style="background-color: black" />
    </div>
    <div>
      <p>{{ currentDateFormatted }}</p>
      <h1 style="font-size: 30px">
        {{ getDayName() }} , {{ getCurrentTime() }}
      </h1>
    </div>
    <div>
      <p>{{ props.Output.name }} , {{ props.Output.sys.country }}</p>
    </div>
  </div>
</template>

<style scoped>
.main {
  height: 45vh;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
}
</style>
