<script>
  import MenuItem from "./skin/MenuItem.svelte";
  import { menuPages } from '../page/skin/page.js';
  import { fly } from 'svelte/transition';

  export let menuItems = menuPages;

  let open = false;

  function toggleMenu() {
    open = !open
  }
</script>

<!-- thanks to Tailwind UI kit -->
<nav class="bg-white shadow dark:bg-gray-900 mb-5">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex items-center justify-between h-16">
      <div class="flex items-center">
        <div class="flex-shrink-0 text-gray-800 dark:text-gray-200">
          <!-- Heroicon name: outline/globe -->
          <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3.055 11H5a2 2 0 012 2v1a2 2 0 002 2 2 2 0 012 2v2.945M8 3.935V5.5A2.5 2.5 0 0010.5 8h.5a2 2 0 012 2 2 2 0 104 0 2 2 0 012-2h1.064M15 20.488V18a2 2 0 012-2h3.064M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
        </div>
        <div class="hidden md:block">
          <!-- todo make this customisable for other views/templates -->
          <div class="ml-10 flex items-baseline space-x-4">
            {#each menuItems as item}
              <svelte:component this={MenuItem} {...item} />
            {/each}
          </div>
        </div>
      </div>
      <div class="hidden md:block">
        <div class="ml-4 flex items-center md:ml-6">
          <button class="p-1 rounded-full text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-gray-800 focus:ring-white">
            <span class="sr-only">View notifications</span>
            <!-- Heroicon name: outline/bell -->
            <svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"/>
            </svg>
          </button>
          <!-- Profile dropdown -->
          <div class="ml-3 relative">
            <div>
              <button on:click={toggleMenu} type="button" class="max-w-xs bg-gray-800 rounded-full flex items-center text-sm focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-gray-800 focus:ring-white" id="user-menu-button" aria-expanded="false" aria-haspopup="true">
                <span class="sr-only">Open user menu</span>
                <!-- todo probably replace with an user icon -->
                <img class="h-8 w-8 rounded-full" src="https://geysermc.org/favicon.ico" alt="">
              </button>
            </div>
            <div class="bg-white dark:bg-gray-800 hidden transition-all opacity-0 ease-in-out duration-150 origin-top-right absolute right-0 mt-2 z-20 w-48 rounded-md shadow-lg py-1 ring-1 ring-black ring-opacity-5 focus:outline-none" id="user-menu-dropdown" role="menu" aria-orientation="vertical" aria-labelledby="user-menu-button" tabindex="-1">
              <!-- Active: "bg-gray-100", Not active: "" -->
              <a href="#" class="block px-4 py-2 text-sm text-gray-700 dark:text-gray-100" role="menuitem" tabindex="-1" id="user-menu-item-0">Your Profile</a>
              <a href="#" class="block px-4 py-2 text-sm text-gray-700 dark:text-gray-100" role="menuitem" tabindex="-1" id="user-menu-item-1">Settings</a>
              <a href="#" class="block px-4 py-2 text-sm text-gray-700 dark:text-gray-100" role="menuitem" tabindex="-1" id="user-menu-item-2">Sign out</a>
            </div>
          </div>
        </div>
      </div>
      <div class="-mr-2 flex md:hidden">
        <!-- Mobile menu button -->
        <button type="button" on:click={toggleMenu} class="text-gray-500 dark:text-gray-400 dark:bg-gray-800 dark:hover:text-white hover:bg-gray-200 dark:hover:bg-gray-700 inline-flex items-center justify-center p-2 rounded-md focus:outline-none focus:ring-2 focus:ring-ffset-2 focus:ring-offset-gray-800 focus:ring-white" aria-controls="mobile-menu" aria-expanded="false">
          <span class="sr-only">Open main menu</span>
          <!-- Heroicon name: outline/menu -->
          <!-- Menu open: hidden, closed: block -->
          <svg class="block h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/>
          </svg>
          <!-- Heroicon name: outline/x -->
          <!-- Menu open: block, closed: hidden -->
          <svg class="hidden h-6 w-6 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
          </svg>
        </button>
      </div>
    </div>
  </div>
  <!-- Mobile menu, show/hide based on menu state -->
  {#if open}
    <div transition:fly={{y: -1_000, duration: 200}} class="md:hidden" id="mobile-menu">
      <div class="px-2 pt-2 pb-3 space-y-0.5 sm:px-3">
        {#each menuItems as item}
          <svelte:component this={MenuItem} {...item} />
        {/each}
      </div>
      <div class="pt-4 pb-3 border-t border-gray-200 dark:border-gray-700">
        <div class="flex items-center px-5">
          <div class="flex-shrink-0">
            <img class="h-9 w-9 rounded-full" src="https://geysermc.org/favicon.ico" alt="">
          </div>
          <div class="ml-3">
            <div class="text-sm font-medium leading-none text-gray-900 dark:text-white">Tom Cook</div>
            <div class="text-sm font-medium leading-none text-gray-600 dark:text-gray-400">tom@example.com</div>
          </div>
          <button class="ml-auto flex-shrink-0 p-1 rounded-full text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-gray-800 focus:ring-white">
            <span class="sr-only">View notifications</span>
            <!-- Heroicon name: outline-bell -->
            <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"/>
            </svg>
          </button>
        </div>
        <div class="mt-4 px-2 space-y-0.5">
          <a href="#" class="text-gray-600 dark:text-gray-400 hover:bg-gray-300 dark:hover:bg-gray-800 transition-colors duration-200 transform dark:hover:text-white block px-3 py-2 rounded-md text-sm font-medium">Your Profile</a>
          <a href="#" class="text-gray-600 dark:text-gray-400 hover:bg-gray-300 dark:hover:bg-gray-800 transition-colors duration-200 transform dark:hover:text-white block px-3 py-2 rounded-md text-sm font-medium">Settings</a>
          <a href="#" class="text-gray-600 dark:text-gray-400 hover:bg-gray-300 dark:hover:bg-gray-800 transition-colors duration-200 transform dark:hover:text-white block px-3 py-2 rounded-md text-sm font-medium">Sign out</a>
        </div>
      </div>
    </div>
  {/if}
</nav>