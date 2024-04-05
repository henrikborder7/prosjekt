<script>
    import Navigasjonsbar from "../navigasjon/navigasjonsbar.svelte";
    import { tick } from "svelte";
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
    let sum = 0;
    function hentSpillere() {
        let niSpillere = [];
        sum = 0;
        for (let i = 0; i < tilfeldigeSpillere.length; i++) {
            for (let j = 0; j < json.elements.length; j++) {
                if (json.elements[j].id === tilfeldigeSpillere[i]) {
                    let navn =
                        json.elements[j].first_name +
                        " " +
                        json.elements[j].second_name;
                    let maal = json.elements[j].goals_scored;
                    let assist = json.elements[j].assists;
                    let maalPoeng = maal + assist;
                    let bilde = json.elements[j].code;

                    niSpillere.push({
                        navn: navn,
                        mål: maal,
                        assist: assist,
                        målPoeng: maalPoeng,
                        bilde: bilde,
                    });
                    sum = sum + maalPoeng;
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
        return b.målPoeng + b.mål * 0.1 - (a.målPoeng + b.mål * 0.1);
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
    function hoppOver() {
        visDiv = 3;
    }
</script>

<body>
    <Navigasjonsbar />
    {#if visDiv === 1}
        <div
            id="div1"
            on:click={visSpiller1}
            on:keypress={visSpiller1}
            style="cursor: pointer;"
        ></div>
    {/if}

    {#if visDiv === 2}
        <div id="div2" on:click={visAlleSpillere} style="cursor: pointer;">
            {#if sorterteSpillere[nåværendeSpillerIndex]}
                    <p id="stor_skrift_div2">
                        {sorterteSpillere[nåværendeSpillerIndex].målPoeng}
                    </p>
                <img
                    src={`https://resources.premierleague.com/premierleague/photos/players/110x140/p${sorterteSpillere[nåværendeSpillerIndex].bilde}.png`}
                    alt="Spillerbilde mangler :("
                />
                <p id="navn">{sorterteSpillere[nåværendeSpillerIndex].navn}</p>
                <div id="assist_og_maal">
                    <div id="maal">
                        <p>Mål</p>
                        <p>{sorterteSpillere[nåværendeSpillerIndex].mål}</p>
                    </div>
                    <div id="assist">
                        <p>Assist</p>
                    <p>{sorterteSpillere[nåværendeSpillerIndex].assist}</p>
                    </div>

                </div>
                <button on:click={hoppOver}>Hopp over</button>
            {/if}
        </div>  
    {/if}
    {#if visDiv === 3}
        <div id="div3" on:click={visSpiller1} style="cursor: pointer;">
            <p id="hvit_tekst">Totale målpoeng: {sum}</p>
            {#each sorterteSpillere as spiller}
                <div id="kort">
                    <div id="test">
                        <p id="stor_skrift_div3"><b>{spiller.målPoeng}</b></p>
                        <p>{spiller.mål}</p>
                        <p>{spiller.assist}</p>
                    </div>
                    <img
                        src={`https://resources.premierleague.com/premierleague/photos/players/110x140/p${spiller.bilde}.png`}
                        alt="Spillerbilde mangler :("
                    />

                    <p>{spiller.navn}</p>
                </div>
            {/each}
        </div>
    {/if}
</body>

<style>
    body {
        margin: 0;
    }
    #hvit_tekst {
        color: white;
        justify-self: center;
    }
    #navn {
        justify-self: center;
        margin: 0;
    }
    #kort {
        display: grid;
        grid-template-rows: auto auto auto auto auto auto;
        padding: 5px;
        text-align: center;
        border-radius: 5px;
        background-image: url("https://cardsplug.com/cdn/shop/products/S23ShinyGoldBlank_13658bf6-feff-4b5e-b72f-3b942d911e1d.png?v=1663878408");
        background-repeat: no-repeat;
        background-size: contain;
    }

    #kort img {
        object-fit: cover;
        margin: 0 auto;
        width: auto;
        height: 110px;
        padding-top: 5px;
    }

    #test {
        margin: 0;
        padding: 0;
        position: absolute;
    }

    #div1 {
        width: 400px;
        height: 625px;
        background-image: url("https://cdn.futwiz.com/assets/img/packs/fifa22-pack.png?r=4");
        background-repeat: no-repeat;
        background-size: contain;
    }

    #div2 {
        width: 400px;
        height: 700px;
        background-color: blue;
        display: grid;
        background-image: url("https://pdf-service-static.s3.amazonaws.com/static/layout-images/cardstar/thumbnails/rare-gold-24.png");
        background-repeat: no-repeat;
        background-size: contain;
        gap: 0;
        grid-template-columns: repeat(50, 1fr);
  grid-template-rows: repeat(50, 1fr);
        
    }
    #div2 img {
        object-fit: cover;
        margin: 0 auto;
        width: 240px;
        padding-top: 50px;
        grid-column: span 45;
        border: 2px solid red;
    }
    #stor_skrift_div2 {       
        font-size: 50px;
        grid-column: span 5;
        grid-row: span 1;
        border: 2px solid red;
    }
    #navn{
    grid-column: span 50;
    grid-row: span 1;
    border: 2px solid red;
    }
    #assist_og_maal{
        display: flex;
        gap: 50px;
        grid-column: span 50;
    grid-row: span 25;
    border: 2px solid red;
    }
    #assist{
        display: flex;
        gap: 5px;
        flex-direction: column;
    }
    #maal{
        display: flex;
        gap: 5px;
        flex-direction: column;
    }
    button{
        grid-column: span 50;
    }
    #div3 {
        width: 400px;
        height: 625px;
        background-color: green;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 5px;
        padding: 5px;
    }
    #div3 p {
        grid-column: -1/1;
    }
</style>
