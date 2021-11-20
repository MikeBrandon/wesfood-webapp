<script>
    import CartItem from '$lib/cartItem/cartItem.svelte';
    import { myCart } from '$lib/stores/cartStore';

    let total = 0;

    $myCart.forEach(item => {
        total += item.price;
    });
</script>

<style>
    * {
        margin: 0;
    }

    .total-price {
        font-size: 24px;
        font-weight: bold;
        text-align: right;
    }

    .total {
        text-align: right;
    }
</style>

<main>
    <a href="/menu">
        Menu
    </a>
    {#each $myCart as cartItem, i}
        <CartItem {cartItem}/>
    {/each}
    <p class='total-price'>
        {total}
    </p>
    <p class='total'>
        Total
    </p>
</main>