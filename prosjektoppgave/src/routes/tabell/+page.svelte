<script>
    import { onMount } from "svelte";
    import Navigasjon from "../navigasjon/navigasjonsbar.svelte";
    import fil from "../plLag.json";
    let json = JSON.parse(JSON.stringify(fil));
    import fil2 from "../pl.json";
    let json2 = JSON.parse(JSON.stringify(fil2));
    let topSkaarere = [];
    let lag = "";
    let id = "";
    $: spillere = [];
    let visSpillere = false;
    let spillereKopi = [];

    function sorterSpillere(spillere) {
        const spillereKopi = [...spillere];
        return spillereKopi.sort((a, b) => b.goals_scored - a.goals_scored);
    }

    topSkaarere = sorterSpillere(json2.elements).slice(0, 20);

    onMount(() => {
        function findTeamName(event) {
            const teamName = event.target
                .closest("tr")
                .querySelector("td:nth-child(2)").textContent;
            return teamName;
        }

        // Legg til en eventlistener som lytter etter klikk på tabellen til venstre
        document
            .querySelector("#tabell-venstre")
            .addEventListener("click", (event) => {
                lag = findTeamName(event);
                console.log(lag); // Her kan du gjøre hva du vil med navnet på laget
                visSpillere = true;

                // Når lag variabelen er satt, kall hentID og hentSpillere
                hentID();
                hentSpillere();
            });

        function hentID() {
            for (let i = 0; i < json2.teams.length; i++) {
                if (json2.teams[i].name === lag) {
                    id = json2.teams[i].id;
                    console.log(id);
                }
            }
        }

        function hentSpillere() {
            spillere = [];
            for (let i = 0; i < json2.elements.length; i++) {
                if (json2.elements[i].team == id) {
                    spillere.push(
                        json2.elements[i].first_name +
                            " " +
                            json2.elements[i].second_name,
                    );
                }
            }
        }
    });

    function gjorFalse() {
        visSpillere = false;
    }
</script>

<Navigasjon />

<div class="hovedcontainer">
    <div class="tabell-container">
        <h1>Premier League Tabell 2023/24</h1>
        <!-- Legg til en identifikator til tabellen til venstre -->
        <table id="tabell-venstre">
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
    {#if visSpillere}
        <div class="vis-spillere">
            <h1>Spillerstall</h1>
            <div>
            <button on:click={gjorFalse}>Sjul</button>
        </div>
            <ul class="spillerStall">
                {#each spillere as spiller}
                    <li>{spiller}</li>
                {/each}
            </ul>
            
        </div>
    {/if}
    {#if !visSpillere}
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
        </div>{/if}
</div>

<style>
    .hovedcontainer {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
        gap: 20px;
        padding: 0 20px;
    }

    .tabell-container,
    .vis-spillere,
    .top-scorers {
        background-color: #fff;
        border: 1px solid #ccc;
        border-radius: 5px;
        padding: 0 20px 0 20px;
        margin-top: 20px;
    }
    .vis-spillere{
        display: grid;
        grid-template-columns: auto auto;
    }   
    .spillerStall{
        grid-column: -1/1;

    }
    .vis-spillere div{
        display:flex;
        justify-content: end;
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

    .top-scorers th,
    .top-scorers td {
        text-align: left;
    }

    /* Legg til stil for delen som vises når visSpillere = true */
    .vis-spillere ul {
        padding-left: 0;
        list-style-type: none; /* Fjern punktene på ul */
    }

    .vis-spillere li {
        margin-bottom: 10px;
        border-bottom: 1px solid #ccc;
        padding-bottom: 10px;
    }

    .vis-spillere button {
        display: block;
        margin-top: 20px;
        padding: 10px 20px;
        background-color: #f2f2f2;
        border: 1px solid black;
        border-radius: 5px;
        cursor: pointer;
        max-height: 40px;

    }

    .vis-spillere button:hover {
        background-color: #ddd;
    }

    @media screen and (max-width: 1050px) {
        .hovedcontainer {
            grid-template-columns: 1fr;
        }
    }
</style>
