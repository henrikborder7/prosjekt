<script> import Navigasjon from "./navigasjon/navigasjonsbar.svelte"</script>

<body>
  <Navigasjon/>
  <div>Powered by U&N tec.</div>
  <section class="nyheter">
    <a href="/nyheter" class="seks-kolonner to-rader">
      <img
        src="https://premium.vgc.no/v2/images/c61cf5f9-a1a0-46ba-b7e8-9ae57ec51426?fit=crop&format=auto&h=1493&w=2048&s=69c1156053b8460e092b72241fb55b336ffb1411"
        alt="Bilde av Diogo Jota på båre"
      />
      <h1>Skadetrøbbel i Liverpool</h1>
    </a>
    <a href="/nyheter" class="tre-kolonner">
      <img
        src="https://2.bp.blogspot.com/-6F8lBvGEHt8/Vw3-ElpI1CI/AAAAAAAABP0/jjILGgFmvGUygR-IhXT4weYAvgc90JP7ACK4B/s910-r/IMG_6979%2B%25E2%2580%2593%2BKopi1.jpg" alt="Henrik Nese" style="object-position: top right; zoom"
      />
      <h1>Norsk gutt på prøvespilling for PL-klubb</h1>
    </a>
    <a href="/nyheter" class="tre-kolonner">
      <img
        src="https://m.media-amazon.com/images/S/pv-target-images/83181937ea9b54661056c689c705e493a51f5ed5f69c275b1c459faf243ae5be.jpg"
        style="object-position: bottom;"
      />
      <h1>Se trekkningen kl 12:00</h1>
    </a>
    <a href="/nyheter" class="tre-kolonner">
      <img
        src="https://www.radiosporten.no/wp-content/uploads/2023/06/erik-ten-hag.jpg" alt="ten hag"
      />
      <h1>Ten Hag klar for 2 nye år i Man. United</h1>
    </a>
    <a href="/nyheter" class="tre-kolonner">
      <img
        src="https://www.thesun.co.uk/wp-content/uploads/2023/11/2023-liverpools-darwin-nunez-celebrates-856578572.jpg"
        style="object-position: bottom;"
      />
      <h1>Núñez med ny rekord!</h1>
    </a>
    <a href="/nyheter" class="fire-kolonner">
      <img
        src="https://mf.b37mrtl.ru/files/2021.07/xxl/60fd668e203027683b239632.jpg"
      />
      <h1>City anklaget for 114(!) finansielle brudd</h1>
    </a>
    <a href="/nyheter" class="fire-kolonner">
      <img
        src="https://static.independent.co.uk/2023/09/30/16/SEI174001450.jpg" alt="Everton og Luton i kamp" style="object-position: top;"
       
      />
      <h1>Sjekk hva KI tror ligaresultatet blir!</h1>
    </a>
    <a href="/nyheter" class="fire-kolonner">
      <img
        src="https://static.independent.co.uk/2023/02/14/08/08162150-a5596332-3fc1-437a-8753-6e5852e158e8.jpg" alt="ødegaard og haaland"      style="object-position: top;"
      />
      <h1>Snart spiller de på samme lag!</h1>
    </a>
  </section>
</body>

<style>
  body {
    margin: 0;
  }
  
  a {
    display: inline-block;
    position: relative;
    color: #0087ca;
  }

  a::after {
    content: "";
    position: absolute;
    width: 100%;
    transform: scaleX(0);
    height: 2px;
    bottom: 0;
    left: 0;
    background-color: #0083ca;
    transform-origin: bottom right;
    transition: transform 0.25s ease-out;
  }

  a:hover::after {
    transform: scaleX(1);
    transform-origin: bottom left;
  }

  a {
    text-decoration: none;
    display: flex;
    justify-content: center;
    align-items: center;
    color: rgb(61, 25, 91);
    margin: 16px 0px 16px 24px;
    font-family: Helvetica, sans-serif;
  }
 
  ::placeholder {
    color: rgb(255, 255, 255);
  }

  .nyheter {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    max-width: 70vw;
    grid-auto-rows: minmax(12rem, 12rem);
    gap: 0.7rem;
    margin-left: 5vw;
    background-color: rgb(61, 25, 91);
    border: rgb(212, 212, 212) solid 3vh;
  }

  .nyheter a {
    background-color: #eee;
    display: flex;
    align-items: flex-end;
    padding-left: 1rem;
    padding-bottom: 1rem;
    position: relative;
    font-family: sans-serif;
  }

  .nyheter h1 {
    position: absolute;
    margin: 0;
    color: white;
    background: rgba(0, 0, 0, 0.562);
  }

  .nyheter img {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
  }

  .tre-kolonner {
    grid-column: span 3;
    font-size: small;
  }
  .fire-kolonner {
    grid-column: span 4;
  }
  .seks-kolonner {
    grid-column: span 6;
    font-size: larger;
  }
  .to-rader {
    grid-row: span 2;
    
  }

  div {
    margin: 0px 0px 25px 0px;
    background-color: #eee;
    padding-right: 20px;
  }
</style>
