<template>
  <div class="app">
    <div class="app__title">Вариант селекта где передаем колбэк пропсом</div>
    <VSelect
      :value="selectValue"
      :options="options"
      :changeSelectValue="changeSelectValue"
      placeholder="Выберите название региона"
    />
    <div class="app__title">Вариант селекта где вызываем колбэк по событию</div>
    <VSelect
      :value="selectValue"
      :options="options"
      placeholder="Выберите название региона"
      @changeValue="changeSelectValue"
    />
    <div class="app__title">таблица</div>
    <VTable />
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue';
import VSelect from '@/components/VSelect.vue';
import { options } from '@/consts/select';
import VTable from '@/components/VTable.vue';

const selectValue = ref('');
const changeSelectValue = (newVal: string) => selectValue.value = newVal;
</script>
<style scoped>
.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  align-items: center;
}

.app__title {
  margin: 24px;
}
</style>
