<template>
  <div class="container mt-4">
    <h1>Submit a Joke</h1>
    <form @submit.prevent="submitJoke">
      <div class="form-group">
        <label for="setup">Setup</label>
        <input type="text" class="form-control" id="setup" v-model="joke.setup" required>
      </div>
      <div class="form-group">
        <label for="punchline">Punchline</label>
        <input type="text" class="form-control" id="punchline" v-model="joke.punchline" required>
      </div>
      <div class="form-group">
        <label for="type">Type</label>
        <select class="form-control" id="type" v-model="joke.type" required>
          <option v-for="type in jokeTypes" :value="type">{{ type }}</option>
        </select>
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'JokeForm',
  data() {
    return {
      joke: {
        setup: '',
        punchline: '',
        type: ''
      },
      jokeTypes: []
    };
  },
  created() {
    // Load types from API
    axios.get('http://localhost:3000/jokes/types')
      .then(response => {
        this.jokeTypes = response.data.map(jokeType => {
          return jokeType.name
        });
      }).catch(error => {
        console.error(error);
      });
  },
  methods: {
    submitJoke() {
      axios.post('http://localhost:3001/jokes/submit', this.joke)
        .then(response => {
          console.log(response.data);
          // Clear form
          this.joke.setup = '';
          this.joke.punchline = '';
          this.joke.type = '';
          // Show success message
          alert('Joke submitted!');
        })
        .catch(error => {
          console.error(error);
          // Show error message
          alert('Failed to submit joke.');
        });
    }
  }
};
</script>
