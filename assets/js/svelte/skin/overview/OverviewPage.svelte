<script>
  import SkinEntry from './SkinEntry.svelte';
  import ActionButton from './ActionButton.svelte';
  import PageButton from './PageButton.svelte';
  import { pageFE, currentPage, totalPages } from './page.js';
  import { createNotification } from '../../../notification.js';

  export let skinsPerPage;
  export let fetchUrl;
  export let description;
  export let spinner;

  let lastSwitch = 0;

  function usePlaceholders() {
    $pageFE = Array(skinsPerPage).fill({loading: true})
  }

  export function switchPage(newPage, switchStart = undefined) {
    spinner.show()
    $currentPage = newPage;
    if (switchStart == undefined) {
      lastSwitch = switchStart = Date.now();
    }

    usePlaceholders()

    const url = new URL(window.location);
    url.searchParams.set("page", newPage);
    window.history.pushState({}, "", url);

    fetch(fetchUrl + "?page=" + newPage)
      .then(res => res.json())
      .then(json => {
        if (json.message) {
          console.log(json.message)
        }

        let data = json.data;
        if (!data || data.length === 0) {
          throw new Error("Empty response?? " + json)
        }

        // make sure that we won't override when there has been a more recent page switch
        if (lastSwitch == switchStart) {
          $pageFE = data;
          $totalPages = json.total_pages;
          spinner.hide()
        }
      })
      .catch(err => {
        console.log(err)
        scheduleRetry()
      })
  }

  function scheduleRetry() {
    createNotification("Failed to get skins", "We'll try it again in 30 seconds!", false, null, null, 10_000)
    let start = Date.now();
    let page = $currentPage;
    setTimeout(async () => {
      switchPage(page, start)
    }, 30_000)
  }

  usePlaceholders()
</script>

<header class="bg-white text-gray-900 dark:bg-gray-700 dark:text-gray-200 shadow">
  <div class="max-w-7xl mx-auto py-6 px-4 sm:px-6 lg:px-8">
    <h1 class="text-3xl font-bold text-center">
      {description}
    </h1>
  </div>
</header>

<div class="flex justify-center mt-10">
  <div class="grid 2xl:grid-cols-7 xl:grid-cols-6 lg:grid-cols-5 md:grid-cols-4 sm:grid-cols-4 grid-cols-2 gap-4 justify-between">
    {#each $pageFE as item}
      <svelte:component this={SkinEntry} {...item} />
    {/each}
  </div>
</div>

<div class="flex w-full justify-center items-center text-gray-800">
  <div class="flex h-10 mt-8 justify-center items-center w-2/4 gap-1.5">
    <svelte:component this={ActionButton} clickAction={-1} svgPathData="M10 19l-7-7m0 0l7-7m-7 7h18" {switchPage} />
    <svelte:component this={PageButton} pageNumber={1} onClick={switchPage} />
    <svelte:component this={PageButton} pageNumber={$totalPages / 2} onClick={switchPage} />
    <svelte:component this={PageButton} pageNumber={$totalPages} onClick={switchPage} />
    <svelte:component this={ActionButton} clickAction={1} svgPathData="M14 5l7 7m0 0l-7 7m7-7H3" {switchPage} />
  </div>
</div>
