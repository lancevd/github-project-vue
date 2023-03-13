<template>
    <div>
      <h2>List of Lancevd's Github Repositories</h2>
      <ul>
        <li v-for="repo in displayedRepositories" :key="repo.id" class="repo-list">
          <router-link :to="{ name: 'repository', params: { name: repo.name } }">{{ repo.name }}</router-link>
          <p>{{ repo.description }}</p>
          <p>{{ repo.language }}</p>
        </li>
      </ul>
      <nav v-if="pages.length > 1">
        <ul class="pagination">
          <li class="page-item" :class="{ disabled: currentPage === 1 }">
            <a class="page-link" @click.prevent="goToPage(currentPage - 1)">Previous</a>
          </li>
          <li v-for="page in pages" :key="page" class="page-item" :class="{ active: currentPage === page }">
            <a class="page-link" @click.prevent="goToPage(page)">{{ page }}</a>
          </li>
          <li class="page-item" :class="{ disabled: currentPage === totalPages }">
            <a class="page-link" @click.prevent="goToPage(currentPage + 1)">Next</a>
          </li>
        </ul>
      </nav>
      <router-view></router-view>
    </div>
  </template>
  
  <script>
  export default {
    name: 'GithubRepositories',
    data() {
      return {
        repositories: [],
        currentPage: 1,
        itemsPerPage: 10
      };
    },
    computed: {
      displayedRepositories() {
        const startIndex = (this.currentPage - 1) * this.itemsPerPage;
        const endIndex = startIndex + this.itemsPerPage;
        return this.repositories.slice(startIndex, endIndex);
      },
      totalPages() {
        return Math.ceil(this.repositories.length / this.itemsPerPage);
      },
      pages() {
        const pagesArray = [];
        for (let i = 1; i <= this.totalPages; i++) {
          pagesArray.push(i);
        }
        return pagesArray;
      }
    },
    mounted() {
      this.fetchRepositories();
    },
    methods: {
      fetchRepositories() {
        const username = 'lancevd';
        const url = `https://api.github.com/users/${username}/repos`;
  
        fetch(url)
          .then(response => response.json())
          .then(data => {
            this.repositories = data;
          })
          .catch(error => console.error(error));
      },
      goToPage(pageNumber) {
        if (pageNumber >= 1 && pageNumber <= this.totalPages) {
          this.currentPage = pageNumber;
        }
      }
    }
  };
  </script>
  
  <style>
  ul {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 1rem;
  }

  li {
    list-style-type: none;
  }

  .repo-list{
    
  }

  </style>