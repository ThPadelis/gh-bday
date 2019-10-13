<template>
  <div class="d-flex justify-content-center align-items-center" v-if="user">
    <img :src="user.avatar_url" alt="user avatar" class="rounded-circle mr-3" height="150" />
    <h2 class="mb-0">
      <a :href="user.html_url" class="text-dark">{{getName}}</a>
      <span class="px-1">joined GitHub on</span>
      <span class="d-block">{{user.created_at | date}}</span>
    </h2>
  </div>
</template>

<script>
import * as moment from "moment";
export default {
  name: "user",
  props: {
    user: {
      required: true
    }
  },
  filters: {
    date(date, format = "LL") {
      return moment(date).format(format);
    }
  },
  computed: {
    getName() {
      return this.user.name !== "" ? this.user.name : this.user.login;
    }
  }
};
</script>

<style>
</style>