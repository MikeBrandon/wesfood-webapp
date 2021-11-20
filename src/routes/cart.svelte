<script>
    import CartItem from '$lib/cartItem/cartItem.svelte';
    import { myCart } from '$lib/stores/cartStore';

    let total = 0;

    updateTotal();

    function updateTotal() {
        total = 0;
        $myCart.forEach(item => {
            total += item.price;
        });
    }

    function clearCart() {
        myCart.set([]);
        total = 0;
    }

    async function confirmOrder() {
        // try {
        //     const docRef = await addDoc(collection(db, "orders"), {
        //         name: 'testName'
        //     });
        //     console.log("Document written with ID: ", docRef.id);
        // } catch (e) {
        //     console.error("Error adding document: ", e);
        // }
    }
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

    .tax-price {
        font-size: 16px;
        font-weight: bold;
        text-align: right;
    }

    .total {
        text-align: right;
    }

    .tax {
        font-size: 12px;
        text-align: right
    }

    main {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
    }

    .button {
        position: fixed;
        display: flex;
        bottom: 0;
        right: 0;
        padding: 8px;
    }

    .space {
        width: 8px;
    }
</style>

<main>
    <section>
        <a href="/menu">
            Menu
        </a>
        {#each $myCart as cartItem, i}
            <CartItem {cartItem} on:itemDeleted={updateTotal}/>
        {/each}
        <p class='total-price'>
            {total}
        </p>
        <p class='total'>
            Total
        </p>
        <p class='tax-price'>
            {Math.round((total * 0.07) * 100) / 100}
        </p>
        <p class='tax'>
            Tax (7%)
        </p>
        <p class='total-price'>
            {Math.round((total * 1.07) * 100) / 100}
        </p>
        <p class='total'>
            Grand Total
        </p>
    </section>
    <section>
        <div class='button'>
            <button on:click={clearCart}>
                Clear Cart
            </button>
            <div class="space"/>
            <button on:click={confirmOrder}>
                Confirm Order
            </button>
        </div>
    </section>
</main>