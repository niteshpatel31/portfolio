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
      // Default to dark (or use prefers-color-scheme if you want)
      isDark = true;
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
  :global(html) {
    transition:
      background-color 0.3s ease,
      color 0.3s ease;
  }

  :global(.dark) {
    background-color: #000000;
    color: #e0e0e0;
  }

  :global(.light) {
    background-color: #ffffff;
    color: #000000;
  }

  :global(.dark a) {
    color: #66b3ff;
  }

  :global(.light a) {
    color: #0066cc;
  }

  :global(.dark hr) {
    border-top: 1px solid #333;
  }

  :global(.light hr) {
    border-top: 1px solid #ccc;
  }

  /* Base styles (shared) */
  :global(body) {
    font-family: monospace;
    margin: 0;
    padding: 40px 20px;
    line-height: 1.7;
  }

  :global(main) {
    max-width: 800px;
    margin: 0 auto;
    position: relative;
  }

  .theme-toggle {
    position: fixed;
    top: 20px;
    right: 20px;
    background: none;
    border: none;
    font-size: 1.8rem;
    cursor: pointer;
    padding: 8px;
    z-index: 100;
    opacity: 0.7;
    transition: opacity 0.2s;
  }

  .theme-toggle:hover {
    opacity: 1;
  }

  /* Ensure initial theme is applied */
  :global(html.dark) {
    background-color: #000;
    color: #e0e0e0;
  }
  :global(html.light) {
    background-color: #fff;
    color: #000;
  }
</style>
