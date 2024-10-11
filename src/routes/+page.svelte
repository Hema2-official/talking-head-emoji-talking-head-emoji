<script lang="ts">
	import VolumeSvg from '$lib/UI/VolumeSVG.svelte';
	import VolumeSetter from '$lib/UI/VolumeSetter.svelte';
	let playing = false;
	let position = 0;
	let volume = 50;
	let videoElement: HTMLVideoElement | undefined;

	let isVolumeSetterVisible = false;

	function togglePause() {
		if (playing) {
			videoElement?.pause();
		} else {
			videoElement?.play();
		}
		playing = !playing;
	}

	function goFullscreen() {
		if (videoElement?.requestFullscreen) {
			videoElement?.requestFullscreen();
		}
	}

	function handlePlayPause() {
		playing = !videoElement?.paused;
	}

	$: if (videoElement) {
		videoElement.currentTime = position;
	}
	$: if (videoElement) {
		videoElement.volume = volume / 100;
	}
</script>

<svelte:head>
	<title>Holy fvck Lois I'm</title>
	<link href="https://fonts.cdnfonts.com/css/jokerman" rel="stylesheet" />
</svelte:head>

{#if playing}
	<h1>nevuh gonna mute me down 🗣️🗣️🗣️🗣️🗣️🗣️</h1>
{/if}

<div class="video-wrapper">
	<div class="video-container">
		<!-- svelte-ignore a11y-media-has-caption -->
		<video bind:this={videoElement} on:play={handlePlayPause} on:pause={handlePlayPause}>
			<source src="not_sus_video.mp4" type="video/mp4" />
			Your browser does not support the video tag.
		</video>
		<div class="controls">
			<button on:click={togglePause}>{playing ? 'Pause' : 'Paused'}</button>
			<button
				on:click={() => {
					isVolumeSetterVisible = !isVolumeSetterVisible;
				}}><VolumeSvg color="magenta" /></button
			>
			<input type="range" min={0} max={videoElement?.duration} bind:value={position} />
			<span>3Lz26yhuXhy7uWMR9kU7yPStB1JzSYid9H</span>
		</div>
	</div>
</div>

{#if isVolumeSetterVisible}
	<VolumeSetter bind:volume />
{/if}

<style lang="scss">
	h1 {
		color: #ff0;
		font-size: 66px;
		font-weight: 700;
		font-family: 'Jokerman';
		text-shadow: red 5px 5px 10px;
		border: inset #fff;
		margin: 60px;
	}

	.video-wrapper {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
	}

	.video-container {
		position: relative;
		width: 640px;
		border: 20px dashed red;
	}

	video {
		width: 100%;
	}

	.controls {
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		display: flex;
		justify-content: start;
		background-color: rgba(0, 0, 0, 0.7);
		pointer-events: none;
		z-index: 1000;
		margin: 12px 6px;

		button {
			background-color: rgba(0, 0, 0, 0.5);
			color: white;
			border: none;
			padding: 5px 10px;
			cursor: pointer;
			pointer-events: all;
		}

		button:hover {
			background-color: rgba(0, 0, 0, 0.7);
		}

		input[type='range'] {
			pointer-events: all;
			accent-color: magenta;
			width: 160px;
		}

		span {
			color: lightblue;
		}
	}
</style>
