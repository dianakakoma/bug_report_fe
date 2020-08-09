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
    <div v-for="report in reports" style="text-align:left">
      <p>Report ID:{{ report.id }}</p>
      <h3>Report Type: {{ report.report_type }}</h3>
      <p>Report Description: {{ report.description }}</p>
<!--Show Action -->
      <button v-on:click="showReport(report)" style="background-color: blue; color:white">Show More...</button>
      <div v-if="currentReport === report">
        <p>Suggest Fix: {{ report.suggested_fix }}</p>

        <p>Current Status:{{ report.status }}</p>
        <a v-bind:href="report.url">{{ report.url }}</a>
        <p>Submitted by: {{ report.name }}</p>
<!-- Update/Patch action - Report -->
        <button v-on:click="updateReport(report)">Update Suggested Fix</button>

      </div>
      <p style="color:red">|</p>
    </div>
<!-- New/Create Action - Reports -->
    <div>

      <h1>File a New Report:</h1>
      Report Type:
      <input type="radio" id="bug" value="bug" v-model="picked">
      <label for="bug">Bug</label>
      <p> or
      <input type="radio" id="bright_idea" value="bright idea" v-model="picked">
      <label for="bright_idea">Bright Idea</label>
      
      <br/>

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
      Screenshot: <input type="file" v-on:change="setFile($event)" ref="fileInput">
      <input type="submit" value="Submit your screenshots">
    </form>
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
        reportType: this.newReport_type
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
    showReport: function(report){
      if(this.currentReport === report){
        this.currentReport = {};
      } else {
        this.currentReport = report;
      }
    },
    updateReport: function(report){
      var params = {
        suggested_fix: report.suggested_fix
      };
      axios.patch("/api/reports/" + report.id, params).then(reponse => {
        this.currentPhoto = {};
      });
    },
    setFile: function(event){
      if(event.target.files.length > 0){
        this.image_url = event.target.files[0];
      }
    },
    submit: function(){
      var formData = new FormData();
      form.data.append("image_url",this.image_url);
      formData.append("report_id", this.report.id);
      axios.post("/api/images", formData).then(response =>{
        this.$refs.fileInput.value = "";
        this.report.images.push(reponse.data);
      })
    }
    }
};
</script>
