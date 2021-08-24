<template>
  <label for="regex">Regular Expression</label>
  <input @keyup="testit" v-model="regex" type="text" name="regex" id="regex" />
  <label for="userInput">Test String</label>
  <textarea
    @keyup="testit"
    rows="5"
    v-model="userInput"
    type="text"
    name="userInput"
    id="userInput"
  />
  <div id="results" v-if="results">
    <div v-for="result in results" :key="result">
      <p v-html="result"></p>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "#Tester",
  setup() {
    const regex = ref("");
    const userInput = ref("");
    const results = ref(null);
    const testit = () => {
      const regexval = new RegExp(`(${regex.value})`);
      results.value = userInput.value.split("\n").map((line) => {
        return line.replace(regexval, '<span class="highlight">$&</span>');
      });
    };

    return {
      testit,
      regex,
      userInput,
      results,
    };
  },
};
</script>

<style scoped>
input,
textarea,
label {
  display: block;
}
textarea,
input {
  margin: 10px 0;
  width: 100%;
}
label,
#results {
  margin-top: 40px;
}
#results {
  padding: 3px 40px;
  background-color: var(--accent-bg);
}
p >>> .highlight {
  color: var(--accent);
  font-weight: bold;
}
</style>
