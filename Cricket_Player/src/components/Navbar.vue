<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">
        Sportz Interactive
      </a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <select v-model="selectedTeam" @change="updateSelectedTeam" class="form-select team-select">
          <option value="ALL">All Teams</option>
          <option value="IND">India</option>
          <option value="PAK">Pakistan</option>
          <option value="AUS">Australia</option>
          <option value="ENG">England</option>
          <!-- Add other countries as needed -->
        </select>
        <form class="d-flex search-form" role="search" @submit.prevent="updateSearchQuery">
          <input class="form-control search-input" type="search" placeholder="Search" v-model="searchQuery">
          <button class="btn btn-outline-light search-button" type="submit">Search</button>
        </form>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'NavBar',
  data() {
    return {
      selectedTeam: 'ALL',
      searchQuery: ''
    };
  },
  methods: {
    updateSelectedTeam() {
      this.$emit('team-selected', this.selectedTeam);
    },
    updateSearchQuery() {
      this.$emit('search-players', this.searchQuery);
    }
  }
}
</script>

<style scoped>
/* Navbar Styling */
.navbar {
  background: linear-gradient(90deg, rgba(0,123,255,1) 0%, rgba(0,150,136,1) 100%);
}

.navbar-brand {
  display: flex;
  align-items: center;
  font-weight: bold;
}

.navbar-logo {
  width: 40px;
  height: 40px;
  margin-right: 10px;
}

.navbar-toggler {
  border: none;
}

.navbar-toggler-icon {
  background-image: url('data:image/svg+xml;charset=utf8,%3Csvg viewBox="0 0 30 30" width="30" height="30" xmlns="http://www.w3.org/2000/svg"%3E%3Cpath stroke="%23fff" stroke-width="2" d="M5 7h20M5 15h20M5 23h20"/%3E%3C/svg%3E');
}

.team-select {
  margin-right: 20px;
  font-size: 0.875rem; /* Smaller font size for dropdown */
  padding: 0.375rem 0.75rem; /* Reduced padding */
  height: 2.5rem; /* Smaller height */
}

.search-form {
  display: flex;
  align-items: center;
}

.search-input {
  flex-grow: 1; /* Make input take available space */
  border-radius: 50px;
  border: 1px solid #007bff;
  padding: 0.5rem 1rem; /* Increase padding for larger input */
  margin-right: 10px; /* Space between input and button */
}

.search-button {
  border-radius: 50px;
  border: 1px solid #007bff;
  background-color: #007bff;
  color: white;
  padding: 0.5rem 1rem; /* Padding for button */
}

.search-button:hover {
  background-color: #0056b3;
  border-color: #004494;
}
</style>
