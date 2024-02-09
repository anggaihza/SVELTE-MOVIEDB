<script>
	import { onMount } from 'svelte';
	import { page } from '$app/stores';

	import MovieCard from '../../components/MovieCard.svelte';

	let searchedMovie = [];

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
			`https://api.themoviedb.org/3/search/movie?query=${$page.params.search}&include_adult=false&language=en-US&page=1`,
			options
		);
		const data = await res.json();
		console.log(data);
		if (res.ok) {
			searchedMovie = data.results;
		}
	});
</script>

<div class="searched-movies">
	{#each searchedMovie as movie}
		<MovieCard {movie} />
	{/each}
</div>

<style>
	.searched-movies {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		column-gap: 1rem;
		row-gap: 2rem;
		height: 20vh;
	}
</style>
