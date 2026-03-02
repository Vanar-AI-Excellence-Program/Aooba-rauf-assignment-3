<script lang="ts">
	import { fly, fade } from 'svelte/transition';

	let { data } = $props();
	let visible = $state(false);

	$effect(() => {
		visible = true;
	});

	const greeting = (() => {
		const hour = new Date().getHours();
		if (hour < 12) return 'Good morning';
		if (hour < 17) return 'Good afternoon';
		return 'Good evening';
	})();
</script>

{#if visible}
	<div class="mx-auto max-w-7xl px-4 py-8 sm:px-6 lg:px-8">
		<!-- Header -->
		<div in:fly={{ y: 20, duration: 500 }} class="mb-8">
			<h1 class="text-3xl font-bold text-gray-900">
				{greeting}, <span class="bg-gradient-to-r from-indigo-600 to-purple-600 bg-clip-text text-transparent">{data.session?.user?.name || 'User'}</span>
			</h1>
			<p class="mt-2 text-gray-500">Here's what's happening with your application.</p>
		</div>

		<!-- Stats Grid -->
		<div class="mb-8 grid grid-cols-1 gap-6 sm:grid-cols-3">
			<div
				in:fly={{ y: 30, duration: 500, delay: 100 }}
				class="group relative overflow-hidden rounded-2xl bg-white p-6 shadow-sm ring-1 ring-gray-200 transition-all duration-300 hover:-translate-y-1 hover:shadow-lg"
			>
				<div class="absolute -right-4 -top-4 h-24 w-24 rounded-full bg-indigo-50 transition-transform duration-300 group-hover:scale-150"></div>
				<div class="relative">
					<div class="mb-3 flex h-12 w-12 items-center justify-center rounded-xl bg-indigo-100 text-indigo-600 transition-colors group-hover:bg-indigo-600 group-hover:text-white">
						<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197m13.5-9a2.25 2.25 0 11-4.5 0 2.25 2.25 0 014.5 0z" />
						</svg>
					</div>
					<p class="text-sm font-medium text-gray-500">Total Users</p>
					<p class="mt-1 text-3xl font-bold text-gray-900">{data.stats.users}</p>
				</div>
			</div>

			<div
				in:fly={{ y: 30, duration: 500, delay: 200 }}
				class="group relative overflow-hidden rounded-2xl bg-white p-6 shadow-sm ring-1 ring-gray-200 transition-all duration-300 hover:-translate-y-1 hover:shadow-lg"
			>
				<div class="absolute -right-4 -top-4 h-24 w-24 rounded-full bg-green-50 transition-transform duration-300 group-hover:scale-150"></div>
				<div class="relative">
					<div class="mb-3 flex h-12 w-12 items-center justify-center rounded-xl bg-green-100 text-green-600 transition-colors group-hover:bg-green-600 group-hover:text-white">
						<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12.75L11.25 15 15 9.75m-3-7.036A11.959 11.959 0 013.598 6 11.99 11.99 0 003 9.749c0 5.592 3.824 10.29 9 11.623 5.176-1.332 9-6.03 9-11.622 0-1.31-.21-2.571-.598-3.751h-.152c-3.196 0-6.1-1.248-8.25-3.285z" />
						</svg>
					</div>
					<p class="text-sm font-medium text-gray-500">Active Sessions</p>
					<p class="mt-1 text-3xl font-bold text-gray-900">{data.stats.sessions}</p>
				</div>
			</div>

			<div
				in:fly={{ y: 30, duration: 500, delay: 300 }}
				class="group relative overflow-hidden rounded-2xl bg-white p-6 shadow-sm ring-1 ring-gray-200 transition-all duration-300 hover:-translate-y-1 hover:shadow-lg"
			>
				<div class="absolute -right-4 -top-4 h-24 w-24 rounded-full bg-purple-50 transition-transform duration-300 group-hover:scale-150"></div>
				<div class="relative">
					<div class="mb-3 flex h-12 w-12 items-center justify-center rounded-xl bg-purple-100 text-purple-600 transition-colors group-hover:bg-purple-600 group-hover:text-white">
						<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.19 8.688a4.5 4.5 0 011.242 7.244l-4.5 4.5a4.5 4.5 0 01-6.364-6.364l1.757-1.757m9.86-2.556a4.5 4.5 0 00-6.364-6.364L4.757 8.25a4.5 4.5 0 003.182 7.683" />
						</svg>
					</div>
					<p class="text-sm font-medium text-gray-500">Linked Accounts</p>
					<p class="mt-1 text-3xl font-bold text-gray-900">{data.stats.accounts}</p>
				</div>
			</div>
		</div>

		<!-- User Info Card -->
		<div
			in:fly={{ y: 30, duration: 500, delay: 400 }}
			class="rounded-2xl bg-white p-6 shadow-sm ring-1 ring-gray-200"
		>
			<h2 class="mb-4 flex items-center gap-2 text-lg font-semibold text-gray-900">
				<svg class="h-5 w-5 text-indigo-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.75 6a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0zM4.501 20.118a7.5 7.5 0 0114.998 0A17.933 17.933 0 0112 21.75c-2.676 0-5.216-.584-7.499-1.632z" />
				</svg>
				Your Session Info
			</h2>
			<div class="flex items-center gap-4 rounded-xl bg-gray-50 p-4">
				{#if data.session?.user?.image}
					<img
						src={data.session.user.image}
						alt="Profile"
						class="h-16 w-16 rounded-full ring-2 ring-white shadow-sm"
					/>
				{:else}
					<div class="flex h-16 w-16 items-center justify-center rounded-full bg-gradient-to-br from-indigo-500 to-purple-600 text-xl font-bold text-white shadow-sm">
						{(data.session?.user?.name || data.session?.user?.email || '?')[0].toUpperCase()}
					</div>
				{/if}
				<div>
					<p class="font-semibold text-gray-900">{data.session?.user?.name || 'No name set'}</p>
					<p class="text-sm text-gray-500">{data.session?.user?.email}</p>
				</div>
				<a
					href="/profile"
					class="ml-auto rounded-lg border border-gray-200 px-4 py-2 text-sm font-medium text-gray-700 transition hover:bg-gray-100"
				>
					Edit Profile
				</a>
			</div>
		</div>
	</div>
{/if}
