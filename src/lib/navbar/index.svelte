<script>
    import { myCart } from '$lib/stores/cartStore';
    import UserBox from '$lib/userbox/index.svelte';
    if (!$myCart) {
        myCart.set([]);
    }
</script>

<main>
    <div class='logo'>
        <img src="/gif/1.gif" alt="logo">
        <a href="/">
            Wesfood
        </a>
    </div>
    <div class='right-side'>
        <a href="/cart">
            <div class='cart'>
                <p>{$myCart.length}</p>
                <img class='cart-img' src="/img/cart.png" alt="">
            </div>
        </a>
        <UserBox/>
    </div>
</main>

<style>
    main {
        display: flex;
        justify-content: space-between;
        align-items: center;
        background-color: black;
        color: white;
        border-radius: 8px;
    }

    .logo {
        display: flex;
        align-items: center;
        justify-content: center;
        margin-left: 8px;
    }

    .cart-img {
        color: white;
        height: 20px;
        width: auto;
    }

    .cart {
        margin-right: 8px;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    img {
        border-radius: 25%;
        height: 40px;
        width: 40px;
    }

    a {
        color: white;
        text-decoration: none;
        font-size: 20px;
        font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    }

    .right-side {
        display: flex;
        justify-content: center;
        align-items: center;
    }
</style>