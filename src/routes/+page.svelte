<script lang="ts">
	import { onMount } from 'svelte';

	let yesButtonSize = $state(1);
	let noButtonText = $state('No');
	let showSpecialMessage = $state(false);
	let hearts: Array<{ id: number; left: number; delay: number }> = $state([]);
	let noClickCount = $state(0);

	const noMessages = [
		'No',
		'Are you sure?',
		'Really?',
		'Think again!',
		'Please? 🥺',
		'Pretty please?',
		"Don't break my heart 💔",
		'One more chance?',
		"I'll cry 😢",
		'Final answer?'
	];

	function handleYesClick() {
		showSpecialMessage = true;
		createHearts();
	}

	function handleNoClick() {
		yesButtonSize += 0.5; // Ubah jadi 0.5 biar ga terlalu cepat gede
		noClickCount++;
		const messageIndex = Math.min(noClickCount, noMessages.length - 1);
		noButtonText = noMessages[messageIndex];
	}

	function createHearts() {
		const newHearts = Array.from({ length: 50 }, (_, i) => ({
			id: Date.now() + i,
			left: Math.random() * 100,
			delay: Math.random() * 2
		}));
		hearts = newHearts;

		setTimeout(() => {
			hearts = [];
		}, 4000);
	}

	onMount(() => {
		const interval = setInterval(() => {
			if (!showSpecialMessage && Math.random() > 0.7) {
				hearts = [
					...hearts,
					{
						id: Date.now(),
						left: Math.random() * 100,
						delay: 0
					}
				].slice(-10);
			}
		}, 2000);

		return () => clearInterval(interval);
	});
</script>

<svelte:head>
	<title>A Valentine's Question ❤️</title>
	<meta name="description" content="A special valentine's question for a special someone." />
	<meta property="og:title" content="Will you be my Valentine?" />
	<meta property="og:description" content="A special question for someone special ❤️" />
	<meta name="theme-color" content="#e73c7e" />
</svelte:head>

<main class="relative size-full min-h-screen overflow-hidden">
	<!-- Animated gradient background -->
	<div class="bg-gradient-valentine-animated animate-gradient absolute inset-0 -z-20"></div>

	<!-- Background image overlay -->
	<img
		src="/assets/heart-bg.jpg"
		class="absolute inset-0 -z-10 size-full object-cover opacity-30"
		alt="background"
	/>

	<!-- Floating hearts -->
	{#each hearts as heart (heart.id)}
		<div
			class="animate-float-up pointer-events-none absolute text-4xl"
			style="left: {heart.left}%; animation-delay: {heart.delay}s;"
		>
			❤️
		</div>
	{/each}

	<!-- Main content -->
	<div
		class="relative z-10 flex size-full min-h-screen flex-col items-center justify-center p-4 text-center"
	>
		{#if showSpecialMessage}
			<div class="animate-bounce-in flex flex-col items-center">
				<!-- Success message -->
				<div class="glass mb-6 w-full rounded-3xl p-8">
					<img
						src="/assets/cat_dance.gif"
						class="animate-pulse-slow mx-auto size-64 rounded-2xl shadow-2xl"
						alt="happy cat"
					/>
					<h1 class="text-glow mt-8 mb-4 text-4xl font-bold text-white md:text-5xl">YAY! 🎉</h1>
					<p class="mb-2 text-2xl font-semibold text-white md:text-3xl">
						You're the reason I smile every day
					</p>
					<p class="text-xl text-pink-dark italic">
						"In your eyes, I found my home. In your heart, I found my love."
					</p>
					<p class="mt-4 text-lg text-pink-50">See you on the 14th! ❤️</p>
					<div class="mt-6 flex justify-center gap-2 text-3xl">
						{#each '💕🌹✨💖🎀'.split('') as emoji, i}
							<span class="animate-pulse-slow inline-block" style="animation-delay: {i * 0.2}s;">
								{emoji}
							</span>
						{/each}
					</div>
				</div>
			</div>
		{:else}
			<div class="animate-bounce-in">
				<!-- Question -->
				<div class="glass mb-8 w-full rounded-3xl p-8">
					<img
						src="/assets/cat_heart.gif"
						class="animate-pulse-slow mx-auto mb-6 size-48 rounded-2xl shadow-2xl md:size-64"
						alt="cat with heart"
					/>
					<h1 class="text-glow mb-4 text-3xl font-bold text-white md:text-5xl lg:text-6xl">
						Will you be my Valentine?
					</h1>
					<p class="text-lg font-medium text-pink-100 md:text-xl">
						I promise to make it special! 💝
					</p>
				</div>

				<!-- Buttons -->
				<div class="flex flex-wrap items-center justify-center gap-6">
					<button
						class="shadow-glow-pink btn-valentine-custom relative z-10 rounded-xl shadow-2xl transition-all duration-300 hover:scale-110 hover:brightness-110"
						style="transform: scale({yesButtonSize}); font-size: {Math.min(
							yesButtonSize * 1.2,
							2
						)}rem;"
						onclick={handleYesClick}
						aria-label="Yes"
					>
						<span class="block px-8 py-4 md:px-12 md:py-5">Yes! 💖</span>
					</button>

					<button
						class="shadow-glow-gray btn-valentine-outline-custom rounded-xl shadow-2xl transition-all duration-300 hover:scale-95"
						onclick={handleNoClick}
						aria-label={noButtonText}
					>
						<span class="block px-8 py-4 md:px-12 md:py-5">{noButtonText}</span>
					</button>
				</div>

				<!-- Subtle hint -->
				{#if noClickCount > 3}
					<p class="mt-6 animate-pulse text-sm text-pink-200 italic">
						💡 Psst... the YES button is getting bigger for a reason 😊
					</p>
				{/if}
			</div>
		{/if}
	</div>

	<!-- Decorative elements -->
	<div class="pointer-events-none absolute top-10 left-10 animate-pulse text-6xl opacity-20">
		💕
	</div>
	<div
		class="pointer-events-none absolute top-20 right-20 animate-pulse text-5xl opacity-20"
		style="animation-delay: 1s;"
	>
		💖
	</div>
	<div
		class="pointer-events-none absolute bottom-20 left-20 animate-pulse text-6xl opacity-20"
		style="animation-delay: 2s;"
	>
		💗
	</div>
	<div
		class="pointer-events-none absolute right-10 bottom-10 animate-pulse text-5xl opacity-20"
		style="animation-delay: 0.5s;"
	>
		💝
	</div>
</main>
