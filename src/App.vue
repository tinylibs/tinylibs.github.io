<template>
  <main id="app">
    <h1>{{ company }} Repositories</h1>
    <RepoCard 
      repoName="Repo 1"
      repoDescription="This is a test repository"
      starCount=547
      forkCount=1000000
      dateCreated="2022-02-14"
      @selected="showCommits"
    />
    <div class="slidein" :class="show ? 'open' : ''">
      <button class="close-btn" @click="showCommits">X</button>
      <h2>Repo 1</h2>
      <CommitCard v-if="show" 
        commitName="init commit"
        committerUN="Sylvia"
        commitHash="23hhg23kj4nc"
        dateCreated="2022-02-14"
      />
    </div>
  </main>
</template>

<script>
import RepoCard from './components/RepoCard.vue'
import CommitCard from './components/CommitCard.vue'

export default {
  data()
   {
    return {
      company: 'Netflix',
      show: false
    }
  },
  components: {
    RepoCard,
    CommitCard
  },
  methods: {
    showCommits() {
      this.show = !this.show
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 1.5rem;
}

/* Make slide in box hidden off screen with fixed positioning 100% to the right */
.slidein {
  max-width: 600px;
  padding: 2em 3em; 
  position: fixed; 
  z-index: 100;
  top: 0;
  right: -100%;
  background: #ddd;
  height: 100%;
  box-shadow: 1px 1px 10px rgba(0,0,0,.5);
  transition: all .5s ease-in-out; 
}

/* Set positioning back to 0 when toggled opened */
.open {
  right: 0; 
}

/* add a close button in case toggle button is hidden on smaller screens */
.close-btn {
  border: none; 
  font-weight: bold; 
  font-size: 2em; 
  background: transparent; 
  position: absolute; 
  top: 0;
  left: 0;
  padding: .5em;
}

/* General styles unrelated to slide in */
body {
  font-family: 'Mulish', sans-serif;
}

.toggle {
  margin: 1em;
}

button {
  padding: .5em 1em;
  border-radius: 3em;
  font-size: 1.1em;
}

h2 {
  font-weight: 200;
}
</style>
