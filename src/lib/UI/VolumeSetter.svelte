<script lang="ts">
	import { onMount } from 'svelte';
	import { writable } from 'svelte/store';

	export let volume = 50; // in %

	let numbers: number[] = [];

	let basket: HTMLElement | undefined;
	let basketPos = 0;

	const placeRandom = (index: number) => {
		// place the number at the given index
		numbers[index] = Math.floor(Math.random() * 100000000);
	};

	const fillNumbers = () => {
		// fill with 5 random 8-digit numbers
		for (let i = 0; i < 5; i++) {
			placeRandom(i);
		}
	};

	onMount(() => {
		fillNumbers();
	});

	// make the basket slide from 0 to 600 px and back gradually
	let direction = 1;
	let position = 0;
	const interval = setInterval(() => {
		position += direction * 10;
		if (basket) basket.style.transform = `translateX(${position}px)`;
		basketPos = position;
		if (position >= 600) {
			direction = -1;
		} else if (position <= 0) {
			direction = 1;
		}
	}, 60);

	const selectNumber = (number: number) => {
		const _volume = (number % 28739) % 100;
		const confirmed = confirm(`Do you not want to set the volume to ${_volume} percent?`);
		if (!confirmed) {
			volume = _volume;
		}
	};

	const onNumberClick = (event: MouseEvent) => {
		const block = event.target as HTMLElement;
		const index = numbers.indexOf(parseInt(block.dataset.number!));
		// make the block fall (translateY) and then disappear
		if (!block) return;
		block.style.transition = 'transform 1s ease-in';
		block.style.transform = 'translateY(240px)';
		setTimeout(() => {
			// if the number is above the basket's pos, select it
			const minPosition = (index / 5) * 600;
			const maxPosition = ((index + 1) / 5) * 600;

			if (minPosition < basketPos && basketPos < maxPosition) {
				selectNumber(numbers[index]);
				placeRandom(index);

				block.style.transition = 'transform 0s';
				block.style.transform = 'translateY(0)';
			} else {
				block.style.transition = 'transform 1s ease-out';
				block.style.transform = 'translateY(0)';
			}
		}, 1000);
	};
</script>

<div class="setter-container">
	<div class="numbers">
		{#each numbers as number}
			<button class="number" data-number={number} on:click={onNumberClick}>{number}</button>
		{/each}
	</div>
	<div class="basket" bind:this={basket}>
		<img src="kuka_cut.png" alt="" />
	</div>
</div>

<style lang="scss">
	.setter-container {
		width: 700px;
	}

	.numbers {
		width: 100%;
		display: grid;
		grid-template-columns: repeat(5, 1fr);
		gap: 0;

		.number {
			display: flex;
			align-items: center;
			justify-content: center;
			padding: 10px;
			outline: solid red;
			background-color: lime;
		}
	}

	.basket {
		display: block;
		width: 100px;
		height: 140px;
		margin-top: 200px;

		img {
			width: 100%;
			height: 100%;
		}
	}
</style>
