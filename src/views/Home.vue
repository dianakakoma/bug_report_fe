<template>
  <div class="home">
    <p>
      <img src="@/assets/happy-ladybug.png" width="100" />
    </p>

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
    <!-- Index Action for Reports -->
    <div style="text-align:left" v-for="report in reports">
      <p>Report ID:{{ report.id }}</p>
      <h2>Bug or Bright Idea: {{ report.report_type }}</h2>
      <h3>Report Description: {{ report.description }}</h3>
      <!--Show Action -->
      <button v-on:click="showReport(report)" style="background-color: blue; color:white">Show More...</button>
      <div v-if="currentReport === report">
        <p>Suggest Fix: {{ report.suggested_fix }}</p>
        <p>Current Status:{{ report.status }}</p>
        <a v-bind:href="report.url">{{ report.url }}</a>
        <p>Submitted by: {{ report.name }}</p>
      </div>
      <!-- Update/Patch - User -->
      <div>
        <input type="text" v-model="user.department" />
        <button v-on:click="updateUser(user)">Update Department</button>
      </div>
      <!-- Update/Patch - Report -->
      <div>
        <input type="text" v-model="user.suggested_fix" />
        <button v-on:click="updateSuggestedFix(report)">Update Your Suggested Fix</button>
      </div>
      <h1 style="color:red">***</h1>
    </div>
    <!-- New/Create Action - Reports -->
    <div style="text-align:left">
      <h1>File a New Report</h1>
      Bug or Bright Idea:
      <input type="text" v-model="newReporType" />
      <br />

      description:
      <input type="text" v-model="newDescription" />
      <br />
      suggested_fix:
      <input type="text" v-model="newSuggestedFix" />
      <br />
      URL:
      <input type="text" v-model="newURL" />
      <br />
      Status:
      <input type="string" v-model="newStatus" />
      <br />
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
      currentReport: {},
      reports: [],
      newReportType: "",
      newDescription: "",
      newSuggestedFix: "",
      newURL: "",
      newStatus: ""
    };
  },
  created: function() {
    axios.get("/api/reports").then((response) => {
      this.reports = response.data;
    });
  },
  methods: {
    createReport: function() {
      var params = {
        description: this.newDescription,
        suggestedFix: this.newSuggestedFix,
        url: this.newURL,
        status: this.newStatus,
        reportType: this.newReportType
      };
      axios.post("/api/reports", params).then((reponse) => {
        this.reports.push(reponse.data);
        this.newDescription = "";
        this.newReportType = "";
        this.newSuggestedFix = "";
        this.newURL = "";
        this.newStatus = "";
      });
    },
    showReport: function(report) {
      if (this.currentReport === report) {
        this.currentReport = {};
      } else {
        this.currentReport = report;
      }
    },
    updateUser: function(user) {
      var params = {
        department: user.department
      };
      axios.patch("/api/users/" + user.id, params).then((response) => {
        this.currentUser = {};
      });
    },
    updateSuggestedFix: function(report) {
      var params = {
        suggested_fix: report.suggested_fix
      };
      axios.patch("/api/users/" + report.id, params).then((response) => {
        this.currentUser = {};
      });
    }
  }
};
</script>
