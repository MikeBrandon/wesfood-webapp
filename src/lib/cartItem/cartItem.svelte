<script>
    import { myCart } from "$lib/stores/cartStore";

    export let cartItem;

    function deleteItem() {
        let currentCart = [...$myCart];
        const index = currentCart.indexOf(cartItem);
        if (index > -1) {
            currentCart.splice(index, 1);
        }
        myCart.set(currentCart);
    }
</script>

<div class='cart-item'>
    <div class='left-side'>
        {#if cartItem.imgUrl}
            <img class='image' src={cartItem.imgUrl} alt={cartItem.name}>
        {:else}
            <img class='no-image' src="/img/no-image.png" alt={cartItem.name}>
        {/if}
        <p class='food-title'>{cartItem.name}</p>
    </div>
    <div class="right-side">
        <p>
            Ksh. {cartItem.price}
        </p>
        <img class='delete' src="/img/delete.png" alt="" on:click={deleteItem}>
    </div>
</div>

<style>
    * {
        font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    }

    img {
        height: 50px;
        width: 50px;
        object-fit: cover;
        border-radius: 6px;
        margin-right: 4px;
    }

    .cart-item {
        display: flex;
        justify-content: space-between;
        box-shadow: grey 2px 2px 2px 2px;
        margin-bottom: 8px;
        margin-top: 8px;
        border-radius: 4px;
        padding: 4px;
    }

    .left-side {
        display: flex;
    }

    .right-side {
        display: flex;
        font-weight: bold;
        justify-content: center;
        align-items: center;
    }

    .delete {
        height: 30px;
        width: auto;
        margin-left: 8px;
    }
</style>