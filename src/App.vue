<template>
  <div id="app">
    <header>
      <h1>{{ currentCompany }} Repositories</h1>
      <label for="company">Want repositories from a different company?</label>
        <input type="text" placeholder="Company name" id="company" v-model="company" name="company" v-on:keydown.enter.prevent="getRepos(company)" />
      <button @click='getRepos(company)'>New Search</button>
    </header>
    <div class="user-options">
      <span @click="toggleListView()"><font-awesome-icon icon="fa-solid fa-list" :class="isList ? 'list-icon-color' : ''"/><font-awesome-icon icon="fa-solid fa-border-all" :class="isList ? '' : 'list-icon-color'"/></span>
      <label for="sort-options" id="sort-options-label">Sort by: 
        <select 
          name="sort-options" 
          id="sort-options"
          v-model="sortOn">
          <option value="starCount">Most stars</option><option value="dateCreated">Most recent</option>
          <option value="name">Alphabetical</option>
          <option value="forkCount">Most forks</option>
          <option value="language">Language</option>
        </select>
      </label>
    </div>
    <main v-if="initialLoaded" :class="isList ? 'list' : ''">
        <RepoCard v-for="repo in sortedArray" :key="repo.name"
          :classStyle=isList
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
        <div v-if="commitsLoaded">
          <CommitCard v-for="commit in commits" :key="commit.hash"
            :commitName=commit.title
            :committerUN=commit.username
            :commitHash=commit.hash
            :dateCreated=commit.dateCreated
          />
        </div>
        <div v-else>
          <Loading />
        </div>
      </div>
    </main>
    <main v-else class="loader">
      <Loading />
    </main>
    <Loading v-if="!pagesLoaded"/>
    <button v-if="morePagesAvailable" @click="paginateRepos">See More</button>
  </div>
</template>

<script>
import RepoCard from './components/RepoCard.vue'
import CommitCard from './components/CommitCard.vue'
import Loading from './components/Loading.vue'
import axios from 'axios'
const baseGithubApi = 'https://api.github.com/orgs'
const githubApiParams = 'repos?sort=created&direction=desc&per_page=30'

export default {
  name: 'App',
  data()
   {
    return {
      company: '',
      currentCompany: '',
      show: false,
      isList: true,
      initialLoaded: false,
      commitsLoaded: true,
      pagesLoaded: true,
      repoInfo: [],
      commits: [],
      currentRepo: '',
      sortOn: 'starCount',
      page: 1,
    }
  },
  components: {
    RepoCard,
    CommitCard,
    Loading
  },
  computed: {
    sortedArray: function() {
      const arrayToSort = this.repoInfo;
      if (this.sortOn == 'name' || this.sortOn == 'language') {

        return arrayToSort.sort((a, b) => {
          // check for any null values to avoid error from string method on null
          let aFormatted = a[this.sortOn] !== null ? a[this.sortOn].toLowerCase() : null
          let bFormatted = b[this.sortOn] !== null ? b[this.sortOn].toLowerCase() : null

          // account for null values when sorting to move them to the end
          if (aFormatted === bFormatted) {
            return 0
          } else if (aFormatted === null) {
            return 1
          } else if (bFormatted === null) {
            return -1
          } else {
            return aFormatted < bFormatted ? -1 : 1
          }
        })
      } else {
        return arrayToSort.sort((a, b) => a[this.sortOn] > b[this.sortOn] ? -1 : 1)
      }
    },
    morePagesAvailable: function() {
      return this.repoInfo.length == (30 * this.page)
    },
    class: function() {
      return this.isList
    }
  },
  methods: {
    toggleListView() {
      return this.isList = !this.isList;
    },
    showCommits(name) {
      if (this.commits.length == 0) {
        this.currentRepo = name;
        this.getCommits(this.currentCompany, name)
        this.show = !this.show;
      }
      else {
        this.show = !this.show
        this.commits = [];
      }
    },
    getRepos: async function(company) {
      // clear repoInfo before making a call to the new company
      if (company.trim() === '') return;
      this.initialLoaded = false;
      this.repoInfo = [];
      this.page = 1;
      this.currentCompany = company.trim();
      await axios.get(`${baseGithubApi}/${company}/${githubApiParams}`)
      .then(res =>
        res.data.forEach(repo => {
          this.repoInfo.push({
            name: repo.name,
            language: repo.language,
            description: repo.description,
            starCount: repo.stargazers_count,
            forkCount: repo.forks_count,
            dateCreated: repo.created_at
          })
        })
      )
      this.company = '';
      this.initialLoaded = true;
    },
    paginateRepos: async function() {
      this.pagesLoaded = false;
      this.page = this.page + 1;
      await axios.get(`${baseGithubApi}/${this.currentCompany}/${githubApiParams}&page=${this.page}`)
      .then(res =>
        res.data.forEach(repo => {
          this.repoInfo.push({
            name: repo.name,
            language: repo.language,
            description: repo.description,
            starCount: repo.stargazers_count,
            forkCount: repo.forks_count,
            dateCreated: repo.created_at
          })
        })
      )
      this.pagesLoaded = true;
    },
    getCommits: async function(company, name) {
      this.commitsLoaded = false
      await axios.get(`https://api.github.com/repos/${company}/${name}/commits`)
        .then(res => {
          res.data.forEach(commit => {
            const commitInfo = {
              title: commit.commit.message,
              username: '',
              hash: commit.sha,
              dateCreated: commit.commit.committer.date
            }
            commit.author !== null ? commitInfo.username = commit.author.login : commitInfo.username = commit.commit.author.email
            this.commits.push(commitInfo)
          })
        }
      )
      this.commitsLoaded = true;
    },
  },
  mounted: function() {
    this.getRepos('Netflix');
  },
}
</script>

<style>
:root {
  --main-color: #00beac;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

header {
  margin: 0rem auto 2rem auto;
  background-image: linear-gradient(to bottom right, var(--main-color), var(--main-color), #fff, #fff, #fff)
}

header label {
  display: flex;
  flex-direction: column;
  width: 35vw;
  margin: 1rem auto 0.5rem;
}

header input, header button {
  line-height: 1.5rem;
  padding: 0.5rem;
  border: 1px solid var(--main-color);
}

header input {
  border-radius: 0.3rem 0rem 0rem 0.3rem;
}

header button {
  border-radius: 0rem 0.3rem 0.3rem 0rem;
  background-color: var(--main-color);
  border-top: 0px;
  border-bottom: 2px solid var(--main-color);
  color: #fff;
}

header button:hover {
  background-color: #00998a;
}

input:focus {
  font-size: 1.1em;   
}

main {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-flow: row;
  grid-gap: 1.5rem;
  margin: 1rem;
}

.loader {
  display: flex;
  width: 100vw;
  justify-content: center;
}

.user-options {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin: 0.5rem 2rem;
}

.user-options > span {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 2vw;
  cursor: pointer;
}

.user-options > span > * {
  margin: 0 0.5rem;
}

/* Styles for switching between list and grid views */
.list-icon-color {
  color: var(--main-color);
}

main.list {
  display: flex;
  flex-direction: column;
}

/* Make slide in box hidden off screen with fixed positioning 100% to the right */
.slidein {
  max-width: 600px;
  padding: 2em 3em; 
  position: fixed; 
  z-index: 100;
  top: 0;
  right: -100%;
  background-image: linear-gradient(to right, #d0d0d0, #ececec, #fff, #fff, #fff, #fff, #ececec, #d0d0d0);
  height: 95vh;
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
  border: 1px solid var(--main-color);
  background-color: #fff;
  color: var(--main-color);
  padding: .5em 1em;
  border-radius: 0.7em;
  font-size: 1em;
  font-weight: bolder;
  cursor: pointer;
}

button:hover {
  background-color: var(--main-color);
  color: #fff;
  transition: 0.5s
}

h1 {
  margin-top: 0rem;
  padding-top: 1.5rem;
}

h2, h3 {
  color: var(--main-color);
}

h2 {
  font-weight: boldest;
  font-size: 2.5em;
}



@media screen and (max-width: 767px) {
  header label {
    width: 80vw;
  }
  
  main {
    grid-template-columns: auto;
  }

  .slidein {
    max-width: 200px;
  }
}

</style>
