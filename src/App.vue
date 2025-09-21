<template>
  <div class="app">
    <h1>🧠 Sentiment Analysis</h1>
    <textarea v-model="text" placeholder="Type something..." rows="4"></textarea>
    <button @click="analyzeSentiment">Analyze</button>

    <div v-if="loading">Analyzing...</div>
    <div v-if="result">
      <h2>Result</h2>
      <p><strong>Text:</strong> {{ result.text }}</p>
      <p><strong>Sentiment:</strong> 
        <span :style="{ color: result.sentiment === 'positive' ? 'green' : 'red' }">
          {{ result.sentiment }}
        </span>
      </p>
      <p><strong>Probability:</strong> {{ (result.probability * 100).toFixed(2) }}%</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      text: "",
      result: null,
      loading: false,
    };
  },
  methods: {
    async analyzeSentiment() {
      if (!this.text) return;
      this.loading = true;
      this.result = null;
      try {
        const response = await fetch("https://ai-sentiment-api-2euy.onrender.com/predict", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({ text: this.text }),
        });
        this.result = await response.json();
      } catch (err) {
        console.error("Error:", err);
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

<style>
.app {
  max-width: 600px;
  margin: auto;
  padding: 2rem;
  font-family: Arial, sans-serif;
}

textarea {
  width: 100%;
  margin-bottom: 1rem;
  padding: 0.5rem;
}

button {
  padding: 0.5rem 1rem;
  background: #42b883;
  border: none;
  color: white;
  cursor: pointer;
  border-radius: 5px;
}

button:hover {
  background: #369d6d;
}

h2 {
  margin-top: 1.5rem;
}
</style>