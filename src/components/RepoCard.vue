<template>
  <article @click="$emit('selected')">
    <h2>{{ repoName }}</h2>
    <p>{{ repoDescription }}</p>
    <p><font-awesome-icon icon="fa-solid fa-star" /> {{ starCount }} <font-awesome-icon icon="fa-solid fa-code-fork" /> {{ forkCount }}</p>
    <p>Created: {{ dateCreated }}</p>
  </article>
</template>

<script>
export default {
  name: 'RepoCard',
  props: {
    repoName: String,
    repoDescription: String,
    starCount: Number,
    forkCount: Number,
    dateCreated: Date,
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
