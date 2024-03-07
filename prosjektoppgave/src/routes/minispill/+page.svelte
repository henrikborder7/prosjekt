<script>
    import Navigasjonsbar from "../navigasjon/navigasjonsbar.svelte";
    import { tick } from 'svelte';
    import fil1 from "../pl.json";

    let json = JSON.parse(JSON.stringify(fil1));

    let visDiv = 1; // Variabel for å styre hvilken div som skal vises
    let alleSpillereVist = false;
    let nåværendeSpillerIndex = 0;
    let tilfeldigeSpillere = [];

    async function visSpiller1() {
    if (visDiv === 1) {
        visDiv = 2;
        genererTilfeldigeSpillere();
        hentSpillereOgVisDiv1();
    } else {
        visDiv = 1;
        alleSpillereVist = false;
        nåværendeSpillerIndex = 0;
    }
}

    function genererTilfeldigeTall() {
        let tilfeldigeTall = [];
        for (let i = 0; i < 9; i++) {
            let tall = Math.floor(Math.random() * 788) + 1;
            if (!tilfeldigeTall.includes(tall)) {
                tilfeldigeTall.push(tall);
            }
        }
        return tilfeldigeTall;
    }

    function genererTilfeldigeSpillere() {
        tilfeldigeSpillere = genererTilfeldigeTall();
    }
    let test = [];
    let sorterteSpillere = [];
    let sum = 0
    function hentSpillere() {
        let niSpillere = [];
        sum = 0
        for (let i = 0; i < tilfeldigeSpillere.length; i++) {
            for (let j = 0; j < json.elements.length; j++) {
                if (json.elements[j].id === tilfeldigeSpillere[i]) {
                    let navn = json.elements[j].first_name + " " + json.elements[j].second_name;
                    let maal = json.elements[j].goals_scored;
                    let assist = json.elements[j].assists;
                    let maalPoeng = maal + assist;
                    let bilde = json.elements[j].code;
  
                    niSpillere.push({navn: navn, mål: maal, assist: assist, målPoeng: maalPoeng, bilde: bilde});
                    sum = sum + maalPoeng 
                               }
            }
        }
        return niSpillere;
    }

    function hentSpillereOgVisDiv1() {
        test = hentSpillere();
        sorterteSpillere = test.slice().sort(sortereSpillereSynkende);
    }

    function sortereSpillereSynkende(a, b) {
        return b.målPoeng + (b.mål * 0.1) - (a.målPoeng + (b.mål * 0.1));
    }

    let besteSpiller = {};

    function visNesteSpiller() {
        nåværendeSpillerIndex++;
        if (nåværendeSpillerIndex >= sorterteSpillere.length) {
            alleSpillereVist = true;
        }
    }

    async function visAlleSpillere() {
        if (visDiv === 1) {
            visDiv = 2;
        } else if (visDiv === 2) {
            visNesteSpiller();
            if (nåværendeSpillerIndex >= sorterteSpillere.length) {
                visDiv = 3;
            }
        } else if (visDiv === 3) {
            genererTilfeldigeSpillere();
            hentSpillereOgVisDiv1();
            visDiv = 1;
        }

    }
    function hoppOver(){
        visDiv = 3;
    }
</script>

<body>
    <Navigasjonsbar />
    {#if visDiv === 1}
        <div id="div1" on:click={visSpiller1} on:keypress={visSpiller1}>Div 1</div> 
    {/if}

    {#if visDiv === 2}
        <div id="div2" on:click={visAlleSpillere}>
            {#if sorterteSpillere[nåværendeSpillerIndex]}
                <p>Målpoeng: {sorterteSpillere[nåværendeSpillerIndex].målPoeng} ({sorterteSpillere[nåværendeSpillerIndex].mål}/{sorterteSpillere[nåværendeSpillerIndex].assist}) </p>
                <p>Mål: {sorterteSpillere[nåværendeSpillerIndex].mål}</p>
                <p>Assist: {sorterteSpillere[nåværendeSpillerIndex].assist}</p>
                <img src={`https://resources.premierleague.com/premierleague/photos/players/110x140/p${sorterteSpillere[nåværendeSpillerIndex].bilde}.png`} alt="Spillerbilde mangler :(" >
                <p>{sorterteSpillere[nåværendeSpillerIndex].navn}</p> 
                <button on:click={hoppOver}>Hopp over</button>
            {/if}
        </div>
    {/if}
    {#if visDiv === 3}
        <div id="div3" on:click={visSpiller1}>
            <p>Totale målpoeng: {sum}</p>
            {#each sorterteSpillere as spiller}
                <div id="kort">  
                    <p>Målpoeng: {spiller.målPoeng} ({spiller.mål}/{spiller.assist}) </p>
                    <p>Mål: {spiller.mål}</p>
                    <p>Assist: {spiller.assist}</p>
                    <img src={`https://resources.premierleague.com/premierleague/photos/players/110x140/p${spiller.bilde}.png`} alt="Spillerbilde mangler :(" >
                    <p>{spiller.navn}</p> 
                </div>
            {/each}
            
        </div>

    {/if}
</body>

<style>
    #kort {
        background-color: gold;
        border-radius: 10px;
        padding: 20px;
        display: grid;
        grid-template-rows: auto auto auto auto auto auto;
        gap: 10px;
        text-align: center;
    }

    #kort img {
        
        object-fit: cover;
        margin: 0 auto;
        z-index: 0;
    }

    #kort p {
        margin: 0;
        z-index: 1;
    }

    #div1 {
        width: 1000px;
        height: 1000px;
        background-color: red;
    }

    #div2 {
        width: 1000px;
        height: 1000px;
        background-color: blue;
    }

    #div3 {
        width: 1000px;
        height: 1000px;
        background-color: green;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 20px;
        padding: 20px;
    }
    #div3 p{
        grid-column: -1/1;
    }
</style>
