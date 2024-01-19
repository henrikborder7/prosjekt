<script>
    import { onMount } from 'svelte';
  
    let teams = [];
  
    onMount(async () => {
      try {
        // Hent lagdata fra teams.json (antatt at den er i samme mappe)
        const response = await fetch('/teams.json');
        const data = await response.json();
        teams = data.data;
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    });
  
    function sortTeamsByDivision() {
      teams = teams.sort((a, b) => a.division.localeCompare(b.division));
    }
  </script>
  
  <style>
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
    }
  
    th, td {
      border: 1px solid #dddddd;
      text-align: left;
      padding: 8px;
    }
  
    th {
      background-color: #f2f2f2;
    }
  </style>
  
  <h1>NBA Teams</h1>
  
  <button on:click={sortTeamsByDivision}>Sort by Division</button>
  
  {#if teams.length > 0}
    <table>
      <thead>
        <tr>
          <th>Team Name</th>
          <th>Abbreviation</th>
          <th>Division</th>
          <th>Conference</th>
        </tr>
      </thead>
      <tbody>
        {#each teams as team}
          <tr>
            <td>{team.full_name}</td>
            <td>{team.abbreviation}</td>
            <td>{team.division}</td>
            <td>{team.conference}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  {:else}
    <p>Loading...</p>
  {/if}