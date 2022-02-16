<template>
  <article>
    <h3>{{ commitName }}</h3>
    <p>Committer: {{ committerUN }}</p>
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
  max-width: 90vw;
  margin: 0 auto;
  text-align: left;
  overflow-wrap: break-word;
}

h3 {
  font-weight: bold;
  font-size: 1.2em;
}

p:last-of-type {
  font-weight: bold;
}

</style>
