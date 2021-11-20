<script>
    import { registered } from "$lib/stores/authStore";
    import Registration from '$lib/registration/index.svelte'
    import Menu from '$lib/menu/index.svelte'
</script>

<main>
    {#key $registered}
        {#if $registered == 0}
            <Registration/>
        {:else if $registered == 1}
            <Menu/>
        {:else}
            Welcome to Wesfood!
        {/if}
    {/key}
</main>

<style>

</style>