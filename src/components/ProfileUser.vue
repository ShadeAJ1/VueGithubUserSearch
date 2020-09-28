<template>
  <div class="card" style="width: 50%">
    <div class="card-content">
      <div class="media">
        <div class="media-left">
          <figure class="image is-48x48">
            <img :src="user.avatar_url" alt="Placeholder image" />
          </figure>
        </div>
        <div class="media-content">
          <p class="title is-4" v-text="user.name" v-if="user.name"></p>
          <p class="title is-4" v-else>{{ user.login }}</p>
          <p class="subtitle is-6">@{{ user.login }}</p>
        </div>
      </div>

      <div class="content">
        <b-message type="is-info" :closable="false">
          <div class="columns">
            <div class="column is-one-third">
              <li class="list-group-item">
                Repositories
                <strong>{{ user.public_repos }}</strong>
              </li>
              <li class="list-group-item">
                Followers
                <strong>{{ user.followers }}</strong>
              </li>
              <li class="list-group-item">
                Following <strong>{{ user.following }}</strong>
              </li>
            </div>
            <div class="column">
              <strong v-if="user.bio">Bio</strong>
              <p class="mb-1" v-if="user.bio">{{ user.bio }}</p>
              <b-button tag="a" :href="user.html_url"> Go To Profile </b-button>
            </div>
          </div>
        </b-message>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["user"],
};
</script>