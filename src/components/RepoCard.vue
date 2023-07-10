<template>
  <a :href="url">
    <article>
      <h2>{{ name }}</h2>
      <p>{{ description }}</p>
      <span>⭐ {{ stars }}</span>
    </article>
  </a>
</template>

<script lang="ts">
export default {
  props: {
    repo: String,
    name: String,
    description: String,
    stars: Number,
  },
  computed: {
    url() {
      return `https://github.com/${this.repo}`;
    },
  },
};
</script>

<style scoped>
article {
  position: relative;
  padding: 1.25rem;
  border: 1px dashed ButtonBorder;
  transition: background var(--easing), color var(--easing);
}
article:hover {
  background-color: ButtonFace;
  color: ButtonText;
}
h2 {
  font-size: 1.25rem;
  line-height: 1.2;
}
p {
  margin-top: 0.5rem;
}
span {
  position: absolute;
  display: flex;
  right: 20px;
  top: 20px;
  align-items: center;
  gap: 4px;
  font-size: 16px;
  padding: 4px;
}
</style>
