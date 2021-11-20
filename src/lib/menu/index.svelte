<script>
    import { collection, getDocs, getFirestore } from "firebase/firestore";
    import { onMount } from "svelte";
    import MenuItem from '$lib/menuItem/index.svelte';

    const db = getFirestore();

    let menuArray = [];

    async function getMenuItems() {
        const querySnapshot = await getDocs(collection(db, "menu"));
        menuArray = [];
        querySnapshot.forEach((doc) => {
            // doc.data() is never undefined for query doc snapshots
            menuArray.push(doc.data())
        });
    }
        
    onMount(() => {
        getMenuItems();
    });
</script>

<style>
    main {
        color: black;
        text-align: left;
    }
</style>

<main>
    <h2>
        Menu
    </h2>
    {#each menuArray as menuItem}
        <MenuItem {menuItem}/>
    {/each}
</main>

