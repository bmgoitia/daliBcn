
<script>
	import { onMount } from 'svelte';
	import { writable } from 'svelte/store';

	// Datos de ejemplo
	const mapaLlocsDali = [
	    { 
	        nombreEdificio: 'Ateneu', 
	        descrEdificio: 'Descripción del Ateneu', 
	        imgEdificio: 'ateneu.jpg', 
	        coordEdificio: [2.1700, 41.3850] 
	    },
	    { 
	        nombreEdificio: 'Restaurante Caracoles', 
	        descrEdificio: 'Descripción del rte Caracoles', 
	        imgEdificio: 'caracoles.jpg', 
	        coordEdificio: [2.1760, 41.3805] 
	    },
	    { 
	        nombreEdificio: 'Llongueras', 
	        descrEdificio: 'Descripción del encuentro con Llongueras', 
	        imgEdificio: 'llongueras.jpg', 
	        coordEdificio: [2.1603, 41.3925] 
	    },
	    { 
	        nombreEdificio: 'Hotel Ritz', 
	        descrEdificio: 'Descripción del hotel Ritz', 
	        imgEdificio: 'ritz1.jpg', 
	        coordEdificio: [2.1700, 41.3910] 
	    },
	];

	let currentIndex = writable(0);

	function nextSlide() {
		currentIndex.update(n => (n + 1) % mapaLlocsDali.length);
	}

	function prevSlide() {
		currentIndex.update(n => (n - 1 + mapaLlocsDali.length) % mapaLlocsDali.length);
	}
</script>



<div class="lv_container">
	<!-- Bloque 1: Carrusel de thumbnails -->
	<div class="lv_carousel">
		<div class="lv_arrows" on:click={prevSlide}>&#9664;</div>
		<div class="lv_thumbnailWrapper">
			{#each mapaLlocsDali as lugar, i}
				{#if i === $currentIndex}
					<div>
						<img src={lugar.imgEdificio} alt={lugar.nombreEdificio} class="lv_thumbnail" />
						<p>{lugar.nombreEdificio}</p>
					</div>
				{/if}
			{/each}
		</div>
		<div class="lv_arrows" on:click={nextSlide}>&#9654;</div>
	</div>

	<!-- Bloque 2: Información del lugar -->
	<div class="lv_infoBlock">
		{#each mapaLlocsDali as lugar, i}
			{#if i === $currentIndex}
				<img src={lugar.imgEdificio} alt={lugar.nombreEdificio} class="lv_infoImage" />
				<div class="lv_infoText">
					<h2>{lugar.nombreEdificio}</h2>
					<p>{lugar.descrEdificio}</p>
				</div>
			{/if}
		{/each}
	</div>

	<!-- Bloque 3: Mapa -->
	<div class="lv_map">
		<p>Mapa aquí (Mapbox se integrará más tarde)</p>
	</div>
</div>






<style>
	.lv_container {
		display: grid;
		grid-template-rows: 2fr 2fr 6fr;
		height: 100vh;
	}

	.lv_carousel {
		display: flex;
		align-items: center;
		justify-content: space-between;
		position: relative;
		padding: 1rem;
	}

	.lv_thumbnailWrapper {
		display: flex;
		overflow: hidden;
		flex-grow: 1;
		justify-content: center;
	}

	.lv_thumbnail {
		width: 50px;
		height: 50px;
		border-radius: 50%;
		margin: 0 10px;
	}

	.lv_arrows {
		cursor: pointer;
		font-size: 2rem;
	}

	.lv_infoBlock {
		display: grid;
		grid-template-columns: 4fr 6fr;
		padding: 1rem;
	}

	.lv_infoImage {
		width: 100%;
		object-fit: cover;
	}

	.lv_infoText {
		padding: 0 1rem;
	}

	.lv_map {
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: #e5e5e5;
	}
</style>
