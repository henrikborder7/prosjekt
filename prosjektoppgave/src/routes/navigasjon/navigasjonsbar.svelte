<section class="toppbar">
    <a href="/"><img
      src="https://soccerarmor.com/wp-content/uploads/2022/11/Premier_League_Logo.svg_.png"
      alt="Premier Leauge"
      height="50px"
      style="margin: 0.4rem"
    /></a>
    <a class="lenker" href="../tabell">Tabell</a>
    <a class="lenker" href="../butikk">Butikk</a>
    <a class="lenker" href="../nyheter">Nyheter</a>
    <a class="lenker" href="#">Kontakt</a>
    <a class="nyhetsknapp" href="../nyheter">Nyhetsbrev</a>
  </section>
<style>
    body {
    margin: 0;
  }
  .toppbar {
    background: linear-gradient(
      45deg,
      rgb(98, 255, 242),
      rgb(192, 242, 123),
      rgb(93, 242, 93),
      rgb(98, 255, 242)
    );
    display: flex;
    height: 70px;
    width: 100vw;

    z-index: 1000;
  }
  a {
    display: inline-block;
    position: relative;
    color: #0087ca;
  }

  .lenker::after {
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

  .lenker:hover::after {
    transform: scaleX(1);
    transform-origin: bottom left;
  }

  .lenker {
    text-decoration: none;
    display: flex;
    justify-content: center;
    align-items: center;
    color: rgb(61, 25, 91);
    margin: 16px 0px 16px 24px;
    font-family: Helvetica, sans-serif;
  }
  .nyhetsknapp {
    text-decoration: none;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    color: rgb(61, 25, 91);
    margin: 10px 0px 10px 30vw;
    font-family: Helvetica, sans-serif;
    border-radius: 4rem;
    border: none;
    padding: 0px 15px 0px 15px;
    background-color: rgb(242, 242, 242);
    cursor: pointer;
    transition: 1s;
  }

  .nyhetsknapp:hover {
    background-color: white;
    transform: scale(1.1);
    transition: 1s;
  }

 
</style>
