<template>
  <div class="home">
    <img src="@/assets/happy-ladybug.png" width="100" />
    <h1>{{ message }}</h1>
    <h2>A bug reporting tool built for the end user!</h2>
    <!-- Sign Up | Login | Logout Buttons -->
    <button class="signup_btn"><a href="/signup">Sign up!</a></button>
    |
    <button>
      <a href="/login">Login</a>
    </button>
    |
    <button><a href="/logout">Logout</a></button>
    <hr />
    <!-- Index Action - Report -->
    <div v-for="report in reports">
      <p>Report ID:{{ report.id }}</p>
      <h2>Bug or Bright Idea: {{ report.report_type }}</h2>
      <h3>Report Description: {{ report.description }}</h3>
      <p>Suggest Fix: {{ report.suggested_fix }}</p>
      <p>Current Status:{{ report.status }}</p>
      <a v-bind:href="report.url">{{ report.url }}</a>
      <p>User Id: {{ report.user_id }}</p>
      <p style="color:red">|</p>
    </div>
    <!--Create Action - Report -->
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Bug Report!",
      reports: []
    };
  },
  created: function() {
    axios.get("api/reports").then((response) => {
      this.reports = response.data;
    });
  },
  methods: {}
};
</script>
