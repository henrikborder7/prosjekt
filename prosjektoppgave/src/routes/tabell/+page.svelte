<script>
    import fil from "../pl.json"
    let json = JSON.parse(JSON.stringify(fil));

    let navn = "";
    let navner = []


    function sokEtterSpiller(navn) {
        for (let i = 0; i < json.elements.length; i++) {
            if (json.elements[i].first_name === navn) {
                return json.elements[i].first_name+" "+ json.elements[i].second_name;
            }
        }
        return "";
    }

    json.elements.sort(function(a,b){
        return b.goals_scored - a.goals_scored
    });

    let rangertListe = json.elements.map(function(spiller){
        return spiller.first_name +" "+spiller.second_name+ ": "+ spiller.goals_scored +" mål"
    }) 


    
    function skEtterSpiller(fornavn){let matchendeSpillere= []

    for (let i = 0; i < json.elements.length; i++) {
        if (json.elements[i].first_name === fornavn) {
        matchendeSpillere.push(json.elements[i].first_name + " " + json.elements[i].second_name);
    }
  }
visSpillere(matchendeSpillere)
    }

    function visSpillere(spillere) {
  if (spillere.length === 0) {
    return "Ingen spillere funnet.";
  } else {
    for (let i = 0; i < spillere.length; i++) {
      return spillere[i]
    }
  }
    }

</script>

<label><input bind:value={navn}>Antal mål
</label>

<p>{navner}</p>

<p>{sokEtterSpiller(navn)}</p>
<p>{visSpillere(navn)}</p>

<p>{json.elements[0].first_name}</p>

<ul>

    {#each rangertListe as liste}
    <li>
        {liste}
    </li>
    {/each}
</ul>