<script>
    let cart = 0;
    let pris = 0;
    let navn = "";
    let email = "";
    let cartItems = [];

    let produkt = [
        {
            id: 1,
            name: "Ikke mas-koppen",
            pris: 120,
            beskrivelse:
                "Få ro imens du ser Premier-leauge, og kjøp koppen nå!",
            bilde: "https://www.godthaill.com/cdn/shop/products/ikkemaspremierleaguemugrek.jpg?v=1663324199",
        },
        {
            id: 2,
            name: "PL-sekken",
            pris: 349,
            beskrivelse:
                "Bær med deg det du trenger, og i tilegg se classy ut!",
            bilde: "https://www.tradeinn.com/f/13736/137368906/nike-premier-league-backpack.jpg",
        },
        {
            id: 3,
            name: "Mini PL-trofee",
            pris: 199,
            beskrivelse: "Et mini trofee, perfekt for hylla! Høyde 16cm.",
            bilde: "https://img.fruugo.com/product/3/13/1267893133_max.jpg",
        },
        {
            id: 4,
            name: "Offisiell ball 23/24",
            pris: 1499,
            beskrivelse:
                "Årets ofisiell kampball. Ypperste kvalitet, og med teknologi som gjør at ballen suser inn i nettmaskene",
            bilde: "https://www.torshovsport.no/pub_images/original/79718.jpg",
        },
        {
            id: 5,
            name: "Nike leggbeskyttere",
            pris: 479,
            beskrivelse:
                "Robuste leggbeskyttere med leggskinn holder. Material: syntetisk gjenvunnet plastikk.",
            bilde: "https://lovellcdn.b-cdn.net/products/242469.jpg",
        },
        {
            id: 6,
            name: "Manchester United FC ",
            pris: 0,
            beskrivelse: "Rotten klubb gis bort.",
            bilde: "https://upload.wikimedia.org/wikipedia/en/thumb/7/7a/Manchester_United_FC_crest.svg/1200px-Manchester_United_FC_crest.svg.png",
        },
        {
            id: 7,
            name: "Liverpool drakt 24/25",
            pris: 1189,
            beskrivelse: "Liverpool bortedrakt 2024/2025.",
            bilde: "https://cdn.fifakitcreator.com/kits/2023/11/08/654ba929ee515.jpg",
        },
        {
            id: 8,
            name: "Nike Fotball-vannflaske",
            pris: 160,
            beskrivelse:
                "Hold deg hydrert under trening og kamper med denne høykvalitets vannflasken fra Nike.",
            bilde: "https://images.sport1.no/getpic.php?vnr=200004844613&imageFileType=WEBP&width=1200&height=1200",
        },
        {
            id: 9,
            name: "Adidas Fotball-ryggsekk",
            pris: 299,
            beskrivelse:
                "En praktisk ryggsekk med plass til fotballsko, klær og andre nødvendigheter. Adidas-stripene gir den et stilig preg.",
            bilde: "https://assets.adidas.com/images/h_840,f_auto,q_auto,fl_lossy,c_fill,g_auto/7e872005a1af4552a6d3af3c0092129f_9366/Football_Ryggsekk_Rod_HN5732_01_standard.jpg",
        },
        {
            id: 10,
            name: "Puma Fotball-hansker",
            pris: 979,
            beskrivelse:
                "Ultimat grep på ballen med moderne griptec teknologi. Redd alle skudd med de komfortable fotballhanskene fra Puma.",
            bilde: "https://www.torshovsport.no/pub_images/original/78675.jpg",
        },
        {
            id: 11,
            name: "Select Fotball",
            pris: 389,
            beskrivelse:
                "Offisiell Select-fotball for optimal ytelse. Designet for presise spark og god ballkontroll. Str. 5",
            bilde: "https://thumblr.uniid.it/product/227026/e1fe4cb3146e.jpg",
        },
        {
            id: 12,
            name: "Fotballnett fra Select",
            pris: 399,
            beskrivelse:
                "Praktisk nett for å øve på skudd og forbedre presisjonen. Enkelt å montere og ta med til treningsfeltet. Plass til 9 baller.",
            bilde: "https://thumblr.uniid.it/product/29044/aca0f8d5f6d3.jpg",
        },
    ];
    $: produktSomSkalVises = produkt.slice(0, productsToShow);
    let searchQuery = "";
    let searchedProducts = [];
    let erKLikket = false;

    function searchProducts() {
        if (searchQuery != "") {
            erKLikket = true;
            searchedProducts = produkt.filter((product) =>
                product.name.toLowerCase().includes(searchQuery.toLowerCase()),
            );

            // Reset productsToShow when a search is performed
            productsToShow = initialProductsToShow;
        }
    }

    function handleSearch(event) {
    if (event.key === "Enter") {
      searchProducts();
    }
  }

    function resetSearch() {
        searchQuery = "";
        searchedProducts = [];
        erKLikket = false;
    }

    function addProductToCart(product) {
        let existingItem = cartItems.find((item) => item.id === product.id);

        if (existingItem) {
            existingItem.quantity += 1;
        } else {
            cartItems.push({
                id: product.id,
                name: product.name,
                quantity: 1,
                pris: product.pris,
            });
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

    function updateCart() {
        cartItems = cartItems;
        cart = cartItems.reduce((total, item) => total + item.quantity, 0);
        pris = cartItems.reduce(
            (total, item) => total + item.quantity * item.pris,
            0,
        );
        console.log("cart:", cart, "Cartitems:", cartItems);
    }

    function removeProductFromCart(productId) {
        cartItems = cartItems.filter((item) => item.id !== productId);
        updateCart();
    }

    let initialProductsToShow = 8; // Number of products to initially display
    let productsToShow = initialProductsToShow;
    const productsPerLoad = 4; // Number of products to load on each "Load More" click

    function loadMore() {
        console.log(searchedProducts.length);
        if (
            productsToShow + productsPerLoad <= searchedProducts.length ||
            searchedProducts.length === 0
        ) {
            productsToShow += productsPerLoad;
            updateProducts();
        }
        updateCart();
    }

    function showLess() {
        if (productsToShow > initialProductsToShow) {
            productsToShow -= productsPerLoad;
            updateProducts();
            updateCart();
        }
    }

    function updateProducts() {
        console.log("searched products " + searchedProducts);
        console.log("searchedProducts.length " + searchedProducts.length);
        console.log("produkt.length " + produkt.length);
        const totalProducts =
            searchedProducts.length > 0
                ? searchedProducts.length
                : produkt.length;
        const rangeText = `${Math.min(productsToShow, totalProducts)}/${totalProducts} produkter vises`;
        console.log("total products " + totalProducts);

        document.querySelector(".product-range").innerText = rangeText;
        console.log("rangeText " + rangeText);
    }
    

</script>

<header>
    <h1>
        <img
            src="https://soccerarmor.com/wp-content/uploads/2022/11/Premier_League_Logo.svg_.png"
            alt="Premier Leauge"
        />
        -Butikken
    </h1>
</header>

<body>
    <nav>
        <a href="/">Hjem</a>
        <a href="tabell">Tabell</a>
        <a href="kamper">Kamper</a>
        <a href="nyheter">Nyhetsbrev </a>
        <a href="minispill">Spill</a>
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
    <div class="search-container">
        <input
            class="søkefelt"
            bind:value={searchQuery}
            on:keyup={handleSearch}
            placeholder="Søk i produkter..."
        />
        <button on:click={searchProducts}>Søk</button>
        {#if searchedProducts.length > 0}
            <button on:click={resetSearch}>Vis alle produkter</button>
        {/if}
    </div>

    <main>
        {#if searchedProducts.length === 0 && erKLikket}
            <p>Ingen produkter funnet.</p>
        {:else if searchedProducts.length > 0}
            {#each searchedProducts as product (product.id)}
                <!-- Display the matching products -->
                <div class="product">
                    <h2>{product.name}</h2>
                    <img src={product.bilde} alt={"Bilde av " + product.name} />
                    <p>{product.beskrivelse}</p>
                    <p>Pris: {product.pris},-</p>
                    <button on:click={() => addProductToCart(product)}
                        >Legg til i handlevogna</button
                    >
                </div>
            {/each}
        {:else}
            {#each produktSomSkalVises as product (product.id)}
                <!-- Display all products -->
                <div class="product">
                    <h2>{product.name}</h2>
                    <img src={product.bilde} alt={"Bilde av " + product.name} />
                    <p>{product.beskrivelse}</p>
                    <p>Pris: {product.pris},-</p>
                    <button on:click={() => addProductToCart(product)}
                        >Legg til i handlevogna</button
                    >
                </div>
            {/each}
            <div class="product-range">
                <!-- Show the range only when not searching -->
                {productsToShow}/{produkt.length} produkter vises
            </div>
        {/if}
    </main>

    <div class="knapp">
        {#if searchQuery === "" && productsToShow < produkt.length}
            <!-- Show "Load More" button only when not searching -->
            <button class="load-more-or-less" on:click={loadMore}
                >Last inn flere</button
            >
        {/if}

        {#if searchQuery === "" && productsToShow > initialProductsToShow}
            <!-- Show "Show Less" button only when not searching -->
            <button class="load-more-or-less" on:click={showLess}
                >Vis mindre</button
            >
        {/if}
    </div>

    <!-- Cart Modal -->
    <div class="cart-modal">
        <div class="cart-content">
            <h2>Handlevognen din:</h2>
            {#if cart > 0}
                <div id="cart-content">
                    {#each cartItems as item (item.id)}
                        <p>
                            <select
                                bind:value={item.quantity}
                                on:change={() =>
                                    updateQuantity(item.id, item.quantity)}
                            >
                                {#each Array(10) as _, i}
                                    <option value={i + 1}>{i + 1}</option>
                                {/each}
                            </select>
                            {item.name}
                            <button
                                on:click={() => removeProductFromCart(item.id)}
                                >Fjern</button
                            >
                        </p>
                    {/each}
                </div>
                <p id="cart-total">Sum: {pris},-</p>
                <button
                    on:click={() =>
                        (document.querySelector(".cart-modal").style.display =
                            "none")}>Lukk</button
                >
                <button
                    on:click={() =>
                        (document.querySelector(".cart-modal2").style.display =
                            "flex")}>Gå videre til betaling</button
                >
            {:else}
                <p>Handlevognen er tom:/</p>
                <button
                    on:click={() =>
                        (document.querySelector(".cart-modal").style.display =
                            "none")}>Lukk</button
                >
            {/if}
        </div>
    </div>


    <div class="cart-modal2">
        <div class="cart-content">
            <h2>Betaling:</h2>
            {#if cart > 0}
                <div id="overskrift-kasse">
                    <div class="kasse">
                        <section style="flex: 3;"><b>Produkt</b></section>
                        <section><b>MVA.</b></section>
                        <section><b>Total</b></section>
                    </div>
                </div>
                {#each cartItems as item}
                    <div class="kasse">
                        <section style="flex:3">
                            {#each produkt as product}
                                {#if product.id === item.id}
                                    <img
                                        src={product.bilde}
                                        height="40px"
                                        alt={"Bilde av " + product.name}
                                    />
                                {/if}
                            {/each}
                            {item.quantity}x {item.name}
                        </section>
                        <section>{item.pris * 0.25 * item.quantity}</section>
                        <section>{item.pris * item.quantity}</section>
                    </div>
                {/each}
            {/if}
            <p>Sum: {pris},-</p>
            <form>
                <input
                    placeholder="Fornavn og etteravn"
                    bind:value={navn}
                    required
                /><br />

                <input
                    type="email"
                    placeholder="E-post"
                    bind:value={email}
                    required
                /><br />
            </form>
            <button
                on:click={() =>
                    (document.querySelector(".cart-modal2").style.display =
                        "none")(
                        (document.querySelector(".cart-modal").style.display =
                            "none"),
                    )}>Lukk</button
            >
        </div>
    </div>
</body>
<footer>
    <div class="social-icons">
        <a
            href="https://twitter.com/premierleague"
            target="_blank"
            rel="noopener noreferrer"
        >
            <img
                src="https://about.twitter.com/content/dam/about-twitter/x/brand-toolkit/logo-black.png.twimg.1920.png"
                alt="Twitter"
            />
        </a>
        <a
            href="https://www.instagram.com/premierleague/"
            target="_blank"
            rel="noopener noreferrer"
        >
            <img
                src="https://www.edigitalagency.com.au/wp-content/uploads/new-Instagram-logo-png-full-colour-glyph.png"
                alt="Instagram"
            />
        </a>
        <a
            href="https://www.facebook.com/premierleague/"
            target="_blank"
            rel="noopener noreferrer"
        >
            <img
                src="https://www.freeiconspng.com/uploads/facebook-logo-png-white-facebook-logo-png-white-facebook-icon-png--32.png"
                alt="Facebook"
            />
        </a>
    </div>
    <p>&copy; 2024 PL-butikken. Alle rettigheter reservert.</p>
</footer>

<style>
    .search-container {
        margin-left: 10px;
    }

    .søkefelt {
        width: 10%;
        transition: 0.3s;
        margin: 10px 5px 0px 10px;
    }
    .søkefelt::content {
        width: 25%;
        transition: 0.3s;
    }
    input {
        padding: 10px;
        border: 1px solid #ccc;
        border-radius: 5px;

        box-sizing: border-box;
    }
    .kasse {
        display: flex;
        margin-bottom: 5px;
        background-color: #f7f6f6;
        padding: 10px;

        box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
    }

    #overskrift-kasse {
        border-bottom: #252624 solid 2px;
        padding-bottom: 5px;
        margin-bottom: 10px;
    }
    section {
        flex: 1;
    }
    form {
        display: flex;
        flex-direction: column;
        gap: 10px;
        text-align: left;
    }
    .knapp {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .load-more-or-less {
        background-color: rgba(191, 227, 254, 0.688);
        align-content: center;
        justify-content: center;
        padding: 20px;
        border-radius: 14px;
        transition: 0.4s;
        color: #252624;
        margin-bottom: 20px;
        width: 20%;
        padding: 20px;
    }
    .load-more-or-less:hover {
        background-color: #b2dde8;
        transition: 0.3s;
    }
    footer {
        background-color: #333;
        color: white;
        text-align: center;
        padding: 10px 0px 10px 0px;

        bottom: 0;
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .social-icons {
        display: flex;
        gap: 10px;
        margin-bottom: 10px;
    }

    .social-icons a {
        text-decoration: none;
        color: white;
    }

    .social-icons img {
        width: 30px; /* Adjust the size as needed */
        height: 30px; /* Adjust the size as needed */
    }

    body {
        font-family: Arial, sans-serif;
        background-color: #f7f6f6;
        margin: 0;
        padding: 0;
    }

    header {
        background: linear-gradient(
            45deg,
            rgb(98, 255, 242),
            rgb(192, 242, 123),
            rgb(93, 242, 93),
            rgb(98, 255, 242)
        );
        color: rgb(61, 25, 91);
        text-align: center;
        padding: 10px;
        font-family:
            system-ui,
            -apple-system,
            BlinkMacSystemFont,
            "Segoe UI",
            Roboto,
            Oxygen,
            Ubuntu,
            Cantarell,
            "Open Sans",
            "Helvetica Neue",
            sans-serif;
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
        padding: 10px 15px 10px 15px;
    }
    nav a:hover {
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

        gap: 20px;

        justify-content: flex-end; /* Align products to the right */
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
        position: fixed;
        top: 35px;
        right: 10px;
        color: #262424;
        background-color: rgba(0, 0, 0, 0.2);
        padding: 5px;
        font-size: 12px;
        cursor: pointer;
        border-radius: 40%;
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
        width: 50px;
    }
</style>
