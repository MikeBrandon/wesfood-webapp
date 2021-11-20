<script>
    import { registered } from "$lib/stores/authStore";
    import Registration from '$lib/registration/index.svelte'
    import Menu from '$lib/menu/index.svelte'
</script>

<svelte:head>
    <script src="https://unpkg.com/@lottiefiles/lottie-player@latest/dist/lottie-player.js"></script>
</svelte:head>

<main>
    {#key $registered}
        {#if $registered == 0}
            <Registration/>
        {:else if $registered == 1}
            <Menu/>
        {:else}
            <div>
                <img src="https://images.pexels.com/photos/1583884/pexels-photo-1583884.jpeg" alt="">
                <h2>
                    Welcome To Wesfood
                </h2>
                <p>
                    Get food delivery to your doorstep from Wesfood. Find the meal you crave and order food easily with the wesfood web-app. Track your order in real-time.
                </p>
                <p>
                    Click Login to get started
                </p>
                <lottie-player   lottie-player src="https://assets8.lottiefiles.com/packages/lf20_AzOvUp.json"  background="transparent"  speed="1"  style="width: 200px; height: 200px;" hover loop autoplay></lottie-player>
            </div>
        {/if}
    {/key}
</main>

<style>
    main {
        color: white;
        text-align: center;
    }

    div {
        display: flex;
        flex-direction: column;
        align-items: center;
        height: 100vh;
        position: relative;
    }

    h2 {
        z-index: 10;
        margin-top: 32px;
        font-size: 32px;
    }

    img {
        position: absolute;
        width: 100vw;
        height: 100vh;
        object-fit: cover;
        z-index: -10;
    }
</style>