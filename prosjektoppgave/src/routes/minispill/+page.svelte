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
        while (tilfeldigeTall.length < 9) {
            let tall = Math.floor(Math.random() * 788) + 1;
            console.log("tall "+tall)
            if (!tilfeldigeTall.includes(tall)) {
                tilfeldigeTall.push(tall);
            }

        }
        console.log("tilfeldigetall "+tilfeldigeTall)
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
                    let navnet = Array.from(navn);
                    if (navnet.length > 20) {
                        navn = json.elements[j].web_name;
                    }
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
    <div id="hoved_div">
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
                    <div id="stor_skrift_div2">
                        <p>
                            {sorterteSpillere[nåværendeSpillerIndex].målPoeng}
                        </p>

                        <img
                            src={`https://resources.premierleague.com/premierleague/photos/players/110x140/p${sorterteSpillere[nåværendeSpillerIndex].bilde}.png`}
                            alt="Spillerbilde mangler :("
                        />
                    </div>

                    <p id="navn">
                        {sorterteSpillere[nåværendeSpillerIndex].navn}
                    </p>
                    <div id="assist_og_maal">
                        <div id="maal">
                            <p>Mål</p>
                            <p>{sorterteSpillere[nåværendeSpillerIndex].mål}</p>
                        </div>
                        <div id="assist">
                            <p>Assist</p>
                            <p>
                                {sorterteSpillere[nåværendeSpillerIndex].assist}
                            </p>
                        </div>
                    </div>
                    <div id="knapp">
                        <button on:click={hoppOver}>Hopp over</button>
                    </div>
                {/if}
            </div>
        {/if}
        {#if visDiv === 3}
            <div id="div3" on:click={visSpiller1} style="cursor: pointer;">
                <p id="totale_maalpoeng">Totale målpoeng: {sum}</p>
                <div id="korter">
                    {#each sorterteSpillere as spiller}
                        <div id="kort">
                            <p id="stor_skrift_div3">
                                <b>{spiller.målPoeng}</b>
                            </p>
                            <img
                                src={`https://resources.premierleague.com/premierleague/photos/players/110x140/p${spiller.bilde}.png`}
                                alt="Spillerbilde mangler :("
                            />
                            <div id="maal_og_assist">
                                <div id="maal">
                                    <p>Mål:</p> <br>
                                    <p>{spiller.mål}</p>
                                </div>
                                <div id="assist">
                                    <p>Assist:</p> <br>
                                    <p>{spiller.assist}</p>
                                </div>
                            </div>
                        </div>
                    {/each}
                </div>
            </div>
        {/if}
    </div>
</body>

<style>
    #hoved_div {
        display: flex;
        justify-content: center;
    }
    body {
        margin: 0;
    }

    #navn {
        justify-self: center;
        margin: 0;
    }
    #kort {
        display: flex;
        flex-direction: column;
        padding: 5px;
        border-radius: 5px;
        background-image: url("https://cardsplug.com/cdn/shop/products/S23ShinyGoldBlank_13658bf6-feff-4b5e-b72f-3b942d911e1d.png?v=1663878408");
        background-repeat: no-repeat;
        background-size: contain;
    }

    #kort img {
        object-fit: cover;
        margin: 0 auto;
        width: auto;
        height: 91px;
        padding-top: 5px;
    }

    #stor_skrift_div3 {
        position: absolute;
        border: 2px solid yellowgreen;
        padding: 5px 5px 0 0;
    }

    #div1 {
        width: 400px;
        height: 600px;
        background-image: url("https://www.fifplay.com/img/fifa/22/packs/gold-pack.png");
        background-repeat: no-repeat;
        background-size: contain;
        margin: 10px 0 0 0;
    }

    #div2 {
        background-image: url("https://pdf-service-static.s3.amazonaws.com/static/layout-images/cardstar/thumbnails/rare-gold-24.png");
        background-repeat: no-repeat;
        background-size: contain;
        width: 400px;
        display: flex;
        flex-direction: column;
        gap: 5px;
        height: 600px;
    }
    #stor_skrift_div2 {
        font-size: 50px;
        display: flex;
        justify-content: center;
        padding: 10px 0 0 0;
    }
    #stor_skrift_div2 p {
        padding: 20px 0 0 10px;
        width: 50px;
    }

    #div2 img {
        width: 240px;
        min-height: 302px;
        padding-top: 40px;
    }
    #navn {
        grid-column: 1; /* Fyller første kolonne */
        display: flex;
        justify-content: center;
        padding-top: 5px;
        font-size: 30px;
    }
    #assist_og_maal {
        display: flex;
        justify-content: center;
        gap: 40px;
    }
    #assist {
        text-align: right;
    }
    #maal {
        text-align: right;
    }
    #knapp {
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: flex-end;
    }
    #knapp button {
        height: 20px;
    }
    #div3 {
        width: 400px;
        height: 625px;
        text-align: center;
    }
    #korter {
        gap: 5px;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        padding: 5px;
    }
    #korter #kort {
        display: flex;
        width: 100%;
        height: 175px;
        flex-direction: column;
        border: 2px solid red;
    }
    #korter #maal_og_assist{
        border: 2px solid blue;
        display: flex;
        gap: 0;
        font-size: 12px;
    }
 
    #korter #assist {
        padding: 0 0 0 12px;
        margin: 0;
        width: 50%;
        justify-content: start;
        align-items: end;
    }
    #korter #maal {
        display: flex;
        justify-content: end;
        align-items: start;
        padding: 0 0 0 5px;
        width: 50%;
    }
    #korter img {
        height: 100px;
    }
    #stor_skrift_div3 {
        position: absolute;
        margin: 18px 0 0 18px;
    }
    #div3 p {
        grid-column: -1/1;
    }
</style>
