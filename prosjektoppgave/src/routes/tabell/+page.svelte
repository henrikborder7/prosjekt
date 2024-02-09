<script>
    import Navigasjon from "../navigasjon/navigasjonsbar.svelte";
    import fil from "../plLag.json";
    let json = JSON.parse(JSON.stringify(fil));
</script>

<Navigasjon />

<h1>Premier League Tabell 2023/24</h1>

<a href="../tabell/spillere"><div>Spillere</div></a>

<table>
    <tr>
        <th colspan="2" style="text-align:left"> Premier league 23/24 </th>
        <th>S</th>
        <th>V</th>
        <th>U</th>
        <th>T</th>
        <th>Mål</th>
        <th> MF </th>
        <th>P</th>
    </tr>

    {#each json.teams as test}
        <tr>
            <td>
                {test.position}
            </td>
            <td>
                {test.name}
            </td>
            <td>
                {test.played}
            </td>
            <td>
                {test.wins}
            </td>
            <td>
                {test.draws}
            </td>
            <td>
                {test.losses}
            </td>
            <td>
                {test.goals_for} - {test.goals_against}
            </td>
            <td>
                {test.goals_for - test.goals_against}
            </td>
            <td>
                {test.points}
            </td>
        </tr>
    {/each}
</table>

<style>
    table {
        border-collapse: collapse;
    }
     th td {
        border: 2px solid black;
    }
</style>
