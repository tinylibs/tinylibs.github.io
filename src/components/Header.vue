<template>
  <header>
    <a href="/"><img class="logo" src="logo.png" alt="Logo" /></a>
    <div class="title-block">
      <h1><a href="/">Tinylibs</a></h1>
      <p>
        <span>A place for tiny and minimal libraries</span>
        <a href="https://github.com/tinylibs"
          ><img class="github" src="github.svg"
        /></a>
      </p>
    </div>
  </header>
</template>

<style scoped>
header {
  position: sticky;
  top: 0px;
  padding: 8px 0;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 16px;
  width: 100%;
  background-color: hsl(var(--yellow));
  z-index: 1;
}
.logo {
  height: 100%;
  aspect-ratio: 1;
}
.title-block {
  display: flex;
  flex-direction: column;
  color: hsl(var(--black));
}
h1 {
  font-size: 24px;
  line-height: 1;
  font-weight: 900;
}
a {
  text-decoration: none;
}
p {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
}
.github {
  vertical-align: sub;
  width: 18px;
  height: 18px;
}

@media (min-width: 767px) {
  .title-block {
    gap: 20px;
  }
  .logo {
    width: 64px;
    height: 64px;
  }
  h1 {
    font-size: 32px;
  }
  p {
    font-size: 16px;
  }
  .github {
    width: 20px;
    height: 20px;
  }
}
</style>
