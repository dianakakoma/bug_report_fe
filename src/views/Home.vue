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
    <!-- New/Create Action - Reports -->
    <div>
      <h1>File a New Report</h1>
      Bug or Bright Idea:
      <input type="text" v-model="newReport_type" />
      <br />

      description:
      <input type="text" v-model="newDescription" />
      <br />
      suggested_fix:
      <input type="text" v-model="newSuggestedFix" />
      <br />
      URL:
      <input type="string" v-model="newURL" />
      <br />
      Status:
      <input type="string" v-model="newStatus" />
      <br />
      <!--User_id:
      <input type="integer" v-model="newUserId" />
      <br /> -->
      <!-- insert ScreenShot URL here
      Screenshot:
      <input type="string" v-model="newScreenShot" />
    -->
      <button v-on:click="createReport()">Create Report</button>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Bug Report!",
      reports: [],
      newDescription: "",
      newSuggestedFix: "",
      newURL: "",
      newStatus: ""
    };
  },
  created: function() {
    axios.get("api/reports").then((response) => {
      this.reports = response.data;
    });
  },
  methods: {
    createReport: function() {
      var params = {
        description: this.newDescription,
        suggested_fix: this.newSuggestedFix,
        url: this.newURL,
        status: this.newStatus
      };
      axios.post("/api/reports", params).then((reponse) => {
        this.reports.push(reponse.data);
        (this.newDescription = ""), (this.newReport_type = ""), (this.newURL = ""), (this.newStatus = "");
      });
    }
  }
};
</script>
