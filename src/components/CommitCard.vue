<template>
  <article>
    <h3>{{ commitName }}</h3>
    <p>{{ committerUN }}</p>
    <p>{{ commitHash }}</p>
    <p>Created: {{ dateCreated }}</p>
  </article>
</template>

<script>
export default {
  name: 'CommitCard',
  props: {
    commitName: String,
    committerUN: String,
    commitHash: String,
    dateCreated: Date,
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
