<template>
  <div class="wrapper">
    <div class="search">
      <label for="search">Search</label>
      <input
        type="text"
        name="search"
        v-model="searchValue"
        @input="handleInput"
      />
    </div>
    <h2 v-if="this.loading">Loader</h2>
    <ul>
      <li v-for="item in results" :key="item.data[0].nasa_id">
        <p>{{item.data[0].description}}</p>
      </li>
    </ul>
  </div>
</template>


<script>

import axios from 'axios';
import debounce from 'lodash.debounce';

const API = 'https://images-api.nasa.gov';

export default {
  name: 'Search',
  data() {
    return {
      searchValue: '',
      results: [],
      loading: false,
    };
  },
  methods: {
    // eslint-disable-next-line
    handleInput: debounce(function () {
      this.loading = true;
      axios.get(`${API}/search?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
          this.loading = false;
        }).catch((error) => {
          console.log(error);
        });
    }, 500),
  },
};
</script>

<style lang="scss" scoped>
  .wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 0;
    padding: 30px;
    width: 100%;
  }

  .search {
    display: flex;
    width: 300px;
    flex-direction: column;
    width: 250px;

    label {
      font-family: Montserrat, sans-serif;
    }

    input {
      height: 30px;
      border: 0;
      border-bottom: 1px solid #000;
    }
  }
</style>

