<script>
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
  import Nav from '$lib/components/nav.svelte';
  import Hero from '$lib/components/hero.svelte';
  import Footer from '$lib/components/footer.svelte';
	
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<Nav />

<Hero img="" title="" />
<main class="">
	{@render children?.()}	
</main>
<Footer/>
