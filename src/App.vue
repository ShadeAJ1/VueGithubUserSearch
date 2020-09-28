<template>
  <div id="app">
    <div>
      <section class="hero is-info">
        <div class="hero-body">
          <div class="container">
            <h1 class="title">Search Github Users</h1>
            <h2 class="subtitle">Made by Aryan Jain</h2>

            <div class="card mt-2 p-3" style="width: 50%">
              <b-field>
                <b-input
                  placeholder="Search for Users"
                  size="is-medium"
                  @input="debounceSearch($event)"
                  v-model="e"
                >
                </b-input>
              </b-field>
            </div>
          </div>
        </div>
      </section>

      <div class="container row mt-3" v-if="users.length !== 0">
        <div class="col-md-4">
          <ProfileUser :user="users" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ProfileUser from "./components/ProfileUser.vue";

import axios from "axios";
import _ from "lodash";

export default {
  name: "app",
  data() {
    return {
      params: {
        url: "https://api.github.com/users",
        client_id: "0bbeff79fa0c5d3f9e01",
        client_secret: "73699f371e3fb06074c17ad064f064d82d29443c",
        per_page: 100,
        sort: "created:asc",
        debounceSearch: null,
      },
      e: "",
      users: [],
      repositories: [],
    };
  },
  created() {
    this.debounceSearch = _.debounce(this.getInfo, 100);
  },
  components: {
    ProfileUser,
  },
  methods: {
    getRepositories(user) {
      axios
        .get(
          `${this.params.url}/${user}/repos?per_page=${this.params.per_page}&sort=${this.params.sort}&client_id=${this.params.client_id}&client_secret=${this.params.client_secret}`
        )
        .then(({ data }) => {
          //console.log(data) // eslint-disable-line no-console

          this.repositories = data;
        })
        .catch(() => {
          this.repositories = [];
        });
    },
    getProfile(user) {
      axios
        .get(
          `${this.params.url}/${user}?client_id=${this.params.client_id}&client_secret=${this.params.client_secret}`
        )
        .then(({ data }) => {
          this.users = data;
        })
        .catch(() => {
          this.users = [];
        });
    },
    getInfo($event) {
      console.log($event);
      const user = $event;

      if (user.length) {
        this.getProfile(user);
        this.getRepositories(user);
      } else {
        this.users = [];
        this.repositories = [];
      }
    },
  },
};
</script>

<style>
.bg-custom {
  background: #3fb984;
  color: #fff;
}

input {
  outline: none;
  box-shadow: none !important;
}
</style>