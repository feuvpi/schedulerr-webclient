<script lang="ts">
	import { fade } from 'svelte/transition';
	import { clickoutside } from '@svelte-put/clickoutside';
	import ButtonDropdown from '../ButtonDropdown.svelte';
	import type { Link } from '$lib/interfaces/link';
	import { openSidebar, sidebarOpen } from '$lib/store';
	import profile from '$lib/public/9.jpg';

	interface DashboardHeaderProps {
		toggleTheme: () => void;
		isDarkTheme: boolean;
		isNotificationsMenuOpen: boolean;
		isProfileMenuOpen: boolean;
	}

	const links: Link[] = [
		{
			name: 'Profile',
			path: '/profile',
			icon: {
				inactive: 'bx:user',
				active: 'bxs:user'
			}
		},
		{
			name: 'Settings',
			path: '/settings',
			icon: {
				inactive: 'basil:settings-outline',
				active: 'basil:settings'
			}
		}
	];

	export let props: DashboardHeaderProps;
	export let isSideMenuOpen: boolean;

	let isOpenNotifications = false;
	let isOpenDropdown = false;

	const handleState = (el: 'dropdown' | 'notifications') => {
		if (el == 'dropdown') {
			isOpenDropdown = !isOpenDropdown;
			isOpenNotifications = false;
		} else {
			isOpenNotifications = !isOpenNotifications;
			isOpenDropdown = false;
		}
	};
</script>

<header
	class="z-10 border-gray-200 bg-indigo-300 p-4 dark:border-gray-600 dark:bg-indigo-800 shadow-md dark:shadow-none top-0 w-full min-w-max"
>
	<div
		class="dark:text-dark-text flex h-full items-center justify-between text-indigo-600 dark:text-indigo-300"
	>
		<!-- Mobile hamburger -->
		<button
			id="sidebar-button"
			type="button"
			aria-expanded="false"
			aria-label="Toggle sidenav"
			on:click={openSidebar}
			class="text-4xl pr-4 focus:outline-none lg:visible"
		>
			<svg
				class="w-4 h-4 text-gray-600 dark:text-white"
				class:is-open={$sidebarOpen}
				class:transform={$sidebarOpen}
				class:transition-transform={$sidebarOpen}
				class:rotate-180={$sidebarOpen}
				xmlns="http://www.w3.org/2000/svg"
				fill="none"
				viewBox="0 0 24 24"
				stroke="currentColor"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M13 5l7 7-7 7M5 5l7 7-7 7"
				/>
			</svg>
		</button>

		<ul class="flex flex-shrink-0 items-center space-x-6">
			<!-- Theme toggler -->
			<li class="flex text-indigo-500 dark:text-indigo-300">
				<button
					class="rounded-md focus:outline-none focus:ring-[3px] focus:ring-orange-200 dark:focus:ring-gray-500"
					on:click={props.toggleTheme}
					aria-label="Toggle color mode"
				>
					{#if props.isDarkTheme}
						<svg class="h-5 w-5" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20">
							<path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z" />
						</svg>
					{/if}
					{#if !props.isDarkTheme}
						<svg class="h-5 w-5" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20">
							<path
								fill-rule="evenodd"
								d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z"
								clip-rule="evenodd"
							/>
						</svg>
					{/if}
				</button>
			</li>
			<!-- Notifications menu -->
			<li class="relative text-light-text dark:text-dark-text">
				<button
					use:clickoutside
					on:clickoutside={() => {
						if (props.isNotificationsMenuOpen) props.isNotificationsMenuOpen = false;
					}}
					class="relative rounded-md align-middle focus:outline-none focus:ring-[3px] focus:ring-orange-200 dark:focus:ring-gray-500"
					on:click={() => {
						props.isNotificationsMenuOpen = !props.isNotificationsMenuOpen;
					}}
					aria-label="Notifications"
					aria-haspopup="true"
				>
					<svg class="h-5 w-5" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20">
						<path
							d="M10 2a6 6 0 00-6 6v3.586l-.707.707A1 1 0 004 14h12a1 1 0 00.707-1.707L16 11.586V8a6 6 0 00-6-6zM10 18a3 3 0 01-3-3h6a3 3 0 01-3 3z"
						/>
					</svg>
					<!-- Notification badge -->
					<span
						aria-hidden="true"
						class="absolute top-0 right-0 inline-block h-3 w-3 translate-x-1 -translate-y-1 transform rounded-full border-2 border-white bg-red-600 dark:border-gray-800"
					/>
				</button>
				{#if props.isNotificationsMenuOpen}
					<ul
						transition:fade|global={{ duration: 150 }}
						class="absolute right-0 mt-2 w-56 space-y-2 rounded-md border border-gray-100 bg-white p-2 text-gray-600 shadow-md dark:border-gray-700 dark:bg-gray-700 dark:text-gray-300"
					>
						<li class="flex">
							<!-- svelte-ignore a11y-invalid-attribute -->
							<a
								class="inline-flex w-full items-center justify-between rounded-md px-2 py-1 text-sm font-semibold transition-colors duration-150 hover:bg-gray-100 hover:text-gray-800 dark:hover:bg-gray-800 dark:hover:text-gray-200"
								href=""
							>
								<span>Messages</span>
								<span
									class="inline-flex items-center justify-center rounded-full bg-red-100 px-2 py-1 text-xs font-bold leading-none text-red-600 dark:bg-red-600 dark:text-red-100"
								>
									13
								</span>
							</a>
						</li>
						<li class="flex">
							<!-- svelte-ignore a11y-invalid-attribute -->
							<a
								class="inline-flex w-full items-center justify-between rounded-md px-2 py-1 text-sm font-semibold transition-colors duration-150 hover:bg-gray-100 hover:text-gray-800 dark:hover:bg-gray-800 dark:hover:text-gray-200"
								href=""
							>
								<span>Sales</span>
								<span
									class="inline-flex items-center justify-center rounded-full bg-red-100 px-2 py-1 text-xs font-bold leading-none text-red-600 dark:bg-red-600 dark:text-red-100"
								>
									2
								</span>
							</a>
						</li>
						<li class="flex">
							<!-- svelte-ignore a11y-invalid-attribute -->
							<a
								class="inline-flex w-full items-center justify-between rounded-md px-2 py-1 text-sm font-semibold transition-colors duration-150 hover:bg-gray-100 hover:text-gray-800 dark:hover:bg-gray-800 dark:hover:text-gray-200"
								href=""
							>
								<span>Alerts</span>
							</a>
						</li>
					</ul>
				{/if}
			</li>
			<!-- Profile menu -->

			<ButtonDropdown on:click={() => handleState('dropdown')} {links} bind:isOpen={isOpenDropdown}>
				<img src={profile} class="w-6" alt="Username" />
			</ButtonDropdown>
		</ul>
	</div>
</header>
