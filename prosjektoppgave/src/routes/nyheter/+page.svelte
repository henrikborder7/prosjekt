<script>
    import fil from "../postnummer (1).json"
  
    let json = JSON.parse(JSON.stringify(fil))
    let postnummer = ""
  
    function hentPoststed(postnummer) {
      for (let i = 0; i < json.length; i++) {
        if (json[i].er === postnummer){
          return json[i].Poststed
        }
        
      }
      return ""
    }
  </script>
  
  <h1>Meld deg på vårt nyhetsbrev</h1>
  <form>
    {#if fil}
      <h3>Postnummer</h3>
      <input placeholder="Postnummer" bind:value={postnummer} required/>
      <h3>Poststed</h3>
      {hentPoststed(postnummer)}
    {/if}
    <button>Meld på</button>
  </form>