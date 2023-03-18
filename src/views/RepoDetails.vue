<template>
  <div>
    <h2>{{ repository.name }}</h2>
    <p id="visi">{{ repository.visibility }}</p>
    <p v-if="repository.description">{{ repository.description }}</p>
    <p>Created: {{ new Date(repository.updated_at).toLocaleDateString('en-us',) }}</p>
    <p>Updated: {{ new Date(repository.created_at).toLocaleDateString('en-us',) }}</p>
    <h3>Stats</h3>
    <div class="boxes">
      <div class="stat-box">
        <h4>Commits</h4>
        {{ repository.commits }}
      </div>
      <div class="stat-box">
        <h4>Forks</h4>
        {{ repository.forks }}
      </div>
      <div class="stat-box">
        <h4>Language</h4>
        {{ repository.language }}
      </div>
      <div class="stat-box">
        <h4>Watchers</h4>
        {{ repository.watchers }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RepoDetails',
  data() {
    return {
      repository: {}
    };
  },
  mounted() {
    this.fetchRepositoryDetails();
  },
  methods: {
    fetchRepositoryDetails() {
      const repositoryName = this.$route.params.name;
      const url = `https://api.github.com/repos/lancevd/${repositoryName}`;

      fetch(url)
        .then(response => response.json())
        .then(data => {
          this.repository = data;
        })
        .catch(error => console.error(error));
    }
  }
};
</script>

<style>

.boxes {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  justify-content: space-between;
  gap: 0.7rem;
  width: 80%;
  margin: auto;
  align-items: center;
  flex-wrap: wrap;
}
.stat-box {
  height: 6rem;
  padding: 0 0.5rem;
  text-align: left;
  box-shadow: 0px 0px 3px #2c3e50;
}
#visi {
  color: #42b983;
  background: #d8f9ea;
  width: fit-content;
  border-radius: 15px;
  padding: 7px 12px;
  margin: auto;
}

/* Responsiveness */
@media screen and (max-width: 768px) {
  .boxes {
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>
