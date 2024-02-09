<!-- App.svelte -->
<script>
  import fil from "../pl.json"
  let json = JSON.parse(JSON.stringify(fil));
  import { onMount } from 'svelte';

  let rangertListeMål = [];
  let rangertListeAssist = [];
  let isButtonClickedMål = false; // Viser mållisten som standard
  let isButtonClickedAssist = false;

  async function fetchData() {
    const response = await fetch('../pl.json');
    json = await response.json();
    sortListByGoals(); // Sorterer listen etter mål som standard
  }

  function sortListByGoals() {
    json.elements.sort((a, b) => b.goals_scored - a.goals_scored);
    rangertListeMål = json.elements.map(spiller => `${spiller.first_name} ${spiller.second_name}: ${spiller.goals_scored} mål`);
    isButtonClickedMål = true;
    isButtonClickedAssist = false; // Resetter den andre knappen
  }

  function sortListByAssists() {
    json.elements.sort((a, b) => b.assists - a.assists);
    rangertListeAssist = json.elements.map(spiller => `${spiller.first_name} ${spiller.second_name}: ${spiller.assists} assists`);
    isButtonClickedMål = false; // Resetter den andre knappen
    isButtonClickedAssist = true;
  }

  onMount(fetchData);
</script>



<h1>Rankert Liste over Spillere</h1>




<div>
  <button on:click={sortListByGoals}>Vis rangert liste etter mål</button>
  <button on:click={sortListByAssists}>Vis rangert liste etter assist</button>
</div>






{#if isButtonClickedMål}
  <ol>
    {#each rangertListeMål as liste}
      <li>{liste}</li>
    {/each}
  </ol>
{/if}

{#if isButtonClickedAssist}
  <ol>
    {#each rangertListeAssist as liste}
      <li>{liste}</li>
    {/each}
  </ol>
{/if}
