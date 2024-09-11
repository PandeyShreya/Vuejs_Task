<template>
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">{{ player.name }}</h5>
        <p class="card-text">Matches: {{ player.matches }}</p>
        <p class="card-text">Runs: {{ player.runs }}</p>
        <p class="card-text">50s/100s: {{ player['50s'] }}/{{ player['100s'] }}</p>
        <p class="card-text">Highest Score: {{ player.highest_score }}</p>
        <p class="card-text">Team Name: {{ player.team_name }}</p>
        <p class="card-text">Best Bowling Figures: {{ player.best_bowling_figures }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'CardView',
    props: {
      player: Object
    }
  }
  </script>
  
  <style scoped>
  .card {
    border: 1px solid #e0e0e0;
    border-radius: 0.5rem;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    background-color: #ffffff;
    margin-bottom: 1rem;
  }
  
  .card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0,0,0,0.2);
  }
  
  .card-body {
    padding: 1rem;
    text-align: left;
  }
  
  .card-title {
    font-size: 1.25rem;
    font-weight: bold;
    color: #007bff;
  }
  
  .card-text {
    margin-bottom: 0.5rem; /* Adjusted spacing between card-text elements */
    color: #495057;
  }
  </style>
  