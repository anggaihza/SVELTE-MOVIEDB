<script>
	import { onMount } from 'svelte';
	import global from './global.css';
	import PopularMovies from '../components/PopularMovies.svelte';
	import SearchMovie from '../components/SearchMovie.svelte';
	import { fly } from 'svelte/transition';

	let popular = [];

	onMount(async () => {
		const options = {
			method: 'GET',
			headers: {
				accept: 'application/json',
				Authorization:
					'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI4NzIxZDdiOTU4YTExNzI4MzU3MWJhYTcyMjM1MGQzZiIsInN1YiI6IjY1YzM0ODUzOGUyZTAwMDE2MmE1N2QxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.oAYr4oUGHzgz8-iA35v-rurP1IGwz7lsZkoxjhlz9xI'
			}
		};

		const res = await fetch(
			'https://api.themoviedb.org/3/movie/popular?language=en-US&page=1',
			options
		);
		const data = await res.json();
		if (res.ok) {
			popular = data.results;
		}
	});
</script>

<section in:fly={{ y: 50, duration: 500, delay: 500 }} out:fly={{ duration: 500 }}>
	{#if popular.length > 0}
		<SearchMovie />
		<PopularMovies {popular} />
	{:else}
		<p>Loading...</p>
	{/if}
</section>
