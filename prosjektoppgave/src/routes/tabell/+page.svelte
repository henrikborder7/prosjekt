<script>
    import { onMount } from "svelte";
    import Navigasjon from "../navigasjon/navigasjonsbar.svelte";
    import fil from "../plLag.json";
    let json = JSON.parse(JSON.stringify(fil));
    import fil2 from "../pl.json";
    import { finiteOrDefault } from "chart.js/dist/helpers/helpers.core";
    let json2 = JSON.parse(JSON.stringify(fil2));
    let topSkaarere = [];

    function sorterSpillere(spillere) {
        return spillere.sort((a, b) => b.goals_scored - a.goals_scored);
    }

    topSkaarere = sorterSpillere(json2.elements).slice(0, 20);


    onMount(() => {
        function findTeamName(event) {
            const teamName = event.target
                .closest("tr")
                .querySelector("td:nth-child(2)").textContent;
            return teamName;
        }

        document.querySelectorAll("table tbody tr").forEach((row) => {
            row.addEventListener("click", (event) => {
                const clickedTeam = findTeamName(event);
                console.log(clickedTeam); // Her kan du gjøre hva du vil med navnet på laget
            });
            
        });
    });




</script>

<Navigasjon />

<div class="hovedcontainer">
    <div class="tabell-container">
        <h1>Premier League Tabell 2023/24</h1>
        <table>
            <thead>
                <tr>
                    <th colspan="2">Lag</th>
                    <th>S</th>
                    <th>V</th>
                    <th>U</th>
                    <th>T</th>
                    <th>Mål</th>
                    <th>MF</th>
                    <th>P</th>
                </tr>
            </thead>
            <tbody>
                {#each json.teams as team}
                    <tr>
                        <td>{team.position}</td>
                        <td>{team.name}</td>
                        <td>{team.played}</td>
                        <td>{team.wins}</td>
                        <td>{team.draws}</td>
                        <td>{team.losses}</td>
                        <td>{team.goals_for} - {team.goals_against}</td>
                        <td>{team.goals_for - team.goals_against}</td>
                        <td>{team.points}</td>
                    </tr>
                {/each}
            </tbody>
        </table>
    </div>

    <div class="top-scorers">
        <h1>Toppscorere</h1>
        <table>
            <thead>
                <tr>
                    <th>#</th>
                    <th>Spiller</th>
                    <th>Mål</th>
                </tr>
            </thead>
            <tbody>
                {#each topSkaarere as player, index}
                    <tr>
                        <td>{index + 1}</td>
                        <td>{player.web_name}</td>
                        <td>{player.goals_scored}</td>
                    </tr>
                {/each}
            </tbody>
        </table>
    </div>
</div>

<style>
    .hovedcontainer {
        display: grid;
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 20px; /* Juster gapet etter behov */
    }

    .tabell-container {
        max-width: 600px; /* Juster bredden etter behov */
    }
    table {
        width: 100%;
        border-collapse: collapse;
    }

    th,
    td {
        padding: 10px;
        text-align: center;
    }

    thead {
        background-color: #f2f2f2;
    }

    tbody tr:nth-child(even) {
        background-color: #f2f2f2;
    }

    tbody tr:hover {
        background-color: #ddd;
    }

    td:first-child,
    th:first-child {
        text-align: left;
    }

    td:nth-child(1),
    td:nth-child(2) {
        text-align: left;
    }
</style>
