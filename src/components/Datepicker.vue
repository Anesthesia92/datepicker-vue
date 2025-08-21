<script setup>
import {computed, ref, watch} from 'vue';
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'

import {enUS, ru, zhCN} from 'date-fns/locale'
import {format, isValid, parseISO} from 'date-fns';

const props = defineProps({
  modelValue: String
})

const emit = defineEmits(['update:modelValue']);
const date = ref(props.modelValue || new Date());

const formattedDate = computed(() => {
  const code = locales[localeName.value].code;
  const formatted = formatDate(date) || '';

  if (code === 'ru') {
    return `Выбрана дата: ${formatted}`
  } else if (code === 'en-US') {
    return `Selected date is ${formatted}`
  } else {
    return `选择的日期是 ${formatted}`
  }
})

watch(
    () => props.modelValue,
    (val) => {
      if (!val || val === '') {
        date.value = new Date();
      } else {
        const parsed = parseISO(val)
        date.value = isValid(parsed) ? parsed : new Date();
      }
    },
)

watch(date, (newVal) => {
  if (!newVal) {
    emit('update:modelValue', null);
  } else {
    emit('update:modelValue', format(newVal, 'yyyy-MM-dd'));
  }
})
const localeName = ref('ru');

const locales = {
  en: enUS,
  ru: ru,
  zh: zhCN
};

const dateClicked = (date) => {
  emit('date-selected', formatDate(date));
}

const formatDate = (date) => {
  if (!date.value) return null;
  return format(date.value, 'yyyy-MM-dd');
}

const localeArray = [
  { label: 'Русский', value: 'ru' },
  { label: 'English', value: 'en' },
  { label: '中文', value: 'zh' }
]

function onLocaleChange(event) {
  localeName.value = event.target.value;
}

</script>

<template>
  <VueDatePicker v-model="date"
                 inline auto-apply
                 :clearable="true"
                 :hide-navigation="['time']"
                 @date-update="dateClicked"
                 :text-input="{ format: 'MM/dd/yyyy' }"
                 :locale="locales[localeName]"
                 dark
                 :format-locale="locales[localeName]"
  >

    <template #menu-header>
      <select
          class="select-input"
          :value="localeName"
          @change="onLocaleChange"
      >
        <option v-for="m in localeArray" :key="m.value" :value="m.value">{{ m.label }}</option>
      </select>

      <div class="format-date">{{formattedDate }}</div>
    </template>
  </VueDatePicker>

</template>

<style scoped>
.format-date {
  color: cornflowerblue;
  padding: 0.5rem;
}

.select-input {
  padding: 0.5rem;
  margin-bottom: 0.5rem;
  font-size: 14px;
  border-radius: 4px;
  border: 1px solid #ccc;
  background-color: white;
  cursor: pointer;
  transition: border-color 0.2s ease;
  &:hover {
    border-color: #3a86ff;
  }
}

.dp__main {
  width: auto;
}

</style>