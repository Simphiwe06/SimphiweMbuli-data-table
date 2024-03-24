<template>
    <div class="data-table">
      <div class="filters">
        <label for="designation">Select Designation:</label>
        <select id="designation" v-model="selectedDesignation">
          <option value="">All</option>
          <option v-for="designation in designations" :key="designation">{{ designation }}</option>
        </select>
        <input type="text" v-model="searchTerm" placeholder="Search...">
      </div>
      <table>
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
        <button @click="previousPage" :disabled="currentPage === 1">Previous</button>
        <span>Page {{ currentPage }} of {{ totalPages }}</span>
        <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
      </div>
    </div>
  </template>
  
  <script>
  import users from '@/users.json';
  
  export default {
    data() {
      return {
        users: users,
        selectedDesignation: '',
        searchTerm: '',
        currentPage: 1,
        pageSize: 10, // Number of users per page
      };
    },
    computed: {
      designations() {
        return [...new Set(this.users.map(user => user.designation))];
      },
      filteredUsers() {
        let filtered = this.users;
  
        // Filter by designation
        if (this.selectedDesignation) {
          filtered = filtered.filter(user => user.designation === this.selectedDesignation);
        }
  
        // Filter by search term
        if (this.searchTerm) {
          const searchTermLower = this.searchTerm.toLowerCase();
          filtered = filtered.filter(user => 
            user.name.toLowerCase().includes(searchTermLower) ||
            user.surname.toLowerCase().includes(searchTermLower) ||
            user.designation.toLowerCase().includes(searchTermLower) ||
            user.department.toLowerCase().includes(searchTermLower)
          );
        }
  
        // Pagination
        const startIndex = (this.currentPage - 1) * this.pageSize;
        return filtered.slice(startIndex, startIndex + this.pageSize);
      },
      totalPages() {
        return Math.ceil(this.filteredUsers.length / this.pageSize);
      },
    },
    methods: {
      previousPage() {
        if (this.currentPage > 1) {
          this.currentPage--;
        }
      },
      nextPage() {
        if (this.currentPage < this.totalPages) {
          this.currentPage++;
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .data-table {
    margin-top: 20px;
  }
  
  .filters {
    margin-bottom: 20px;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
  }
  
  th, td {
    border: 1px solid #ccc;
    padding: 8px;
    text-align: left;
  }
  
  th {
    background-color: #f2f2f2;
  }
  
  .pagination {
    margin-top: 20px;
    text-align: center;
  }
  
  .pagination button {
    margin: 0 5px;
  }
  </style>
  