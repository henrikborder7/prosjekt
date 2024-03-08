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

    let selectedPlayerInfo = null;

    function visSpillerInfo(spiller) {
        // Find the player's information based on the selected player's name
        selectedPlayerInfo = json2.elements.find(
            (player) =>
                player.first_name + " " + player.second_name === spiller,
        );
    }

    onMount(() => {
        function findTeamName(event) {
            const teamName = event.target
                .closest("tr")
                .querySelector("td:nth-child(2)").textContent;
            return teamName.trim(); // Remove leading and trailing white spaces
        }

        document
            .querySelector("#tabell-venstre")
            .addEventListener("click", (event) => {
                lag = findTeamName(event);
                visSpillere = true;

                hentID();
                hentSpillere();
            });

            function hentID() {
            const team = json2.teams.find(
                (team) => team.name.trim().toLowerCase() === lag.toLowerCase(),
            );
            id = team ? team.id : null;
            console.log(id);
        }

        function hentSpillere() {
            spillere = [];
            if (id !== null) {
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
        }
    });

    function gjorFalse() {
        visSpillere = false;
        selectedPlayerInfo = null; // Reset selected player information when hiding the player list
    }

    let visSpillere2 = true;
</script>

<body>
    <Navigasjon />

    <div class="hovedcontainer">
        {#if visSpillere2}
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
                    <tbody class="tabell" style="cursor:pointer">
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
                <p><span id="CL"> ....</span> - Champions League</p>
                <div id="EL"></div>
                <p>- Europa League</p>
                <div id="CL2"></div>
                <p>- Confrence League</p>
                <div id="nedrykk"></div>
                <p>- Nedrykk</p>
            </div>
        {/if}
        {#if !visSpillere2}{/if}
        {#if visSpillere}
            <div class="vis-spillere">
                <h1>Spillerstall</h1>

                <div>
                    <button on:click={gjorFalse}>Skjul</button>
                </div>
                <ul class="spillerStall">
                    {#each spillere as spiller}
                        <li
                            on:click={() => visSpillerInfo(spiller)}
                            style="cursor:pointer"
                        >
                            {spiller}
                        </li>
                    {/each}
                </ul>
            </div>
        {/if}

        {#if selectedPlayerInfo !== null}
            <div>
                <button on:click={gjorFalse}>Skjul</button>
            </div>
            <div class="selected-player-info">
                <h2>
                    {selectedPlayerInfo.first_name}
                    {selectedPlayerInfo.second_name}
                </h2>
                <img
                    src={`https://resources.premierleague.com/premierleague/photos/players/110x140/p${selectedPlayerInfo.code}.png`}
                    alt="Spillerbilde mangler :("
                />
                <!-- Display other player information as needed -->
                <p>Lag: {selectedPlayerInfo.team}</p>
                <p>Posisjon: {selectedPlayerInfo.element_type}</p>
                {#if selectedPlayerInfo.news !== ""}
                    <p>Status: {selectedPlayerInfo.news}</p>
                {/if}

                <p>Antall mål: {selectedPlayerInfo.goals_scored}</p>
                <p>Antall assist: {selectedPlayerInfo.assist}</p>
                {#if selectedPlayerInfo.element_type == 1 || selectedPlayerInfo.element_type == 2}
                    <p>Rent bur:{selectedPlayerInfo.clean_sheets}</p>
                {/if}
                {#if selectedPlayerInfo.element_type == 1}
                    <p>Straffer reddet: {selectedPlayerInfo.penalties_saved}</p>
                {/if}
                {#if selectedPlayerInfo.element_type == 4}
                    <p>
                        Forventa mål per 90 min: {selectedPlayerInfo.expected_goals_per_90}
                    </p>
                {/if}
                <!-- Add more details based on your JSON structure -->
                <!-- ... -->
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
                    <tbody style="cursor:pointer">
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
</body>

<style>
    body {
        margin: 0;
    }
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
    .vis-spillere {
        display: grid;
        grid-template-columns: auto auto;
    }
    .spillerStall {
        grid-column: -1/1;
    }
    .vis-spillere div {
        display: flex;
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
    .tabell tr:nth-child(1),
    .tabell tr:nth-child(2),
    .tabell tr:nth-child(3),
    .tabell tr:nth-child(4) {
        border-left: 2px solid blue; /* Blue border for the first 4 teams */
    }

    .tabell tr:nth-child(5),
    .tabell tr:nth-child(6) {
        border-left: 2px solid rgb(245, 164, 34); /* Orange border for the next 2 teams */
    }

    .tabell tr:nth-child(7) {
        border-left: 2px solid rgb(17, 231, 17); /* Green border for the next team */
    }
    .tabell tr:nth-child(17) {
        border-bottom: 2px solid rgb(55, 55, 55); /* Green border for the next team */
    }

    .tabell tr:nth-child(18),
    .tabell tr:nth-child(19),
    .tabell tr:nth-child(20) {
        border-left: 2px solid red; /* Red border for the last 3 teams */
    }

    @media screen and (max-width: 1050px) {
        .hovedcontainer {
            grid-template-columns: 1fr;
        }
    }
    #CL {
        background-color: blue;
        width: 10px;
        height: 10px;
        color: blue;
        border-radius: 2px;
    }
</style>
