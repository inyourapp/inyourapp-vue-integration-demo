<template>
  <div class="hello">
    <h1>Text AI Demo</h1>
    <p>Type or paste any text into the textarea below</p>
    <textarea v-model="input"></textarea>
    <p>Results:</p>
    <textarea v-model="output"></textarea>
    <ul>
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
      input: '',
      output: '',
    };
  },
  watch: {
    input(val, oldVal) {
      console.log('Input changed to: ', val);
      this.processInput(val);
    },
  },
  methods: {
    async processInput(input) {
      console.log('Querying REST API Endpoint...');
      try {
        const parsedBody = await request({
          method: 'POST',
          uri: 'https://api.inyourapp.ai/v0/text',
          headers: {
            apiKey: 'foo',
          },
          body: {
            text: input,
          },
          json: true,
        });
        this.output = JSON.stringify(parsedBody);
      } catch (err) {
        console.error(err);
      }
    },
  },
  created() {
    this.processInput = debounce(this.processInput, 500);
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
textarea {
  width: 50%;
  min-height: 200px;
}
</style>
