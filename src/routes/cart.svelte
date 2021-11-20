<script>
    import CartItem from '$lib/cartItem/cartItem.svelte';
    import { userStore } from '$lib/stores/authStore';
    import { myCart } from '$lib/stores/cartStore';
    import { addDoc, collection, getDocs, getFirestore } from '@firebase/firestore';
    import { onMount } from 'svelte';

    let total = 0;
    let payment = 1;
    let orderPlaced = false;
    let phone = null;

    const db = getFirestore();

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

    async function getContact() {
        const querySnapshot = await getDocs(collection(db, "users"));
        querySnapshot.forEach((doc) => {
            // doc.data() is never undefined for query doc snapshots
            if (doc.data().id == $userStore.uid) {
                phone = doc.data().phone;
            }
        });
    }

    async function confirmOrder() {
        if ($userStore) {
            if ($myCart.length > 0) {
                try {
                    const docRef = await addDoc(collection(db, "orders"), {
                        user: $userStore.displayName,
                        number: phone,
                        orders: $myCart,
                        time: new Date(),
                        cash: (payment === 1) ? true : false
                    });
                    myCart.set([]);
                    total = 0;
                    alert('Order submitted. Please wait for a confirmation call.');
                    orderPlaced = true;
                    console.log("Document written with ID: ", docRef.id);
                } catch (e) {
                    console.error("Error adding document: ", e);
                }
            } else {
                alert('You have nothing on your Cart!');
            }
        } else {
            alert('Please Login first!');
        }
    }

    onMount(() => {
        getContact();
    })
</script>

<style>
    * {
        margin: 0;
        font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    }

    .total-price {
        font-size: 24px;
        font-weight: bold;
        text-align: right;
    }

    .bill {
        font-size: 24px;
        font-weight: bold;
        text-align: right;
        text-decoration-line: underline;
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
        display: flex;
    }

    .footer {
        position: fixed;
        bottom: 0;
        right: 0;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: flex-end;
        padding: 8px;
    }

    .space {
        width: 8px;
    }

    .payment {
        margin-bottom: 8px;
    }
</style>

<main>
    <section>
        <a href="/menu">
            Menu
        </a>
        <h2>
            Menu
        </h2>
        {#each $myCart as cartItem, i}
            <CartItem {cartItem} on:itemDeleted={updateTotal} admin={false}/>
        {/each}
        <p class='bill'>
            Bill
        </p>
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
        <div class='footer'>
            <div class='payment'>
                <label>
                    <input type=radio bind:group={payment} name="scoops" value={1}>
                    Cash
                </label>
                
                <label>
                    <input type=radio bind:group={payment} name="scoops" value={2}>
                    Credit Card
                </label>
            </div>
            <div class='button'>
                <button on:click={clearCart}>
                    Clear Cart
                </button>
                <div class="space"/>
                {#if !orderPlaced}
                    <button on:click={confirmOrder} disabled={!phone}>
                        Confirm Order
                    </button>
                {/if}
            </div>
        </div>
    </section>
</main>