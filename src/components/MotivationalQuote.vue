<template>
    <div class="quote-container">
      <div class="quote-box">
        <div v-if="loading" class="loading-spinner">Loading...</div>
        <div v-if="error" class="error-message">
          <p>Oops! Something went wrong. Please try again later.</p>
        </div>
        <p class="quote-text" v-if="quote && !error">{{ quote }}</p>
        <p class="quote-author" v-if="author && !error">- {{ author }}</p>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  
  export default {
    setup() {
      const quote = ref('');
      const author = ref('');
      const loading = ref(true);
      const error = ref(false);
      const errorMessage = ref('');
      
      const fetchQuote = async () => {
        loading.value = true;
        error.value = false;
        errorMessage.value = '';
        
        try {
            const apiKey = import.meta.env.VITE_API_KEY;
          const response = await fetch('https://api.api-ninjas.com/v1/quotes?category=happiness', {
            method: 'GET',
            headers: { 'X-Api-Key': apiKey },
          });
  
          if (!response.ok) throw new Error(`HTTP error! Status: ${response.status}`);
  
          const data = await response.json();
  
          if (!data || !data[0] || !data[0].quote || !data[0].author) {
            throw new Error('Invalid data format received from API');
          }
  
          quote.value = data[0].quote;
          author.value = data[0].author;
        } catch (err) {
          error.value = true;
          errorMessage.value = `Error fetching quote: ${err.message || 'Unknown error'}`;
        } finally {
          loading.value = false;
        }
      };
  
      onMounted(fetchQuote);
  
      return { quote, author, loading, error, errorMessage, fetchQuote };
    },
  };
  </script>
  
  <style scoped>
  .quote-container {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
  }
  
  .quote-box {
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
    max-width: 600px;
    width: 100%;
  }
  
  .quote-text {
    font-size: 1.5rem;
    color: #333;
    font-weight: bold;
    margin-bottom: 10px;
  }
  
  .quote-author {
    font-size: 1.2rem;
    color: #777;
    margin-top: 10px;
  }
  
  .error-message {
    color: red;
    font-size: 1.2rem;
    margin-bottom: 10px;
    padding: 10px;
    background-color: #ffe6e6;
    border-radius: 5px;
    border: 1px solid red;
  }
  
  .loading-spinner {
    font-size: 1.2rem;
    font-weight: bold;
    color: #007BFF;
    margin-bottom: 10px;
  }
  
  button {
    background-color: #007BFF;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 1rem;
    margin-top: 20px;
  }
  
  button:hover {
    background-color: #0056b3;
  }
  
  .category-selector {
    margin-top: 20px;
  }
  
  .category-selector select {
    padding: 8px;
    font-size: 1rem;
    border-radius: 5px;
    margin-top: 10px;
  }
  
  @media (max-width: 600px) {
    .quote-text {
      font-size: 1.2rem;
    }
    .quote-author {
      font-size: 1rem;
    }
  }
  </style>
  