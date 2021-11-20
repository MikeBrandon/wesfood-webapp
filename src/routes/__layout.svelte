<script>
    import Navbar from '$lib/navbar/index.svelte';

    import { app } from '$lib/utils/firebase';
    const myApp = app;
</script>

<svelte:head>
	<title>WesFood Inc.</title>
</svelte:head>

<Navbar/>
<slot />
