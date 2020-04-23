<svelte:options tag="flapp-watch" />

<script>
	import { onMount } from 'svelte';
	import queryString from "query-string";

	let { id } = queryString.parse( window.location.search );
	let src = '';
	let movie = null;

	onMount(async () => {

		const headers = new Headers({
			Authorization: 'TOKEN_THE_MOVIE_DB'
		});

		const config = {
			method: 'GET',
			headers: headers,
			mode: 'cors',
		};
		const res = await fetch(`https://api.themoviedb.org/3/movie/${id}`, config);
		movie = await res.json();
		src = `https://image.tmdb.org/t/p/w500${movie.poster_path}`
	});
</script>

<div>
	<div class="container">
		<div class="row">
			{#if movie }
				<div class="col-md-5">
					<img {src} alt="poster">
				</div>
				<div class="col-md-7 center">
					<h2> { movie.original_title } </h2>
					<p class="lead"> { movie.overview } </p>
					<p>
				  <span>
   					<i class="fab fa-imdb"></i>
					  { movie.vote_average }
				  </span>
					{#each movie.genres as item}
						<span class="badge badge-pill badge-dark"> { item.name } </span>
					{/each}
					<span class="badge badge-pill badge-light"> { movie.release_date } </span>
					</p>
				</div>
			{:else}
				<p>Cargando...</p>
			{/if}
		</div>
	</div>
</div>

<style>
	@import "https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css";

	img { width: 100%; }
	.center { margin: auto }
</style>
