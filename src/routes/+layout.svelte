<script lang="ts">
  import { onMount } from "svelte";

  // Reactive variable for current theme
  let isDark = true;

  // Load preference on mount
  onMount(() => {
    const saved = localStorage.getItem("theme");
    if (saved) {
      isDark = saved === "dark";
    } else {
      isDark = window.matchMedia("(prefers-color-scheme: dark)").matches;
    }
    applyTheme();
  });

  // Toggle function
  function toggleTheme() {
    isDark = !isDark;
    localStorage.setItem("theme", isDark ? "dark" : "light");
    applyTheme();
  }

  // Apply classes and styles based on theme
  function applyTheme() {
    if (isDark) {
      document.documentElement.classList.add("dark");
      document.documentElement.classList.remove("light");
    } else {
      document.documentElement.classList.add("light");
      document.documentElement.classList.remove("dark");
    }
  }
</script>

<!-- Simple toggle button in the top-right corner -->
<button
  on:click={toggleTheme}
  aria-label="Toggle dark/light mode"
  class="theme-toggle"
>
  {#if isDark}☀️{:else}🌙{/if}
</button>

<main>
  <slot />
</main>

<style>
  :global(:root) {
    --bg-color: #FFFFFF;
    --text-color: #111111;
    --text-muted: #666666;
    --link-color: #0055FF;
    --link-hover: #0033CC;
  }

  :global(.dark) {
    --bg-color: #0A0A0A;
    --text-color: #F0F0F0;
    --text-muted: #888888;
    --link-color: #3399FF;
    --link-hover: #66B3FF;
  }

  :global(html) {
    transition: background-color 0.3s ease, color 0.3s ease;
    background-color: var(--bg-color);
    color: var(--text-color);
  }

  /* Base styles (shared) */
  :global(body) {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
    margin: 0;
    padding: 60px 20px;
    line-height: 1.7;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  :global(main) {
    max-width: 680px;
    margin: 0 auto;
    position: relative;
  }

  :global(h1, h2, h3, h4, h5, h6) {
    font-weight: 600;
    color: var(--text-color);
    margin-top: 2rem;
    margin-bottom: 1rem;
  }

  :global(p) {
    margin-bottom: 1.2rem;
  }

  :global(a) {
    color: var(--link-color);
    text-decoration: none;
    transition: color 0.2s ease;
  }

  :global(a:hover) {
    color: var(--link-hover);
    text-decoration: underline;
  }

  :global(strong) {
    font-weight: 600;
  }

  .theme-toggle {
    position: fixed;
    top: 20px;
    right: 20px;
    background: none;
    border: none;
    font-size: 1.5rem;
    cursor: pointer;
    padding: 8px;
    z-index: 100;
    opacity: 0.6;
    transition: opacity 0.2s;
  }

  .theme-toggle:hover {
    opacity: 1;
  }

  /* Ensure initial theme is applied correctly during SSR/early load */
  :global(html.dark) {
    background-color: #0A0A0A;
    color: #F0F0F0;
  }
  :global(html.light) {
    background-color: #FFFFFF;
    color: #111111;
  }
</style>
