<template>
  <div id="app">
    <h1>{{ company }} Repositories</h1>
    <main id="app">
      <RepoCard v-for="repo in repoInfo" :key="repo.name"
        :repoName=repo.name
        :repoDescription=repo.description
        :language=repo.language
        :starCount=repo.starCount
        :forkCount=repo.forkCount
        :dateCreated=repo.dateCreated
        @selected="showCommits"
      />
      <div class="slidein" :class="show ? 'open' : ''" v-if="show">
        <button class="close-btn" @click="showCommits">X</button>
        <h2>{{ currentRepo }}</h2>
        <div class="commit-info">
          <CommitCard v-for="commit in commits" :key="commit.hash"
            :commitName=commit.title
            :committerUN=commit.username
            :commitHash=commit.hash
            :dateCreated=commit.dateCreated
          />
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import RepoCard from './components/RepoCard.vue'
import CommitCard from './components/CommitCard.vue'

export default {
  data()
   {
    return {
      company: 'Netflix',
      show: false,
      repoInfo: [],
      commits: [],
      currentRepo: ''
    }
  },
  components: {
    RepoCard,
    CommitCard
  },
  methods: {
    showCommits(name) {
      if (this.commits.length == 0) {
        this.currentRepo = name;
        this.getCommits(this.company, name)
        this.show = !this.show;
      }
      else {
      // this.commits ? this.commits = [] : this.getCommits(this.company, name);
      this.show = !this.show
      this.commits = [];
      }
    },
    getRepos(company) {
      // clear repoInfo before making a call to the new company
      // this.repoInfo = [];
      fetch(`https://api.github.com/orgs/${company}/repos`)
      .then(res => res.json())
      .then(json => {
        json.forEach(repo => {
          this.repoInfo.push({
            name: repo.name,
            language: repo.language,
            description: repo.description,
            starCount: repo.stargazers_count,
            forkCount: repo.forks_count,
            dateCreated: repo.created_at
          })
        })
      })
    },
    getCommits(company, name) {
      fetch(`https://api.github.com/repos/${company}/${name}/commits`)
      .then(res => res.json())
      .then(json => {
        json.forEach(commit => {
          this.commits.push({
            title: commit.commit.message,
            username: commit.author.login,
            hash: commit.sha,
            dateCreated: commit.commit.committer.date
          })
        })
      })
    }
  },
  mounted: function() {
    this.getRepos('Netflix');
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

main {
  max-width: 1000px;
  display: grid;
  grid-template-columns: auto auto auto;
  grid-gap: 1rem;
  margin: 1rem 1rem;
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
  height: 100vh;
  overflow-y: auto;
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

.toggle {
  margin: 1em;
}

button {
  padding: .5em 1em;
  border-radius: 3em;
  font-size: 1.1em;
  cursor: pointer;
  position: fixed;
}

h2 {
  font-weight: 200;
}
</style>
