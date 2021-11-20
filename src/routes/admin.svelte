<script>
    import { userStore } from '$lib/stores/authStore';
    import AdminDashboard from '$lib/adminDashboard/index.svelte';

    function userIsAdmin() {
        return $userStore.email == 'bkb.braock@gmail.com';
    }
</script>

<main>
    {#if $userStore}
        {#if userIsAdmin()}
            <AdminDashboard userName={$userStore.displayName}/>
        {:else}
            <p>You are not an Admin!</p>
        {/if}
    {:else}
        <p>Please Sign in!</p>
    {/if}
</main>

<style>

</style>