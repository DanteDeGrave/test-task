<template>
  <div
    class="select"
    :class="{'select_open': optionsListVisibility}"
  >
    <div class="select__content">
      <span v-if="value.length && !optionsListVisibility" class="select__value">
        {{ value }}
      </span>
      <span
        class="select__placeholder"
        :class="{ 'select__placeholder_active': value.length && !optionsListVisibility}"
      >
        {{ placeholder }}
      </span>
      <button
        class="select__show-btn"
        :class="{'select__show-btn_active': optionsListVisibility}"
        type="button"
        @click="toggleOptionsList"
      />
    </div>
    <ul
      class="select__options-list"
      :class="{'select__options-list_open': optionsListVisibility}"
      ref="optionsList"
    >
      <li
        v-for="option in options"
        :key="option.id"
        class="select__options-item"
        :class="{ 'select__options-item_selected': option.title === value }"
        @click="onPotionClick(option.title)"
      >
        {{ option.title }}
      </li>
    </ul>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue';
import type { SelectOption } from '@/models/select';

const props = defineProps<{
  value: string,
  placeholder: string,
  options: SelectOption[],
  changeSelectValue?: (option: string) => void,
}>();

const emits = defineEmits<{
  (e: 'changeValue', value: string ): void
}>()

const optionsListVisibility = ref(false);
const optionsList = ref<HTMLUListElement>();
const toggleOptionsList = () => {
  optionsListVisibility.value = !optionsListVisibility.value;
  if (!optionsList.value) { return; }
  if (optionsListVisibility.value) {
    optionsList.value.style.maxHeight = `${optionsList.value.scrollHeight}px`;
  } else {
    optionsList.value.style.maxHeight = '0';
  }
};

const onPotionClick = (value: string) => {
  toggleOptionsList();
  if (props.changeSelectValue) {
    props.changeSelectValue(value);
    return;
  }
  emits('changeValue', value);
}
</script>
<style scoped>
.select {
  position: relative;
  width: 300px;
  border-radius: 13px;
  box-sizing: border-box;
  box-shadow: 0 0 6px rgba(0, 0, 0, .15);
  transition: all .3s ease;
  color: #29277d;
}

.select_open {
  outline: 1px solid #f968bf;
  box-shadow: none;
}

.select__content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px;
}

.select__placeholder {
  transition: all .2s;
  color: rgba(41, 39, 125, 0.40);
}

.select__placeholder_active {
  position: absolute;
  top: 0;
  transform: translateY(-50%);
  font-size: 15px;
}

.select__options-list {
  display: flex;
  flex-direction: column;
  list-style: none;
  margin: 0;
  padding: 0;
  border-top: none;
  max-height: 0;
  overflow: hidden;
  transition: all .3s ease;
  border-radius: 0 0 13px 13px;
}

.select__options-list_open {
  border-top: 1px solid #f968bf;
}

.select__show-btn {
  border: none;
  width: 24px;
  height: 24px;
  background-image: url("@/assets/arrowSelect.svg");
  background-repeat: no-repeat;
  background-position: center;
  background-color: transparent;
  transition: all .3s ease;
}

.select__show-btn_active {
  transform: rotate(180deg);
}

.select__show-btn,
.select__options-item {
  cursor: pointer;
}


.select__options-item {
  padding: 4px 8px;
}

.select__options-item:hover {
  background-color: #dadefe;
}
</style>
