<template>
  <main v-if="repos">
    <RepoCard
      v-for="repo in repos"
      :key="repo.repo"
      :repo="repo.repo"
      :name="repo.name"
      :description="repo.description"
      :stars="repo.stars"
    />
  </main>
  <div v-else class="loader">
    <Loader />
  </div>
</template>

<script lang="ts">
import RepoCard from "./RepoCard.vue";
import Loader from "./Loader.vue";
export default {
  components: {
    RepoCard,
    Loader,
  },
  data() {
    return {
      repos: null,
    };
  },
  methods: {
    async fetchRepos() {
      const response = await fetch("https://ungh.cc/orgs/tinylibs/repos");
      const json = await response.json();
      let { repos } = json;
      const hidden = ["tinylibs.github.io", ".github", "tinybench.old"];
      repos = repos.filter((x) => !hidden.includes(x.name));
      this.repos = repos;
    },
  },
  mounted() {
    this.fetchRepos();
  },
};
</script>

<style scoped>
main {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}
@media (min-width: 767px) {
  main {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}
@media (min-width: 1024px) {
  main {
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }
}
</style>
