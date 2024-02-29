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

const fetchData = async (inputValue: string): Promise<void>  => {
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
  <VApp style="background-color: rgb(61, 61, 61);">
    <VContainer class="d-flex align-center justify-center" style="background-color:rgb(255, 255, 255); height: 100vh; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);">
      <VRow>
        <VCol cols="4" style="background-color: white; color: black; padding: 20px;">
          <VRow style="justify-content: center">
            <h1>The Weather App</h1>
          </VRow>
          <InputUser @submitForm="fetchData"/>
          <v-alert v-if="errorOutput" text="City Not Found Enter Valid city" class="mt-5" type="error" variant="tonal"></v-alert>
          <Temperature v-if="sendingOutput" :Output="sendingOutput"/>
        </VCol>
        <VCol cols="8" style="background-color: rgb(203, 203, 203); color: white;"> 
          <Trail :Output="sendingOutput" v-if="sendingOutput"/>
        </VCol>
      </VRow>
    </VContainer>
  </VApp>
</template>

