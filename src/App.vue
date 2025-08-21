<script setup>
import {ref, watch} from 'vue';
import VueDatePicker from './components/Datepicker.vue';
import {parse, isValid, format} from 'date-fns';
import { MaskInput } from 'vue-3-mask';

const selectedDate = ref();
const inputDate = ref(selectedDate.value);

watch(inputDate, (val) => {
  if (!val) {
    selectedDate.value = null;
    return;
  }

  const parsed = parse(val, 'yyyy-MM-dd', new Date());
  if (isValid(parsed) && format(parsed, 'yyyy-MM-dd') === val) {
    selectedDate.value = val;
  }
})

</script>

<template>
  <h1>Test Vue 3 DatePicker</h1>

  <main class="main-block">
    <VueDatePicker v-model="selectedDate"
    />

    <div>
      <label for="MaskInput" class="label-select">Props: {{selectedDate}}</label>
      <div>
        <MaskInput v-model="selectedDate" mask="####-##-##" class="date-select" id="MaskInput"
        placeholder="YYYY-MM-DD" />
      </div>
    </div>

  </main>
</template>

<style scoped>

h1 {
  margin-bottom: 1.5rem;
}

.main-block {
  display: flex;
  gap: 24px;
  align-items: flex-start;
  flex-wrap: wrap;

  .date-select {
    padding: 8px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  .label-select {
    margin-bottom: 1rem;
    display: inline-block;
  }
}
</style>
