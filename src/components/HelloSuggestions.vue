<template>
  <div class="hello">
    <h1>Vue.js Suggestions AI Demo</h1>
    <p>Type or paste any data into the spreadsheet below</p>

    <fieldset>
      <label for="input">Google Spreadsheet URL with input data:</label>
      <input id="input" v-model="input"/>
    </fieldset>

    <fieldset>
      <label for="output">
        Response from the
        <a href="" target="_blank">https://api.inyourapp.ai/v0/suggestions</a> endpoint:
      </label>
      <textarea id="output" v-model="output"></textarea>
      <button></button>
    </fieldset>

    <ul>
      <li>
        <a href="https://inyourapp.docs.apiary.io/" target="_blank">API Docs</a>
      </li>
      <li>
        For more information, visit
        <a href="https://inyourapp.ai/" target="_blank">inyourapp.ai</a>
      </li>
    </ul>
  </div>
</template>

<script>
import debounce from 'lodash.debounce';
import request from 'request-promise-native';

export default {
  name: 'HelloWorld',
  data() {
    return {
      input: 'https://docs.google.com/spreadsheets/d/1MSbr4XhbH27L3yb4Kl6mny-5lMl5qPRmiDskCaBS-gc/edit',
      output: '',
    };
  },
  watch: {
    input(val) {
      console.log('Input changed to: ', val);
      this.processInput(val);
    },
  },
  methods: {
    async processInput(input) {
      console.log(
        'Querying the https://gateway.inyourapp.ai/v0/models/demo/predict REST API Endpoint...',
      );
      try {
        const parsedBody = await request({
          method: 'POST',
          uri: 'https://gateway.inyourapp.ai/v0/models/demo/predict',
          headers: {
            apiKey: 'foo',
          },
          body: {
            googleSpreadsheetUrl: input,
          },
          json: true,
        });
        this.output = JSON.stringify(parsedBody, null, '\t');
      } catch (err) {
        console.error(err);
      }
    },
  },
  created() {
    this.processInput = debounce(this.processInput, 500);
    this.processInput(this.input);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
fieldset {
  width: 50%;
}
textarea {
  min-height: 200px;
}
div.hello {
  text-align: center;
  justify-content: center;
  width: 100%;
}
fieldset {
  margin: auto;
  display: inline-block;
  border: 0;
}
textarea, input {
  padding: 2%;
  width: 96%;
  border: 1px solid #888888;
}
label {
  display: block;
  margin-bottom: 0.5em;
}
</style>
