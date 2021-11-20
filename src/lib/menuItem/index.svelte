<script>
    import { myCart } from "$lib/stores/cartStore";

    export let menuItem;
    console.log(menuItem);

    function addToCart() {
        let currentCart = [...$myCart];
        currentCart.push(menuItem);
        myCart.set(currentCart);
    }
</script>

<style>
    p {
        margin: 0;
    }

    * {
        font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    }

    main {
        width: 160;
        padding-top: 2px;
        border-radius: 4px;
        margin-top: 8px;
        padding-left: 8px;
        padding-right: 8px;
        box-shadow: grey 2px 2px 2px 2px;
    }

    .image {
        width: 100%;
        height: 200px;
        object-fit: cover;
        border-radius: 8px;
    }

    .no-image {
        height: 200px;
        object-fit: contain;
        width: 100%;
    }

    .food-title {
        font-size: 20px;
        font-weight: bold;

    }

    .clock {
        height: 20px;
    }

    .details {
        display: flex;
        justify-content: space-between;
    }

    .right-stuff {
        display: flex;
    }

    .atc-btn {
        margin-top: 8px;
        margin-bottom: 8px;
    }

    .panel {
        display: flex;
        justify-content: space-between;
    }

</style>

<main>
    {#if menuItem.imgUrl}
        <img class='image' src={menuItem.imgUrl} alt={menuItem.name}>
    {:else}
        <img class='no-image' src="/img/no-image.png" alt={menuItem.name}>
    {/if}
    <p>Ksh. {menuItem.price}</p>
    <div class='details'>
        <p class='food-title'>{menuItem.name}</p>
        <div class='right-stuff'>
            <img class='clock' src="/img/clock.png" alt="">
            <p>{menuItem.prepMin}min</p>
        </div>
    </div>
    <p class='description'>
        {menuItem.description}
    </p>
    <div class='panel'>
        <div>
            <button class='atc-btn' on:click={addToCart}>
                Add to cart
            </button>
            <button class='atc-btn'>
                Like
            </button>
        </div>
        <div class='right-stuff'>
            <img class='clock' src="/img/heart.png" alt="">
            <p>{menuItem.stars}</p>
        </div>
    </div>
</main>