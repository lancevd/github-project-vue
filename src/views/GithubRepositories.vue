<template>
    <div>
      <h2>List of Lancevd's Github Repositories</h2>
      <ul class="repos-ul">
        <li v-for="repo in displayedRepositories" :key="repo.id" class="repo-list">
          <router-link :to="{ name: 'repository', params: { name: repo.name } }">{{ repo.name }}</router-link>
          <p v-if="repo.description" class="repo-desc">{{ repo.description }} {{ repo.description.length }}</p>
          <p v-else>No description</p>
          <!-- <p v-if="repo.description.length > 30"> Too long </p> -->
          <div id="lang">
            <!-- <div id="lang-logo"> -->
              <img v-if="repo.language == 'HTML'" src="../assets/html.png" class="lang-logo" >
              <img v-else-if="repo.language == 'CSS'" src="../assets/css.png" class="lang-logo" >
              <img v-else-if="repo.language === 'JavaScript'" src="../assets/javascript.png" class="lang-logo" >
              <img v-else-if="repo.language === 'Vue'" src="../assets/logo.png" class="lang-logo" >
              <!-- <img class="lang-logo" src="../assets/html.png" > -->
            <!-- </div> -->
            <p>{{ repo.language }}</p>
          </div>
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
        itemsPerPage: 12
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
  .repos-ul {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 1rem;
  }

  li {
    list-style-type: none;
  }

  .repo-list{
    /* border: solid gray 2px; */
    border-radius: 10px;
    padding: 0.5rem 0.8rem;
    box-shadow: 0px 0px 3px rgba(60, 59, 59, 0.685);
    text-align: left;
  }

  .repo-list a {
    text-decoration: none;
    color: #42b983;
  }

  #lang {
    display: flex;
    gap: 0.5rem;
    align-items: center;
    justify-content: start;
  }

  .lang-logo {
    height: 50px;
    width: 50px;
    border-radius: 50%;
    /* overflow: hidden; */
    /* border: red solid; */
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .pagination {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 15px;
  }

  .page-item {
    width: 150px;
    background-color: #42b983;
    cursor: pointer;
    border-radius: 5px;
    height: 45px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .page-item:hover {
    padding: 5px;
  }

  .page-link {
    width: 100%;
  }

  </style>