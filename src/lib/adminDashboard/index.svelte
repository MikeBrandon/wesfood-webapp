<script lang='ts'>
import { onMount } from "svelte";
import { getDocs, collection, getFirestore, addDoc, deleteDoc, doc } from 'firebase/firestore';
import CartItem from "$lib/cartItem/cartItem.svelte";

    export let userName = 'User';

    let numberOfUsers = 0;

    let isAddingFood = false;
    let foodShow = false;
    let menuArray = [];
    let fdName: string = '';
    let fdPrep: number = 0;
    let fdPrice: number = 0;
    let fdDescription: string = '';
    let fdVariantString: string = '';
    let fdImage : string = '';

    const db = getFirestore();

    async function getNumberOfUsers() {
        const querySnapshot = await getDocs(collection(db, "users"));
        querySnapshot.forEach((doc) => {
            numberOfUsers++;
        });
    }

    function showFoodAdd() {
        if (isAddingFood) {
            isAddingFood = false;
        } else {
            isAddingFood = true;
        }
    }

    function showFoods() {
        if (foodShow) {
            foodShow = false;
        } else {
            foodShow = true;
        }
    }

    async function submitFood() {
        try {
            const docRef = await addDoc(collection(db, "menu"), {
                name: fdName,
                prepMin: fdPrep,
                price: fdPrice,
                stars: 0,
                description: fdDescription,
                imgUrl: fdImage,
                variant: fdVariantString.split(';')
            });
            isAddingFood = false;
            console.log("Document written with ID: ", docRef.id);
        } catch (e) {
            console.error("Error adding document: ", e);
        }
    }

    async function getMenuItems() {
        const querySnapshot = await getDocs(collection(db, "menu"));
        menuArray = [];
        querySnapshot.forEach((doc) => {
            // doc.data() is never undefined for query doc snapshots
            menuArray.push({
                id: doc.id,
                data: doc.data()
            })
        });
    }

    async function deleteFood(event) {
        await deleteDoc(doc(db, "menu", event.detail));
        getMenuItems();
    }

    onMount(() => {
        getNumberOfUsers();
        getMenuItems();
    })
</script>

<main>
    <h2>Welcome back {userName}</h2>
    <div class='user-num'>
        <p class='users'>
            {numberOfUsers}
        </p>
        <p>Number of Users</p>
    </div>

    <div class="foods">
        <div class='buttons'>
            <button on:click={showFoodAdd}>Add New Food</button>
            <button on:click={showFoods}>Show Foods</button>
        </div>
        {#if isAddingFood}
            <div class='anf'>
                <h3>Add New Food</h3>
                <div>
                    <p>Name</p>
                    <input type="text" bind:value={fdName}>
                </div>
                <div>
                    <p>Preparation Time(Min)</p>
                    <input type="number" bind:value={fdPrep}>
                </div>
                <div>
                    <p>Price</p>
                    <input type="number" bind:value={fdPrice}>
                </div>
                <div>
                    <p>Description</p>
                    <input type="text" bind:value={fdDescription}>
                </div>
                <div>
                    <p>Variants (Write with ; between each variant)</p>
                    <input type="text" bind:value={fdVariantString}>
                </div>
                <div>
                    <p>Image URL</p>
                    <input type="text" bind:value={fdImage}>
                </div>
                <button on:click={submitFood}>
                    Submit
                </button>
            </div>
        {/if}
        {#if foodShow}
            <section>
                {#each menuArray as menuItem}
                    <CartItem cartItem={menuItem.data} admin={true} on:deleteMenu={deleteFood} id={menuItem.id}/>
                {/each}
            </section>
        {/if}
    </div>
</main>

<style>
    p {
        margin: 4px;
    }

    h3 {
        margin-top: 4px;
        margin-bottom: 4px;
    }

    input {
        margin-bottom: 8px;
    }

    main, div {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .users {
        font-size: 32px;
    }

    .anf {
        margin-top: 8px;
        box-shadow: 2px 2px 2px 2px grey;
        padding: 8px;
        border-radius: 6px;
    }
</style>