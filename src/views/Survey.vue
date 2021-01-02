<template>
  <div class="container">
    <survey :survey="survey"></survey>
  </div>
</template>

<script>
const axios = require("axios").default;
import * as SurveyVue from "survey-vue";
import "bootstrap/dist/css/bootstrap.css";
var Survey = SurveyVue.Survey;
Survey.cssType = "bootstrap";

import * as widgets from "surveyjs-widgets";

import { init as customWidget } from "../components/customwidget";

// widgets.icheck(SurveyVue);
widgets.select2(SurveyVue);
widgets.inputmask(SurveyVue);
widgets.jquerybarrating(SurveyVue);
widgets.jqueryuidatepicker(SurveyVue);
widgets.nouislider(SurveyVue);
widgets.select2tagbox(SurveyVue);
widgets.sortablejs(SurveyVue);
widgets.ckeditor(SurveyVue);
widgets.autocomplete(SurveyVue);
widgets.bootstrapslider(SurveyVue);
customWidget(SurveyVue);

SurveyVue.Serializer.addProperty("question", "tag:number");

const API_URL = "/api/survey/";

export default {
  components: {
    Survey,
  },
  data() {
    return {
      survey: new SurveyVue.Model(),
      surveyName: "",
    };
  },
  mounted() {
    const setSurvey = (response) => {
      console.log(response);
      this.surveyName = response.data.name;
      this.survey = new SurveyVue.Model(response.data);
      this.survey.onComplete.add(onSurveyComplete);
    };
    const onSurveyComplete = (sender, options) => {
      axios
        .post(`${API_URL}/${this.surveyName}/post`, sender.data)
        .then(setSurvey)
        .catch(console.log);
    };
    axios
      .get(`${API_URL}/${this.surveyName}`)
      .then(setSurvey)
      .catch(console.log);
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
