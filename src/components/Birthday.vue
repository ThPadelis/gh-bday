<template>
  <loader v-if="isLoading"></loader>
  <section v-else class="min-vh-100 d-flex justify-content-center align-items-center">
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
              ref="username"
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

    Loader: () =>
      import(
        /* webpckChunkName: "loading-spinner-component" */ "@/components/Loader.vue"
      )
  },
  data() {
    return {
      username: "",
      user: null,
      userNotFound: true,
      isLoading: false
    };
  },
  mounted() {
    this.$refs.username.focus();
  },
  methods: {
    async getUser() {
      try {
        this.isLoading = true;
        const response = await axios.get(
          `https://api.github.com/users/${this.username}`
        );
        this.isLoading = false;
        this.userNotFound = false;
        this.user = response.data;
      } catch (error) {
        this.isLoading = false;
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