<script>
	import { onMount } from 'svelte';
	import MovieCard from '../../../components/MovieCard.svelte';
	import { page } from '$app/stores';
	import { fly } from 'svelte/transition';

	let movieDetail;

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
			`https://api.themoviedb.org/3/movie/${$page.params.movie}?language=en-US`,
			options
		);

		const data = await res.json();

		if (res.ok) {
			movieDetail = data;
		}
	});
</script>

{#if movieDetail}
	<div
		class="movie-details"
		in:fly={{ y: 50, duration: 500, delay: 500 }}
		out:fly={{ duration: 500 }}
	>
		<div class="img-container">
			<img
				src={`https://image.tmdb.org/t/p/w500` + movieDetail.backdrop_path}
				alt={movieDetail.title}
			/>
		</div>

		<div class="txt-container">
			<h1>{movieDetail.title}</h1>
			<p class="overview">{movieDetail.overview}</p>
			<p>
				<span>Release Date: </span>
				{movieDetail.release_date} <br />
				<span>Budget:</span> ${movieDetail.budget} <br />
				<span>Rating:</span>
				{movieDetail.vote_average} <br />
				<span>Runtime:</span>
				{movieDetail.runtime}mins
			</p>
		</div>
	</div>
	<!-- <MovieCard movie={movieDetail} /> -->
{:else}
	<p>Loading...</p>
{/if}

<style>
	h1 {
		padding: 1rem 0rem 2rem;
	}
	p {
		padding: 1rem 0rem;
	}
	.img-container {
		width: 100%;
	}
	img {
		width: 100%;
		border-radius: 1rem;
	}
	.movie-details {
		margin: 2rem 20%;
	}
	span {
		font-weight: bold;
	}
</style>
