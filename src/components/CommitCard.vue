<template>
  <article>
    <h3>{{ commitName }}</h3>
    <p>{{ committerUN }}</p>
    <p>{{ commitHash }}</p>
    <p>Created: {{ dateCreated | formatDate}}</p>
  </article>
</template>

<script>
export default {
  name: 'CommitCard',
  props: {
    commitName: String,
    committerUN: String,
    commitHash: String,
    dateCreated: String,
  }
}
</script>

<style scoped>
article {
  border-bottom: 1px solid #000;
}
</style>
