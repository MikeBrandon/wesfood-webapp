<script>
    import UserBox from '$lib/userbox/index.svelte';
</script>

<main>
    <div class='logo'>
        <img src="/gif/1.gif" alt="logo">
        <h1>
            Wesfood
        </h1>
    </div>
    <UserBox/>
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

    h1 {
        margin-left: 4px;
        font-size: 20px;
        font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    }

    img {
        border-radius: 25%;
        height: 40px;
        width: 40px;
    }
</style>