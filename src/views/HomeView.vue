<template>
  <div class="home">
    <div id="details-card">
      <div>
      </div>
      <section id="dp">
        <img :src='user.avatar_url'>
      </section>
      <h2>{{ user.name }}</h2>
      <p>@{{ user.login }}</p>
      <p>{{ user.bio }}</p>

      <a href="/repositories"><button><b>View Repositories</b></button></a>

    </div>
    <div id="info-card">
      <h3>Personal Info</h3>
      <p><b>Email</b>: olamosob@gmail.com</p>
      <p><b>Location</b>: {{ user.location }}</p>
      <p><b>Twitter</b>: {{ user.twitter_username }}</p>
    </div>
    
    <div id="stats-card">
      <h3>Stats</h3>
      <p><b>Followers</b>: {{ user.followers }}</p>
      <p><b>Following</b>: {{ user.following }}</p>
      <p><b>Repositories</b>: {{ user.public_repos }}</p>
      <p><b>Gists</b>: {{ user.public_gists }}</p>

    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'HomeView',
  data() {
    return {
      user: {}
    };
  },
  mounted() {
    this.fetchUserData();
  },
  methods: {
    fetchUserData() {
      const url = 'https://api.github.com/users/lancevd';

      fetch(url)
        .then(response => response.json())
        .then(data => {
          this.user = data;
        })
        .catch(error => console.error(error));
    }
  }
};
</script>

<style>
.home {
  height: 80vh;
  border-radius: 20px;
  margin: 1rem;
  background: #eee;
  display: grid;
  text-align: left;
  grid-template-columns: repeat(20, 1fr);
  grid-template-rows: repeat(20, 1fr);
}

.home div {
  padding: 0.3rem 1rem;
}

#details-card {
  border-radius: 20px;
  background: #fff;
  grid-row: 2/20;
  grid-column: 2/10;
}

#info-card {
  border-radius: 20px;
  background: #fff;
  grid-row: 2/9;
  grid-column: 11/20;
}

#stats-card {
  border-radius: 20px;
  background: #fff;
  grid-column: 11/20;
  grid-row: 10/20;
}

#dp {
  width: 250px;
  height: 250px;
  overflow: hidden;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: auto;
}

#dp img {
  width: 100%;
}

button {
  background: #42b983;
  border: none;
  padding: 15px 10px;
  cursor: pointer;
  border-radius: 5px;
}

button:hover {
  color: #fff;
}

/* RESPONSIVENESS */

@media screen and (max-width: 760px) {
  .home {
    display: flex;
    flex-direction: column;
    gap: 20px;
    height: fit-content;
    padding: 20px;
  }
  #details-card {
    height: 550px;
  }

  #dp{
    height: 200px;
    width: 200px;
  }

  #info-card {
    height: 200px;
  }

  #stats-card {
    height: 200px;
  }
}

@media screen and (max-width:450px) {
  #dp {
    width: 150px;
    height: 150px;
  }
}
</style>