<template>
  <section class="min-vh-100 d-flex justify-content-center align-items-center">
    <div class="container">
      <!-- Logo -->
      <div class="row">
        <div class="col-12 text-center">
          <img :src="require('@/assets/steroidtocat.png')" alt="octocat on steroid" height="200" />
        </div>
      </div>
      <!-- Input -->
      <div class="row justify-content-center">
        <div class="col-12 col-md-5 text-center">
          <div class="input-group border rounded pl-3">
            <input
              type="text"
              name="username"
              id="username"
              class="form-control"
              v-model.trim="username"
              placeholder="GitHub username"
              @keyup.enter="getUser()"
            />
            <div class="input-group-append">
              <button class="btn btn-secondary" @click="getUser()">
                <span class="fa fa-search"></span>
              </button>
            </div>
          </div>
        </div>
      </div>
      <!-- Response -->
      <div class="row justify-content-center">
        <div class="col-12">
          <div v-if="!userNotFound">
            <user :user="user"></user>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "birthday",
  components: {
    User: () =>
      import(/* webpckChunkName: "user-component" */ "@/components/User.vue"),

    NotFound: () =>
      import(
        /* webpckChunkName: "user-not-found-component" */ "@/components/NotFound.vue"
      )
  },
  data() {
    return {
      username: "",
      user: null,
      userNotFound: true
    };
  },
  methods: {
    async getUser() {
      try {
        const response = await axios.get(
          `https://api.github.com/users/${this.username}`
        );
        this.userNotFound = false;
        this.user = response.data;
      } catch (error) {
        if (error.response.status === 404) {
          this.userNotFound = true;
          this.user = null;
        }
      }
    }
  }
};
</script>

<style lang="scss" scoped>
[class*="col-"] {
  margin-bottom: 1.5rem;
}
</style>