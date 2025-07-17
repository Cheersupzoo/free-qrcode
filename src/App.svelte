<script lang="ts">
  import { onMount } from 'svelte';
  import Home from './routes/Home.svelte';
  import Generate from './routes/Generate.svelte';

  let currentRoute = $state('home');
  let params = $state<Record<string, string>>({});

  function parseRoute() {
    const hash = window.location.hash.slice(1);
    if (!hash) {
      currentRoute = 'home';
      params = {};
      return;
    }

    const [route, queryString] = hash.split('?');
    currentRoute = route || 'home';
    
    if (queryString) {
      params = Object.fromEntries(new URLSearchParams(queryString));
    } else {
      params = {};
    }
  }

  function navigate(route: string, searchParams: Record<string, string> = {}) {
    const query = new URLSearchParams(searchParams).toString();
    const hash = query ? `#${route}?${query}` : `#${route}`;
    window.location.hash = hash;
  }

  onMount(() => {
    parseRoute();
    
    window.addEventListener('hashchange', parseRoute);
    
    return () => {
      window.removeEventListener('hashchange', parseRoute);
    };
  });
</script>

<main>
  {#if currentRoute === 'home'}
    <Home {navigate} />
  {:else if currentRoute === 'generate'}
    <Generate {params} {navigate} />
  {/if}
</main>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
      'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
      sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    background-color: white;
    color: #1e293b;
    line-height: 1.6;
  }

  :global(*) {
    box-sizing: border-box;
  }

  main {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }
</style>