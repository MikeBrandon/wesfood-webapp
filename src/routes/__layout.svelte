<script>
	import { app } from '$lib/utils/firebase';
    import Navbar from '$lib/navbar/index.svelte';
    const myApp = app;
</script>

<svelte:head>
	<title>WesFood Inc.</title>
</svelte:head>

<Navbar/>
<slot />
