<template>
  <div>
    <NavBar @team-selected="handleTeamSelection" @search-players="handleSearchQuery" />

    <!-- Batsman Section -->
    <div class="container">
      <div v-if="hasBatsmen" class="text-center mb-4">
        <h1>Batsman</h1>
      </div>
      <div v-if="hasBatsmen" class="row card-row">
        <CardView v-for="player in filteredPlayers(2)" :key="player.name" :player="player" />
      </div>

      <!-- Bowler Section -->
      <div v-if="hasBowlers" class="text-center mb-4">
        <h1>Bowler</h1>
      </div>
      <div v-if="hasBowlers" class="row card-row">
        <CardView v-for="player in filteredPlayers(3)" :key="player.name" :player="player" />
      </div>

      <!-- All-Rounder Section -->
      <div v-if="hasAllRounders" class="text-center mb-4">
        <h1>All-Rounder</h1>
      </div>
      <div v-if="hasAllRounders" class="row card-row">
        <CardView v-for="player in filteredPlayers(4)" :key="player.name" :player="player" />
      </div>
    </div>
  </div>
</template>

<script>
import NavBar from './Navbar.vue';
import CardView from './Card.vue';

export default {
  name: 'SportzInteractive',
  components: {
    NavBar,
    CardView
  },
  data() {
    return {
      searchQuery: '',
      selectedTeam: 'ALL',
      players: []
    };
  },
  computed: {
    filteredPlayers() {
      return (role) => {
        return this.players
          .filter(player => {
            const matchesTeam = this.selectedTeam === 'ALL' || player.team_name === this.selectedTeam;
            const matchesSearch = player.name.toLowerCase().includes(this.searchQuery.toLowerCase());
            const matchesRole = player.role === role;
            return matchesTeam && matchesSearch && matchesRole;
          });
      };
    },
    hasBatsmen() {
      return this.filteredPlayers(2).length > 0;
    },
    hasBowlers() {
      return this.filteredPlayers(3).length > 0;
    },
    hasAllRounders() {
      return this.filteredPlayers(4).length > 0;
    }
  },
  methods: {
    handleTeamSelection(selectedTeam) {
      this.selectedTeam = selectedTeam;
    },
    handleSearchQuery(searchQuery) {
      this.searchQuery = searchQuery;
    }
  },
  created() {
    fetch('/players.json')
      .then(response => response.json())
      .then(data => {
        this.players = data.originalPlayers;
      })
      .catch(error => console.error('Error fetching player data:', error));
  }
}
</script>

<style scoped>
.container {
  margin-top: 20px;
}

.card-row {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem; 
}

.card-row > * {
  flex: 1 1 calc(25% - 1rem); 
}
</style>
