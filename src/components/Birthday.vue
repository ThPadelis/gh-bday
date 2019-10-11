<template>
  <main class="min-vh-100 d-flex align-items-center">
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-5">
          <div class="form-group">
            <!-- <label for="username">Username</label> -->
            <input
              type="text"
              name="username"
              id="username"
              v-model="username"
              class="form-control"
              placeholder="@username"
              autocomplete="off"
              @keyup.enter="onSubmit()"
            />
          </div>
        </div>
        <div class="col-12" v-if="user">
          <div class="d-flex align-items-center justify-content-center">
            <img :src="user.avatar_url" alt="user avatar" height="150" class="rounded-circle mr-4" />
            <div class="mb-0">
              <h4>
                <a
                  :href="user.html_url"
                  target="_blank"
                  class="text-dark mr-1"
                >{{user.name || username | capitalize}}</a>
                <span class="mr-1">joied GitHub on</span>
                <span class="d-block text-underline">{{user.created_at | moment}}</span>
              </h4>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import axios from "axios";
import * as moment from "moment";
export default {
  name: "birthday",
  filters: {
    capitalize(value) {
      if (!value) return "";
      value = value.toString();
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
    moment(date, format = "LL") {
      return moment(date).format(format);
    }
  },
  data() {
    return {
      username: "",
      user: null
    };
  },
  methods: {
    getUserInfo(username) {
      return axios
        .get(`https://api.github.com/users/${username}`)
        .then(response => response.data)
        .catch(error => {
          this.user = null;
        });
    },
    onSubmit() {
      if (this.username.trim() !== "") {
        this.getUserInfo(this.username).then(user => {
          this.user = user;
          this.getNextBday(new Date(user.created_at));
        });
      }
    },
    getNextBday(birthday) {
      let today = moment();
      var bday = moment(
        new Date(today.get("year"), birthday.getMonth(), birthday.getDate())
      );

      let nextBday = moment.duration(bday.diff(moment())).asDays();
      console.log(Math.abs(Math.floor(nextBday)));
    }
  }
};
</script>

<style scoped>
main {
  background: #f8f9fa;
}

[class*="col-"] {
  margin-top: 1.5rem;
  margin-bottom: 1.5rem;
}

.text-underline {
  text-decoration: underline;
}
</style>