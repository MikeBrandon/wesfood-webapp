<script lang='ts'>
import { onMount } from "svelte";
import { getDocs, collection, getFirestore, addDoc } from 'firebase/firestore';

    export let userName = 'User';

    let numberOfUsers = 0;

    let isAddingFood = false;
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

    onMount(() => {
        getNumberOfUsers()
    })
</script>

<main>
    <h2>Welcome back {userName}</h2>
    <div class='user-num'>
        <p>
            {numberOfUsers}
        </p>
        <p>Number of Users</p>
    </div>

    <div class="foods">
        <button on:click={showFoodAdd}>Add New Food</button>
        {#if isAddingFood}
            <div>
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
    </div>
</main>

<style>
    p {
        margin: 4px;
    }

    input {
        margin-bottom: 8px;
    }
</style>