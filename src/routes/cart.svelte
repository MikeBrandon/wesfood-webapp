<script>
    import CartItem from '$lib/cartItem/cartItem.svelte';
    import { myCart } from '$lib/stores/cartStore';
</script>

<style>

</style>

<main>
    <a href="/menu">
        Menu
    </a>
    {#each $myCart as cartItem, i}
        <CartItem {cartItem}/>
    {/each}
</main>