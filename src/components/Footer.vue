<template>
  <footer>
    <p>Made with ❤️</p>
    <!-- prettier-ignore -->
    <p><a href="https://github.com/tinylibs/tinylibs.github.io">View source on GitHub</a></p>
  </footer>
</template>

<style scoped>
footer {
  position: sticky;
  bottom: 0;
  padding: 8px 0;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 16px;
  width: 100%;
  z-index: 1;
}
a:hover {
  text-decoration: underline;
}
</style>
