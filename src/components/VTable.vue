<template>
  <div class="base-table">
    <table class="base-table__table">
      <thead class="base-table__header">
        <th
          v-for="(head, key) in tableHeaders"
          :key="`table-head${key}`"
          class="base-table__head"
          :class="`base-table__head_${key}`"
        >
          <div class="base-table__head-wrapper">
            <span>{{ head }}</span>
            <div class="base-table__head-buttons">
              <button
                class="base-table__head-button base-table__head-button_up"
                :class="{'base-table__head-button_active': sortType.colName === key && sortType.type === 'up'}"
                type="button"
                @click="toggleSort(key, 'up')"
              />
              <button
                class="base-table__head-button base-table__head-button_down"
                :class="{'base-table__head-button_active': sortType.colName === key && sortType.type === 'down'}"
                type="button"
                @click="toggleSort(key, 'down')"
              />
            </div>
          </div>
        </th>
      </thead>
      <tbody>
        <tr
          v-for="row in computedTableData"
          :key="row.id"
          class="base-table__row"
        >
          <td
            v-for="(_, key) in tableHeaders"
            :key="`table-data-${key}`"
            class="base-table__data"
          >
            <div v-if="key === 'deadline'">
              {{new Date(row[key as keyof TableData]).toLocaleDateString('en') }}
            </div>
            <div v-else>
              {{ row[key as keyof TableData] }}
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script setup lang="ts">
import { tableData, tableHeaders } from '@/consts/tables';
import type { TableData } from '@/models/tables';
import { computed, reactive } from 'vue';

const sortType = reactive({
  colName: '' as keyof TableData,
  type: '' as 'up' | 'down',
});

const toggleSort = (colName: keyof TableData, type: 'up' | 'down') => {
  sortType.colName = colName;
  sortType.type = type;
};

const computedTableData = computed(() => {
  if (sortType.colName && sortType.type) {
    return [...tableData].sort((a, b) => {
      const valueA = sortType.colName === 'deadline' ? new Date(a[sortType.colName]).getTime() : a[sortType.colName];
      const valueB = sortType.colName === 'deadline' ? new Date(b[sortType.colName]).getTime() : b[sortType.colName];

      if (typeof valueA === 'string' && typeof valueB === 'string') {
        return sortType.type === 'up' ? valueA.localeCompare(valueB) : valueB.localeCompare(valueA);
      }
      if (typeof valueA === 'number' && typeof valueB === 'number') {
        return sortType.type === 'up' ? valueA - valueB : valueB - valueA;
      }
      return 0;
    });
  }
  return tableData;
});

</script>
<style scoped>
.base-table {
  height: 600px;
  width: 800px;
  overflow: auto;
}

.base-table__head {
  text-align: start;
  background-color: #d4e0e8;
  font-size: 18px;
}

.base-table__data,
.base-table__head {
  padding: 16px;
}

.base-table__data {
  line-height: 120%;
  font-size: 18px;
}

.base-table__header {
  position: sticky;
  top: 0;
  background-color: #fff;
}

.base-table__table {
  width: 100%;
  border-collapse: collapse;
}

.base-table__row {
  border-bottom: 1px solid #d4e0e8;
}

.base-table__row:nth-child(even) {
  background-color: #ecf1f4;
}

.base-table__row {
  color: #688999;
}

.base-table__head-wrapper {
  display: flex;
  gap: 6px;
  justify-content: space-between;
  align-items: center;
}

.base-table__head-buttons {
  display: flex;
  flex-direction: column;
}

.base-table__head-button {
  cursor: pointer;
  width: 10px;
  height: 10px;
  border: none;
  box-sizing: border-box;
  mask-repeat: no-repeat;
  mask-size: contain;
  mask-position: 50%;
  background-color: #5b68cd;
}

.base-table__head-button_up {
  mask-image: url("@/assets/arrowSort.svg");
  transform: rotate(180deg);
}

.base-table__head-button_down {
  mask-image: url("@/assets/arrowSort.svg");
}

.base-table__head-button_active {
  background-color: #29277d;
}
</style>