<script>
    import fil from "../postnummer (1).json";

    let json = JSON.parse(JSON.stringify(fil));
    let postnummer = "";
    let navn = "";
   
    let adresse="";
    let email="";
    let digital =false
    let fysisk =false
    let showWarning=true
    function hentPoststed(postnummer) {
        for (let i = 0; i < json.length; i++) {
            if (json[i].er === postnummer) {
                return json[i].Poststed;
            }
        }
        return "";
    }
</script>
<body>
    <nav>
        <a href="/">Hjem</a>
        <a href="tabell">Tabell</a>
        <a href="kamper">Kamper</a>
        <a href="nyheter">Nyhetsbrev </a>
        <a href="butikk">Butikk</a>
    </nav>
    
    <h1>Meld deg på vårt nyhetsbrev</h1>
    
    <form on:submit|preventDefault={() => showWarning = !(digital || fysisk)}>
        <h3>Navn</h3>
        <input placeholder="Navn" bind:value={navn} required />

        <h3>E-post</h3>
        <input type="email" placeholder="E-post" bind:value={email} required />

        <h3>Type utgave?</h3>
        <label>Digital utgave<input type="checkbox" bind:checked={digital}/></label><br>
        <label>Fysisk utgave<input type="checkbox" bind:checked={fysisk}/></label><br>


        {#if fysisk==true }
            <h3>Adresse</h3>
            <input placeholder="Adresse" bind:value={adresse} required />

            <h3>Postnummer</h3>
            <input placeholder="Postnummer" bind:value={postnummer} required />

            <h3>Poststed</h3>
            {hentPoststed(postnummer)}
        {/if}

        <button type="submit" disabled={!((digital || fysisk) && navn && email)}>Meld på</button>

    </form>
</body>
<style>
    body {
        font-family: Arial, sans-serif;
        background-color: #f4f4f4;
        margin: 0;
        padding: 0;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        min-height: 100vh;
    }

    nav {
        background-color: #333;
        padding: 10px;
        color: white;
        text-align: center;
        margin-bottom: 20px;
    }

    nav a {
        color: white;
        margin: 0 10px;
        text-decoration: none;
    }

    form {
        background-color: #fff;
        border-radius: 8px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        padding: 20px;
        width: 300px;
    }

    h1 {
        margin-bottom: 20px;
    }

    h3 {
        margin-top: 10px;
    }

    input {
        width: 100%;
        padding: 8px;
        margin: 5px 0;
        box-sizing: border-box;
        border: 1px solid #ccc;
        border-radius: 4px;
    }

    label {
        display: block;
        margin-top: 10px;
    }

    button {
        background-color: #4caf50;
        color: #fff;
        padding: 10px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        width: 100%;
        margin-top: 10px;
    }

    button:hover {
        background-color: #45a049;
    }
</style>