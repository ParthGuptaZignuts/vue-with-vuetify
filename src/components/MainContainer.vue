<script setup lang="ts">
import InputUser from "./InputUser.vue";
import Trail from "./Trail.vue";
import Temperature from "./Temperature.vue";
import { ref } from "vue";
import axios, { AxiosResponse } from "axios";

type NullableString = string | null;

const emitValue = ref<NullableString>(null);
const sendingOutput = ref<NullableString>(null);
const errorOutput = ref<string | null>(null);

async function fetchData(inputValue: string): Promise<void> {
  emitValue.value = inputValue;

  try {
    const response: AxiosResponse = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?q=${emitValue.value}&appid=376891b216b4fbb8ab25637a32003e0d`
    );
    sendingOutput.value = response.data;
    errorOutput.value = null;
    console.log(response.data);
  } catch (error: any) {
    console.error("error fetching the data", error);
    errorOutput.value =
      (error?.response?.data?.message as string) || "Unknown error";
    sendingOutput.value = null;
  }
}

</script>

<template>
  <VApp>
    <VContainer style=" border-radius: 15px; ">
      <VRow>
        <VCol cols="4" style="background-color: white; color: black;">
          <VRow style="justify-content: center">
            <h1>The Weather App</h1>
          </VRow>
          <InputUser @submitForm="fetchData"/>
          <Temperature v-if="sendingOutput" :Output="sendingOutput"/>
        </VCol>
        <VCol cols="8" style="background-color: rgb(203, 203, 203); color: white;"> 
          <Trail :Output="sendingOutput" v-if="sendingOutput"/>
        </VCol>
      </VRow>
    </VContainer>
  </VApp>
</template>

