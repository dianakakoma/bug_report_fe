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
    <!-- New/Create Action - Reports -->
    <div style="text-align:left">
      <h1>File a New Report</h1>
      Bug or Bright Idea:
      <input type="text" v-model="newReportType" />
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
      <form v-on:submit.prevent="submit()">
        Screenshot:
        <input type="file" v-on:change="setFile($event)" ref="fileInput" />
        <input type="submit" value="Submit your Screenshot" />
      </form>
      <button v-on:click="createReport()">Create Report</button>
    </div>

    <!-- Index Action for Reports -->
    <h4>Scroll down to see what has already been submitted</h4>
    <div style="text-align:left" v-for="report in reports">
      <p>Report ID:{{ report.id }}</p>
      <h2>Bug or Bright Idea: {{ report.report_type }}</h2>
      <h3>Report Description: {{ report.description }}</h3>
      <!--Show Action -->
      <button v-on:click="showReport(report)" style="background-color: blue; color:white">Show More...</button>
      <div v-if="currentReport === report">
        <p>Suggested Fix: {{ report.suggested_fix }}</p>
        <!-- Update/Patch - Report -->
        <div>
          Update the Fix:
          <input type="text" v-model="report.suggested_fix" />
          <button v-on:click="updateSuggestedFix(report)">Update Your Suggested Fix</button>
        </div>
        <!-- end update patch -->
        <p>Current Status:{{ report.status }}</p>
        <a v-bind:href="report.url">{{ report.url }}</a>
        <p>Submitted by: {{ report.name }}</p>
      </div>
      <h1 style="color:red">***</h1>
    </div>
    <!-- New/Create Action - Reports -->
    <div style="text-align:left">
      <h1>File a New Report</h1>
      Bug or Bright Idea:
      <input type="text" v-model="newReportType" />
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
    </div>
    <button v-on:click="createReport()">Create Report</button>
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
      newStatus: "",
      selectedFile: null,
      image: ""
    };
  },
  created: function() {
    axios({
      method: "GET",
      url: `https://radiant-mesa-02892.herokuapp.com/api/reports`,
      headers: {
        "Access-Control-Allow-Origin": "*",
        "Content-type": "application/json"
      }
    }).then((response) => {
      this.reports = response.data;
      console.log(response.data);
    });
  },
  methods: {
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.image = event.target.files[0];
      }
    },
    submit: function() {
      var formData = new FormData();
      formData.append("image", this.image);
      formData.append("report_id", this.report.id);
      axios.post("https://radiant-mesa-02892.herokuapp.com/", formData).then((response) => {
        this.$refs.fileInput.value = "";
        this.reports.images.push(response.data);
      });
    },
    createReport: function() {
      var params = {
        description: this.newDescription,
        suggestedFix: this.newSuggestedFix,
        url: this.newURL,
        status: this.newStatus,
        reportType: this.newReportType
      };
      axios({
        method: "POST",
        url: `https://cors-anywhere.herokuapp.com/https://radiant-mesa-02892.herokuapp.com/api/reports`,
        data: params,
        headers: {
          "Access-Control-Allow-Origin": "*",
          "Content-type": "application/json"
        }
      }).then((reponse) => {
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

    /*
    updateUser: function(user) {
      var params = {
        department: user.department
      };
      axios.patch("/api/users/" + user.id, params).then((response) => {
        this.currentUser = {};
      });
    }
    */
    updateSuggestedFix: function(report) {
      var params = {
        suggested_fix: report.suggested_fix
      };
      axios.patch("/api/reports/" + report.id, params).then((response) => {
        this.currentReport = {};
      });
    }
  }
};
</script>
