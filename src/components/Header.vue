<template>
  <header class="header">
    <div class="header-logo">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M4 12H8V20H4V12ZM10 8H14V20H10V8ZM16 4H20V20H16V4Z" fill="#1a73e8"/>
      </svg>
      {{ siteName }}
    </div>
    <nav class="header-nav">
      <ul>
        <li><a href="https://aaronetwork.xyz/" target="_blank">Site</a></li>
        <li><a href="https://explorer.aaronetwork.xyz/" target="_blank">Explorer</a></li>
        <li><a href="https://github.com/aaronetwork" target="_blank">Github</a></li>
        <li><a href="https://x.com/aaronetwork" target="_blank">Twitter</a></li>
      </ul>
    </nav>
  </header>
</template>

<script>
export default {
  data() {
    return {
      siteName: import.meta.env.VITE_SITE_NAME
    };
  }
};
</script>

<style scoped>

</style>
