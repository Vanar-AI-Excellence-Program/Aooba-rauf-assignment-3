<script lang="ts">
	import { enhance } from '$app/forms';
	import { fly } from 'svelte/transition';

	let { data, form } = $props();
	let profileLoading = $state(false);
	let passwordLoading = $state(false);
	let visible = $state(false);

	$effect(() => {
		visible = true;
	});
</script>

{#if visible}
	<div class="mx-auto max-w-2xl px-4 py-8 sm:px-6 lg:px-8">
		<h1 in:fly={{ y: 20, duration: 500 }} class="mb-8 text-3xl font-bold text-gray-900">Profile</h1>

		<!-- Profile Info Card -->
		<div in:fly={{ y: 30, duration: 500, delay: 100 }} class="rounded-2xl bg-white p-6 shadow-sm ring-1 ring-gray-200">
			<div class="mb-6 flex items-center gap-4">
				{#if data.user.image}
					<img
						src={data.user.image}
						alt="Profile"
						class="h-20 w-20 rounded-full ring-4 ring-indigo-100 shadow-sm"
					/>
				{:else}
					<div class="flex h-20 w-20 items-center justify-center rounded-full bg-gradient-to-br from-indigo-500 to-purple-600 text-2xl font-bold text-white shadow-sm">
						{(data.user.name || data.user.email || '?')[0].toUpperCase()}
					</div>
				{/if}
				<div>
					<p class="text-lg font-semibold text-gray-900">{data.user.name || 'No name set'}</p>
					<p class="text-gray-500">{data.user.email}</p>
				</div>
			</div>

			{#if form?.success}
				<div class="mb-6 flex items-center gap-2 rounded-lg bg-green-50 p-4 text-sm text-green-600">
					<svg class="h-5 w-5 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
					</svg>
					Profile updated successfully!
				</div>
			{/if}

			{#if form?.error}
				<div class="mb-6 flex items-center gap-2 rounded-lg bg-red-50 p-4 text-sm text-red-600">
					<svg class="h-5 w-5 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v3.75m9-.75a9 9 0 11-18 0 9 9 0 0118 0zm-9 3.75h.008v.008H12v-.008z" />
					</svg>
					{form.error}
				</div>
			{/if}

			<form
				method="POST"
				action="?/updateProfile"
				use:enhance={() => {
					profileLoading = true;
					return async ({ update }) => {
						profileLoading = false;
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
						value={data.user.name ?? ''}
						class="w-full rounded-lg border border-gray-300 px-4 py-2.5 text-gray-900 transition focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
					/>
				</div>
				<div>
					<label for="email" class="mb-1 block text-sm font-medium text-gray-700">Email</label>
					<input
						id="email"
						name="email"
						type="email"
						required
						value={data.user.email}
						class="w-full rounded-lg border border-gray-300 px-4 py-2.5 text-gray-900 transition focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
					/>
				</div>
				<button
					type="submit"
					disabled={profileLoading}
					class="inline-flex items-center gap-2 rounded-lg bg-indigo-600 px-6 py-2.5 font-semibold text-white shadow-sm transition hover:bg-indigo-700 hover:shadow-md disabled:opacity-50"
				>
					{#if profileLoading}
						<svg class="h-4 w-4 animate-spin" fill="none" viewBox="0 0 24 24">
							<circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
							<path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"></path>
						</svg>
						Saving...
					{:else}
						Save Changes
					{/if}
				</button>
			</form>
		</div>

		<!-- Change Password Card -->
		<div in:fly={{ y: 30, duration: 500, delay: 200 }} class="mt-8 rounded-2xl bg-white p-6 shadow-sm ring-1 ring-gray-200">
			<h2 class="mb-5 flex items-center gap-2 text-xl font-semibold text-gray-900">
				<svg class="h-5 w-5 text-indigo-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16.5 10.5V6.75a4.5 4.5 0 10-9 0v3.75m-.75 11.25h10.5a2.25 2.25 0 002.25-2.25v-6.75a2.25 2.25 0 00-2.25-2.25H6.75a2.25 2.25 0 00-2.25 2.25v6.75a2.25 2.25 0 002.25 2.25z" />
				</svg>
				Change Password
			</h2>

			{#if form?.passwordSuccess}
				<div class="mb-6 flex items-center gap-2 rounded-lg bg-green-50 p-4 text-sm text-green-600">
					<svg class="h-5 w-5 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
					</svg>
					Password changed successfully!
				</div>
			{/if}

			{#if form?.passwordError}
				<div class="mb-6 flex items-center gap-2 rounded-lg bg-red-50 p-4 text-sm text-red-600">
					<svg class="h-5 w-5 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v3.75m9-.75a9 9 0 11-18 0 9 9 0 0118 0zm-9 3.75h.008v.008H12v-.008z" />
					</svg>
					{form.passwordError}
				</div>
			{/if}

			<form
				method="POST"
				action="?/changePassword"
				use:enhance={() => {
					passwordLoading = true;
					return async ({ update }) => {
						passwordLoading = false;
						await update();
					};
				}}
				class="space-y-5"
			>
				<div>
					<label for="currentPassword" class="mb-1 block text-sm font-medium text-gray-700">Current Password</label>
					<input
						id="currentPassword"
						name="currentPassword"
						type="password"
						required
						placeholder="Enter current password"
						class="w-full rounded-lg border border-gray-300 px-4 py-2.5 text-gray-900 transition focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
					/>
				</div>
				<div>
					<label for="newPassword" class="mb-1 block text-sm font-medium text-gray-700">New Password</label>
					<input
						id="newPassword"
						name="newPassword"
						type="password"
						required
						minlength={8}
						placeholder="At least 8 characters"
						class="w-full rounded-lg border border-gray-300 px-4 py-2.5 text-gray-900 transition focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
					/>
				</div>
				<div>
					<label for="confirmPassword" class="mb-1 block text-sm font-medium text-gray-700">Confirm New Password</label>
					<input
						id="confirmPassword"
						name="confirmPassword"
						type="password"
						required
						minlength={8}
						placeholder="Repeat new password"
						class="w-full rounded-lg border border-gray-300 px-4 py-2.5 text-gray-900 transition focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
					/>
				</div>
				<button
					type="submit"
					disabled={passwordLoading}
					class="inline-flex items-center gap-2 rounded-lg bg-indigo-600 px-6 py-2.5 font-semibold text-white shadow-sm transition hover:bg-indigo-700 hover:shadow-md disabled:opacity-50"
				>
					{#if passwordLoading}
						<svg class="h-4 w-4 animate-spin" fill="none" viewBox="0 0 24 24">
							<circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
							<path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"></path>
						</svg>
						Changing...
					{:else}
						Change Password
					{/if}
				</button>
			</form>
		</div>
	</div>
{/if}
