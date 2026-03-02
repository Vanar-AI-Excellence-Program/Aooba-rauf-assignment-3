<script lang="ts">
	import { enhance } from '$app/forms';
	import { fly } from 'svelte/transition';
	import OAuthButtons from '$lib/components/OAuthButtons.svelte';

	let { form } = $props();
	let loading = $state(false);
	let visible = $state(false);

	$effect(() => {
		visible = true;
	});
</script>

<div class="flex min-h-[calc(100vh-64px)] items-center justify-center px-4 py-12">
	{#if visible}
		<div in:fly={{ y: 30, duration: 600 }} class="w-full max-w-md">
			<div class="rounded-2xl bg-white p-8 shadow-xl ring-1 ring-gray-100">
				<!-- Header -->
				<div class="mb-8 text-center">
					<div class="mx-auto mb-4 flex h-14 w-14 items-center justify-center rounded-2xl bg-indigo-100">
						<svg class="h-7 w-7 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7.5v3m0 0v3m0-3h3m-3 0h-3m-2.25-4.125a3.375 3.375 0 11-6.75 0 3.375 3.375 0 016.75 0zM4 19.235v-.11a6.375 6.375 0 0112.75 0v.109A12.318 12.318 0 0110.374 21c-2.331 0-4.512-.645-6.374-1.766z" />
						</svg>
					</div>
					<h2 class="text-2xl font-bold text-gray-900">Create an account</h2>
					<p class="mt-1 text-gray-500">Get started with AuthKit</p>
				</div>

				{#if form?.error}
					<div in:fly={{ y: -10, duration: 300 }} class="mb-6 flex items-center gap-2 rounded-lg bg-red-50 p-4 text-sm text-red-600">
						<svg class="h-5 w-5 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v3.75m9-.75a9 9 0 11-18 0 9 9 0 0118 0zm-9 3.75h.008v.008H12v-.008z" />
						</svg>
						{form.error}
					</div>
				{/if}

				<form
					method="POST"
					use:enhance={() => {
						loading = true;
						return async ({ update }) => {
							loading = false;
							await update();
						};
					}}
					class="space-y-5"
				>
					<div>
						<label for="name" class="mb-1 block text-sm font-medium text-gray-700">Full Name</label>
						<input
							id="name"
							name="name"
							type="text"
							required
							value={form?.name ?? ''}
							class="w-full rounded-lg border border-gray-300 px-4 py-2.5 text-gray-900 transition focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
							placeholder="John Doe"
						/>
					</div>
					<div>
						<label for="email" class="mb-1 block text-sm font-medium text-gray-700">Email</label>
						<input
							id="email"
							name="email"
							type="email"
							required
							value={form?.email ?? ''}
							class="w-full rounded-lg border border-gray-300 px-4 py-2.5 text-gray-900 transition focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
							placeholder="you@example.com"
						/>
					</div>
					<div>
						<label for="password" class="mb-1 block text-sm font-medium text-gray-700">
							Password
						</label>
						<input
							id="password"
							name="password"
							type="password"
							required
							minlength="6"
							class="w-full rounded-lg border border-gray-300 px-4 py-2.5 text-gray-900 transition focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
							placeholder="At least 6 characters"
						/>
					</div>
					<div>
						<label for="confirmPassword" class="mb-1 block text-sm font-medium text-gray-700">
							Confirm Password
						</label>
						<input
							id="confirmPassword"
							name="confirmPassword"
							type="password"
							required
							minlength="6"
							class="w-full rounded-lg border border-gray-300 px-4 py-2.5 text-gray-900 transition focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
							placeholder="Repeat your password"
						/>
					</div>
					<button
						type="submit"
						disabled={loading}
						class="flex w-full items-center justify-center gap-2 rounded-lg bg-indigo-600 px-4 py-2.5 font-semibold text-white shadow-sm transition hover:bg-indigo-700 hover:shadow-md disabled:opacity-50"
					>
						{#if loading}
							<svg class="h-4 w-4 animate-spin" fill="none" viewBox="0 0 24 24">
								<circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
								<path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"></path>
							</svg>
							Creating account...
						{:else}
							Create Account
						{/if}
					</button>
				</form>

				<div class="my-6 flex items-center gap-4">
					<div class="h-px flex-1 bg-gray-200"></div>
					<span class="text-xs font-medium uppercase tracking-wider text-gray-400">or continue with</span>
					<div class="h-px flex-1 bg-gray-200"></div>
				</div>

				<OAuthButtons />

				<p class="mt-6 text-center text-sm text-gray-500">
					Already have an account?
					<a href="/login" class="font-semibold text-indigo-600 transition hover:text-indigo-500">Sign in</a>
				</p>
			</div>
		</div>
	{/if}
</div>
