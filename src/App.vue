<template>
  <div class="container">
    <h1>ITEM FINDER</h1>
    <div class="filter">
      <select name="year" id="year" v-model="filter.year">
        <option v-if="filter.year == ''" selected="" value="">SELECT YEAR</option>
        <option v-for="y in year" :value="y">{{ y }}</option>
      </select>
      <select name="type" id="type" v-model="filter.type">
        <option v-if="filter.type == ''" selected="" value="">SELECT TYPE</option>
        <option v-for="t in type" :value="t">{{ t }}</option>
      </select>
      <select name="cost" id="cost" v-model="filter.cost">
        <option v-if="filter.cost == ''" selected="" value="">SELECT COST</option>
        <option v-for="c in cost" :value="c">{{ c }}</option>
      </select>
      <button @click="filter = {
        year: '',
        type: '',
        cost: '',
        details: ''
      }">RESET</button>
    </div>

    <table v-if="filteredItems.length > 0">
      <thead>
        <tr>
          <th>YEAR</th>
          <th>TYPE</th>
          <th>COST</th>
          <th>DETAILS</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in filteredItems">
          <td>
            <p>
              {{ item.YEAR }}
            </p>
          </td>
          <td>
            <p>
              {{ item.TYPE }}
            </p>
          </td>
          <td>
            <p>
              {{ item.COST }}
            </p>
          </td>
          <td>
            <p v-for="i in item.DETAIL.split('\n')">
              {{ i }} <br>
            </p>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>Item Not Found</p>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import { Items } from './database/Items.json';

const filter = ref({
  year: '',
  type: '',
  cost: '',
  details: ''
});

const filteredItems = computed(() => {
  return Items.filter(item => {
    return (
      (filter.value.year ? item.YEAR === filter.value.year : true) &&
      (filter.value.type ? item.TYPE === filter.value.type : true) &&
      (filter.value.cost ? item.COST === filter.value.cost : true) &&
      (filter.value.details ? item.DETAIL === filter.value.details : true)
    );
  });
})

const year = computed(() => {
  const year = [];

  Items.forEach(item => {
    if (!year.includes(item.YEAR)) {
      year.push(item.YEAR);
    }
  });

  return year.sort((a, b) => a - b);
})

const type = computed(() => {
  const type = [];

  Items.forEach(item => {
    if (!type.includes(item.TYPE)) {
      type.push(item.TYPE);
    }
  });

  return type;
})

const cost = computed(() => {
  const cost = [];

  Items.forEach(item => {
    if (!cost.includes(item.COST)) {
      cost.push(item.COST);
    }
  });

  return cost.sort((a, b) => a - b);;
})
</script>

<style scoped></style>