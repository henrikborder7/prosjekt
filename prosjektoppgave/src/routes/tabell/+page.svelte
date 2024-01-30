<script>
    import fil from "../pl.json"
    let json = JSON.parse(JSON.stringify(fil));

    let navn = "";


    function sokEtterSpiller(navn) {
        for (let i = 0; i < json.elements.length; i++) {
            if (json.elements[i].first_name === navn) {
                return json.elements[i].first_name+" "+ json.elements[i].second_name;
            }
        }
        return "";
    }

    function skEtterSpiller(navn) {
        let i = 0
        while(i < json.elements.length) {
            if (json.elements[i].first_name === navn) {
                return json.elements[i].first_name+" "+ json.elements[i].second_name;
            }
            i = i++
        }
        return "";
    }

    json.elements.sort(function(a,b){
        return b.goals_scored - a.goals_scored
    });

    let rangertListe = json.elements.map(function(spiller){
        return spiller.first_name +" "+spiller.second_name+ ": "+ spiller.goals_scored +" mål"
    }) 
    console.log(rangertListe)
</script>

<label><input bind:value={navn}>Antal mål
</label>

<p>{sokEtterSpiller(navn)}</p>
<p>{skEtterSpiller(navn)}</p>
<p>{json.elements[0].first_name}</p>

<ul>

    {#each rangertListe as liste}
    <li>
        {liste}
    </li>
    {/each}
</ul>