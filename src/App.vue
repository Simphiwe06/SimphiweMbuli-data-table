<template>
  <div id="app">
    <h1>User Data Table</h1>
    <DataTable />
  </div>
</template>

<script>
import DataTable from './components/DataTable.vue';

export default {
  name: 'App',
  components: {
    DataTable,
  },
};
</script>

<style>
#app {
  text-align: center;
  padding: 20px;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}
</style>
