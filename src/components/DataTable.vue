<template>
  <div class="data-table">
    <div class="filters">
      <select v-model="selectedDesignation" class="select">
        <option value="">All Designations</option>
        <option v-for="designation in designations" :key="designation" :value="designation">{{ designation }}</option>
      </select>
      <input type="text" v-model="searchTerm" placeholder="Search..." class="search-input">
    </div>
    <table class="table">
      <thead>
        <tr>
          <th>Name</th>
          <th>Surname</th>
          <th>Designation</th>
          <th>Department</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(user, index) in filteredUsers" :key="index">
          <td>{{ user.name }}</td>
          <td>{{ user.surname }}</td>
          <td>{{ user.designation }}</td>
          <td>{{ user.department }}</td>
        </tr>
      </tbody>
    </table>
    <div class="pagination">
      <button :disabled="currentPage === 1" @click="prevPage" class="pagination-button">Previous</button>
      <span class="pagination-text">{{ currentPage }}</span>
      <button :disabled="currentPage === totalPages" @click="nextPage" class="pagination-button">Next</button>
    </div>
  </div>
</template>

<script>
import usersData from '../../public/users.json';

export default {
  data() {
    return {
      users: usersData,
      selectedDesignation: '',
      searchTerm: '',
      pageSize: 9999,
      currentPage: 1,
    };
  },
  computed: {
    designations() {
      return [...new Set(this.users.map(user => user.designation))];
    },
    filteredUsers() {
      let filtered = this.users;
      if (this.selectedDesignation) {
        filtered = filtered.filter(user => user.designation === this.selectedDesignation);
      }
      if (this.searchTerm) {
        const searchTermLower = this.searchTerm.toLowerCase();
        filtered = filtered.filter(user =>
          user.name.toLowerCase().includes(searchTermLower) ||
          user.surname.toLowerCase().includes(searchTermLower) ||
          user.designation.toLowerCase().includes(searchTermLower) ||
          user.department.toLowerCase().includes(searchTermLower)
        );
      }
      return filtered;
    },
    totalPages() {
      return Math.ceil(this.filteredUsers.length / this.pageSize);
    },
  },
  methods: {
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
  },
};
</script>

<style scoped src="./DataTable.css"></style>
