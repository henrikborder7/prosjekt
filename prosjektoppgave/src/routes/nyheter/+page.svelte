<script>
    import fil from "../postnummer (1).json";

    let json = JSON.parse(JSON.stringify(fil));
    let postnummer = "";
    let fornavn = "";
    let Etternavn = "";
    let digital =false
    let fysisk =false
    function hentPoststed(postnummer) {
        for (let i = 0; i < json.length; i++) {
            if (json[i].er === postnummer) {
                return json[i].Poststed;
            }
        }
        return "";
    }
</script>

<h1>Meld deg på vårt nyhetsbrev</h1>
<form>
    <h3>Fornavn</h3>

    <input placeholder="Fornavn" bind:value={fornavn} required />

    <h3>Etternavn</h3>

    <input placeholder="Etternavn" bind:value={Etternavn} required />

    <h3>Type utgave?</h3>
    <label></label><input type="checkbox" placeholder="Kun digital" bind:checked={digital}/>
    <input type="checkbox" placeholder="Digital og fysisk" bind:checked={fysisk}/>

    {#if fysisk==true }
    {#if fil}
        <h3>Postnummer</h3>
        <input placeholder="Postnummer" bind:value={postnummer} required />
        <h3>Poststed</h3>
        {hentPoststed(postnummer)}
    {/if}
    {/if}
    <button>Meld på</button>
</form>
