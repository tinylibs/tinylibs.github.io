<template>
  <article @click="$emit('selected', repoName)">
    <h2>{{ repoName }}</h2>
    <p>{{ repoDescription }}</p>
    <p>{{ language }}</p>
    <span><font-awesome-icon icon="fa-solid fa-star" /> {{ starCount }}</span><span> <font-awesome-icon icon="fa-solid fa-code-fork" /> {{ forkCount }}</span>
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
    border: 1px solid #d0d0d0;
    background-color: #fff;
    cursor: pointer;
    overflow: auto;
    border-radius: 8px;
    box-shadow: 0 0 20px 1px #d0d0d0;
    text-align: left;
    padding: 1rem;
  }

  h2 {
    font-size: 1.5em;
  }

  p:first-of-type, p:last-of-type {
    font-weight: bold;
    overflow-wrap: break-word;
  }

  span {
    display: inline-block;
    margin-right: 2em;
  }
</style>
