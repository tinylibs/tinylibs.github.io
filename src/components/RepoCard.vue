<template>
  <article @click="$emit('selected', repoName)">
    <h2>{{ repoName }}</h2>
    <p>{{ repoDescription }}</p>
    <p>{{ language }}</p>
    <p><font-awesome-icon icon="fa-solid fa-star" /> {{ starCount }} <font-awesome-icon icon="fa-solid fa-code-fork" /> {{ forkCount }}</p>
    <p>Created: {{ dateCreated | formatDate }}</p>
  </article>
</template>

<script>
export default {
  name: 'RepoCard',
  props: {
    repoName: String,
    repoDescription: String,
    language: String,
    starCount: Number,
    forkCount: Number,
    dateCreated: String,
  }
}
</script>

<style scoped>
  article {
    border: 1px solid #000;
    width: 30vw;
    cursor: pointer;
  }
</style>
