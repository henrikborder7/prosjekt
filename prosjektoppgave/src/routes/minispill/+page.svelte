<script>
    import Navigasjonsbar from "../navigasjon/navigasjonsbar.svelte";
    import fil1 from "../pl.json";
    let json = JSON.parse(JSON.stringify(fil1));

    let visDiv1 = true; // Variabel for å styre synligheten til div1 og div2
    let visDiv2 = false;
    let visDiv3 = false;

    function visSpiller1() {
        visDiv1 = false;
        visDiv2 = true;
    }
    function visAlleSpillere() {
        visDiv2 = false;
        visDiv3 = true;
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

    let tilfeldigeSpillere = genererTilfeldigeTall();
    console.log(tilfeldigeSpillere);

    function hentSpillere() {
        let niSpillere = [];
        console.log("Nå" + tilfeldigeSpillere);
        for (let i = 0; i < tilfeldigeSpillere.length; i++) {
            console.log(tilfeldigeSpillere[i]);
            for (let j = 0; j < json.elements.length; j++) {
                if (json.elements[j].id === tilfeldigeSpillere[i]) {
                    let navn =
                        json.elements[j].first_name +
                        " " +
                        json.elements[j].second_name;
                    let maal = json.elements[j].goals_scored;
                    niSpillere.push(navn+maal);
  
                }
            }
            
        }

        return niSpillere;
    }
    let test = hentSpillere();
    console.log("test  " + test);
</script>

<body>
    <Navigasjonsbar />
    {test}
    {#if visDiv1}
        <div id="div1" on:click={visSpiller1}>Div 1</div>
    {/if}
    {#if visDiv2}
        <div id="div2" on:click={visAlleSpillere}>Div 2</div>
    {/if}
    {#if visDiv3}
        <div id="div3" on:click={visAlleSpillere}>Div 3</div>
    {/if}
</body>

<style>
    #div1 {
        width: 300px;
        height: 300px;
        background-color: red;
    }
    #div2 {
        width: 300px;
        height: 300px;
        background-color: blue;
    }
    #div3 {
        width: 300px;
        height: 300px;
        background-color: green;
    }
</style>
