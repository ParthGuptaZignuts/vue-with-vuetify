<script setup lang="ts">
import { ref, defineEmits, Ref } from "vue";

const inputValue: Ref<string | null> = ref<string | null>(null);
const emit = defineEmits(["submitForm"]);

const submitForm = () => {
  if (inputValue.value) {
    emit("submitForm", inputValue.value);
    inputValue.value = null;
  }
};
</script>

<template>
  <v-form ref="form" @submit.prevent="submitForm">
    <v-card class="mt-5">
      <v-card-text>
        <v-text-field
          v-model="inputValue"
          label="City Name"
          :rules="[v => !!v || 'Field is required']"
          required
        ></v-text-field>
      </v-card-text>
    </v-card>
  </v-form>
</template>
