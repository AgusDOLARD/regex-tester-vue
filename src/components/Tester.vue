<template>
  <label for="regex">Regular Expression</label>
  <input @keyup="testit" v-model="regex" type="text" name="regex" />
  <label for="check">Replace</label>
  <input type="checkbox" name="check" v-model="replaceAvailable" />
  <br />
  <input @keyup="testit" v-if="replaceAvailable" v-model="replaceInput" />
  <label for="userInput">Test String</label>
  <textarea @keyup="testit" rows="5" v-model="userInput" name="userInput" />
  <div class="results" v-if="results">
    <div v-for="result in results" :key="result">
      <p v-html="result"></p>
    </div>
  </div>
</template>

<script setup >
import { ref, watch } from "vue";

const regex = ref("^https?");
const replaceInput = ref("https");
const replaceAvailable = ref(false);
const userInput = ref(
  "https://google.com\nhttp://localhost:8080\narch.wiki.com"
);
const results = ref(null);
const testit = () => {
  const regexval = new RegExp(`(${regex.value})`);
  results.value = userInput.value.split("\n").map((line) => {
    if (!line.match(regexval)) return line;
    if (replaceAvailable.value) {
      return line.replace(
        regexval,
        `<span class="highlight">${replaceInput.value}</span>`
      );
    }
    return line.replace(regexval, '<span class="highlight">$&</span>');
  });
};

watch(replaceAvailable, () => testit());
</script>

<style scoped>
input:not([type="checkbox"]),
textarea {
  display: block;
}
input[type="checkbox"] {
  float: right;
}
input:not([type="checkbox"]),
textarea {
  margin: 10px 0;
  width: 100%;
}
label,
.results {
  margin-top: 40px;
}
.results {
  padding: 3px 40px;
  background-color: var(--accent-bg);
}
p >>> .highlight {
  color: var(--accent);
  font-weight: bold;
}
</style>
