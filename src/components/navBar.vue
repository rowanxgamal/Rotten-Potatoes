<template>
  <nav>
    <ul class="nav nav-pills">
      <li @click="clearSearch()" class="nav-item">
        <router-link to="/" class="nav-link"
          ><img src="../assets/Group 5.png"
        /></router-link>
      </li>
      <li class="nav-item">
        <div class="input-group">
          <input
            v-model="search"
            type="text"
            style="width: 500px"
            class="form-control"
            placeholder="Search"
            @input="searchMov"
            @click="trackSearch"
          />
          <!-- <div class="input-group-append searchbtn">
            <a
              class="nav-link dropdown-toggle"
              data-bs-toggle="dropdown"
              href="#"
              role="button"
              aria-expanded="false"
            >
              <button class="btn btn-dark" type="button">all</button>
            </a>
            <ul class="dropdown-menu">
              <li><a class="dropdown-item" href="#">TBC</a></li>
            </ul> 
          </div> -->
          <i class="fa-solid fa-magnifying-glass"></i>
        </div>
      </li>
      <li class="nav-item">
        <router-link
          class="nav-link links"
          :to="this.loginState ? '/watchList' : '/login'"
          @click="track"
          ><p>Watch list</p></router-link
        >
        
      </li>
      
      <li class="nav-item"><router-link class="nav-link links" to="/favourites"> Favourites </router-link></li>
      <li class="nav-item">
        <router-link to="/about" class="nav-link links"
          ><p>About/Contact us</p></router-link
        >
      </li>
      <li class="nav-item">
        <router-link v-if="!loginState" to="/signup" class="nav-link links"
          ><p>SignUp/Login</p></router-link
        >
        <router-link v-else to="/profile" class="nav-link links"
          ><i class="fa-solid fa-user"></i
        ></router-link>
        <a
          class="nav-link dropdown-toggle"
          data-bs-toggle="dropdown"
          href="#"
          role="button"
          aria-expanded="false"
        ></a>
        <ul class="dropdown-menu">
          <li>
            <h4>{{ userNAME }}</h4>
          </li>
          <li>
            <button v-if="loginState" class="dropdown-item" @click="signOut">
              Sign Out
            </button>
          </li>
        </ul>
      </li>
    </ul>
  </nav>
</template>

<script>
import $store from "../store/index.js";

export default {
  data() {
    return {
      search: "",
      userNAME: "",
      loginState: false,
    };
  },
  created() {
    const userDataString = localStorage.getItem("userData");
    if (!userDataString) {
      this.userNAME = "";
      $store.commit("setLoginStatus", false);
      $store.commit("setCurrUser", null);
    } else {
      $store.commit("setLoginStatus", true);
      $store.commit("setCurrUser", JSON.parse(userDataString));
      this.userNAME = JSON.parse(userDataString).userName;
    }
    this.loginState = $store.state.loginStatus;
  },
  methods: {
    clearSearch() {
      this.search = "";
      $store.commit("setsearchMovs", []);
    },
    searchMov() {
      const options = {
        method: "GET",
        headers: {
          accept: "application/json",
          Authorization:
            "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIwNjQ3YWZiMWU3ZjAwODk2M2M3MTU4MjM5N2VlNjVjMSIsInN1YiI6IjY1NTAxZDM3OTY1M2Y2MDExYmZkYzkzYyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.7PEbDmBQNqrY3JJ7bRgmJV8S8tPVS8ziZ4TkWSJ2IqU",
        },
      };

      fetch(
        `https://api.themoviedb.org/3/search/movie?query=${this.search}&include_adult=false&language=en-US&page=1`,
        options
      )
        .then((response) => response.json())
        .then((response) => {
          console.log("bahaa el fa7l");
          let movies = [];
          console.log(response.results);
          for (let i = 0; i < response.results.length; i++) {
            let x = {
              title: response.results[i].title,
              rate: Math.round(response.results[i].vote_average * 10) / 10,
              img:
                "https://image.tmdb.org/t/p/original" +
                response.results[i].poster_path,
              date: response.results[i].release_date,
              id: response.results[i].id,
            };
            movies.push(x);
          }

          $store.commit("setsearchMovs", movies);
          console.log($store.state.searchMovs);
        })
        .catch((err) => console.error(err));
    },
    signOut() {
      localStorage.removeItem("token");
      localStorage.removeItem("userData");
      this.$store.commit("setLoginStatus", false);
      this.$store.commit("setCurrUser", null);
      this.loginState = $store.state.loginStatus;
      this.$router.replace({ name: "home" });
    },
    track() {
      console.log("Here");
      this.$gtag.event("click navbar", {
        event_category: "click",
        event_label: "watchlist",
        value: "1",
      });
    },
    trackSearch() {
      console.log("Here");
      this.$gtag.event("click search", {
        event_category: "click",
        event_label: "search_clicks",
        value: "1",
      });
    },
  },
};
</script>

<style scoped>
* {
  z-index: 4;
}
nav {
  background-image: url("../assets/potatoterms.png");
  background-repeat: repeat;
  background-size: 30%;
  background-position: center;
  position: fixed;
  top: 0;
  width: 100%;
}
.nav-pills {
  display: flex;
  justify-content: space-evenly;
  list-style: none;
}
a {
  text-decoration: none;
  color: #fff;
  font-size: 14px;
  cursor: pointer;
}
img {
  width: 100px;
  height: 45px;
}
.dropdown-item {
  color: #000;
  font-size: 20px;
}
.form-control {
  height: 40px;
  border-radius: 0px 20px 20px 0;
  border: none;
  margin-top: 14px;
}
.form-control:focus {
  box-shadow: none;
  outline: none;
}
a:hover {
  text-decoration: none;
  color: #000;
}

.links {
  margin-top: 20px;
  font-size: 17px;
  color: #fff;
  letter-spacing: 0.9px;
  
}
.input-group {
  position: relative;
  border-radius: 20px 0 0 20px;
}
.searchbtn {
  position: absolute;
  left: -67px;
  height: 40px;
  border-radius: 20px 0 0 20px;
  border: none;
  margin-top: 6px;
}

.searchbtn .btn {
  padding: 10px 20px 13px 20px;
  border-radius: 20px 0 0 20px;
  z-index: 1;
  font-size: 10px;
}

.nav-item a {
  display: inline-block;
}

.fa-user {
  margin: -10px 0 0 0;
  padding: 10px;
  background-color: #fff;
  color: #ee9e3f;
  border-radius: 50%;
}

.fa-magnifying-glass {
  position: absolute;
  right: 0;
  bottom: 0;
  padding: 12px;
  background-color: #fff;
  color: #ee9e3f;
  z-index: 1;
}
h4 {
  padding: auto;
  text-align: center;
  font-size: 20px;
  color: #000;
  background-color: #ef9e3f;
}
</style>