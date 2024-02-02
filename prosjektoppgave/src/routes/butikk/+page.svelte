<script>
    let cart = 0;
    let pris = 0;
    let cartItems = [];

    let produkt = [
        { id: 1, name: "Ikke mas-koppen", pris: 120, beskrivelse: "Få ro imens du ser Premier-leauge, og kjøp koppen nå!", bilde: "https://www.godthaill.com/cdn/shop/products/ikkemaspremierleaguemugrek.jpg?v=1663324199" },
        { id: 2, name: "PL-sekken", pris: 349, beskrivelse: "Bær med deg det du trenger, og i tilegg se classy ut!", bilde: "https://www.tradeinn.com/f/13736/137368906/nike-premier-league-backpack.jpg" },
        { id: 3, name: "Mini PL-trofee", pris: 199, beskrivelse: "Et mini trofee, perfekt for hylla! Høyde 16cm.", bilde: "https://img.fruugo.com/product/3/13/1267893133_max.jpg" }
    ];

    function addProductToCart(product) {
        let existingItem = cartItems.find((item) => item.id === product.id);

        if (existingItem) {
            existingItem.quantity += 1;
        } else {
            cartItems.push({ id: product.id, name: product.name, quantity: 1, pris: product.pris });
        }
        pris += product.pris;
        updateCart();
    }

    function updateQuantity(productId, newQuantity) {
        let itemToUpdate = cartItems.find((item) => item.id === productId);
        if (itemToUpdate) {
            itemToUpdate.quantity = newQuantity;
            updateCart();
        }
    }

    function increaseQuantity(productId) {
        let itemToUpdate = cartItems.find((item) => item.id === productId);
        if (itemToUpdate) {
            itemToUpdate.quantity += 1;
            updateQuantity(productId, itemToUpdate.quantity);
        }
    }

    function decreaseQuantity(productId) {
        let itemToUpdate = cartItems.find((item) => item.id === productId);
        if (itemToUpdate && itemToUpdate.quantity > 1) {
            itemToUpdate.quantity -= 1;
            updateQuantity(productId, itemToUpdate.quantity);
        }
    }

    function updateCart() {
        cart = cartItems.reduce((total, item) => total + item.quantity, 0);
    }
</script>

<header>
    <h1>PL-butikken</h1>
</header>

<body>
    <nav>
        <a href="/">Hjem</a>
        <a href="tabell">Tabell</a>
        <a href="kamper">Kamper</a>
        <a href="nyheter">Nyhetsbrev </a>
        <div
            class="cart-indicator"
            on:click={() =>
                (document.querySelector(".cart-modal").style.display = "flex")}
        >
            <img
                class="cart-logo"
                src="https://i.pinimg.com/originals/15/4f/df/154fdf2f2759676a96e9aed653082276.png"
                alt="handlevogn"
            />
            {#if cart > 0}
                <span class="cart-indicator">{cart}</span>
            {/if}
        </div>
    </nav>

    <main>
        {#each produkt as product (product.id)}
            <div class="product">
                <h2>{product.name}</h2>
                <img src={product.bilde} alt={"Bilde av " + product.name} />
                <p>{product.beskrivelse}</p>
                <p>Pris: {product.pris},-</p>
                <button on:click={() => addProductToCart(product)}>Legg til i handlevogna</button>
            </div>
        {/each}
    </main>

    <!-- Cart Modal -->
    <div class="cart-modal">
        <div class="cart-content">
          <h2>Handlevognen din:</h2>
          {#if cart > 0}
    <div id="cart-content">
        {#each cartItems as item}
            <p>
                <button on:click={() => decreaseQuantity(item.name)}>-</button>
                {item.quantity}x {item.name}
                <button on:click={() => increaseQuantity(item.name)}>+</button>
            </p>
        {/each}
    </div>
    <p id="cart-total">Sum: {pris},-</p>
    <button on:click={() => (document.querySelector(".cart-modal").style.display = "none")}>Lukk</button>
    <button on:click={() => (document.querySelector(".cart-modal2").style.display = "flex")}>Gå videre til betaling</button>
{:else}
    <p>Handlevognen er tom:/</p>
    <button on:click={() => (document.querySelector(".cart-modal").style.display = "none")}>Lukk</button>
{/if}
        </div>
      </div>

      <div class="cart-modal2">
        <div class="cart-content">
          <h2>Betaling:</h2>
          {#if cart>0}
          {#each cartItems as item}
              <p>
                
                {item.quantity}x {item.name}
              </p>
            {/each}
            <p>Sum: {pris},-</p>
            {/if}
            <button on:click={() => (document.querySelector(".cart-modal2").style.display = "none")(document.querySelector(".cart-modal").style.display = "none")}>Lukk</button>
          
        </div>
      </div>
</body>

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
        transition: 0.3s;
        padding:10px 15px 10px 15px
    }
    a:hover{
        background-color: rgb(197, 233, 201);
        transition: 0.3s;
    }

    main {
        padding: 20px;
        display: grid;
        gap: 20px;
        grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
        justify-content: flex-end; /* Align products to the right */
    }
    .product {
        display: grid;
        justify-items: center;
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

    img {
        max-width: 275px;
        
    }

    .cart-indicator {
        position: absolute;
        top: 35px;
        right: 10px;
        background-color: rgb(253, 253, 253);
        color: #262424;
        border-radius: 50%;
        padding: 5px;
        font-size: 12px;
        cursor: pointer;
    }

    .cart-modal {
        display: none;
        position: fixed;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        background: rgba(0, 0, 0, 0.5);
        justify-content: center;
        align-items: center;
    }

    .cart-modal2 {
        display: none;
        position: fixed;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        background: rgba(255, 255, 255, 1);
        justify-content: center;
        align-items: center;
    }

    .cart-content {
        background-color: #fff;
        padding: 20px;
        border-radius: 8px;
        max-width: 70%;
        text-align: center;
    }

    .cart-logo {
        width: 40px;
        height: 40px;
    }
</style>
