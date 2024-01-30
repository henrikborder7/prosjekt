<script>
    let cart = 0;
  
    function addToCart() {
      cart += 1;
      alert("Product added to cart!");
    }
  </script>
  
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f4;
      margin: 0;
      padding: 0;
    }
  
    header {
      background-color: #333;
      color: white;
      text-align: center;
      padding: 10px;
    }
  
    nav {
      background-color: #eee;
      padding: 10px;
    }
  
    nav a {
      margin: 0 15px;
      text-decoration: none;
      color: #333;
      font-weight: bold;
    }
  
    main {
      padding: 20px;
    }
  
    .product {
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 20px;
      margin-bottom: 20px;
      max-width: 350px;
    }
  
    button {
      background-color: #4caf50;
      color: #fff;
      padding: 10px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
  
    button:hover {
      background-color: #45a049;
    }
    img{
        width:300px
    }
  </style>
  
  <header>
    <h1>PL-butikken</h1>
  </header>
  <body>
  <nav>
    <a href="/">Hjem</a>
    <a href="tabell">Tabell</a>
    <a href="kamper">Kamper</a>
    <a href="nyheter">Nyhetsbrev </a>
    <a href="butikk">Butikk</a>
  </nav>
  
  <main>
    <div class="product">
      <h2>Ikke mas-koppen</h2>
      <img src="https://www.godthaill.com/cdn/shop/products/ikkemaspremierleaguemugrek.jpg?v=1663324199" alt="Bilde av kopp">
      <p>Få ro imens du ser Premier-leauge, og kjøp koppen nå!</p>
      <p>Pris: 120,-</p>
      <button on:click={addToCart}>Legg til i handlevogna</button>
    </div>
  </main>
  </body>