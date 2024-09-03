<template>
  <header class="header">
    <div class="header-logo">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z" fill="#1a73e8"/>
      </svg>
      {{ siteName }}
    </div>
    <nav class="header-nav">
      <ul>
        <li>Site</li>
        <li>Explorer</li>
        <li>Github</li>
        <li>Twitter</li>
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
