<template>
  <div class="wrapper">
    <div class="search">
      <button @click="getRestaurants">GET RESTAURANT</button>
      <ul>
      <li v-for="item in restaurants" :key="item.id">
        <p>{{ item.name }}</p>
      </li>
    </ul>
      <label for="search">Search</label>
      <input
      id="search"
      name="search"
      v-model="searchValue"
      @input="handleInput" />
    <ul>
      <li v-for="item in results" :key="item.data[0].nasa_id">
        <p>{{ item.data[0].description }}</p>
      </li>
    </ul>
    </div>
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
      restaurants: [],
    };
  },
  methods: {
    // npm run lint -- --fix
    getRestaurants() {
      axios.get('https://localhost:5001/api/restaurant?pageSize=5&pageNumber=1')
        .then((response) => {
          console.log(response.data.items);
          this.restaurants = response.data.items;
        });
    },
    // eslint-disable-next-line
    handleInput: debounce(function() {
      axios.get(`${API}/search?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          console.log(response.data.collection.items);
          this.results = response.data.collection.items;
        })
        .catch((error) => {
          console.log(error);
        });
    }, 500),
  },
};
</script>

<style lang="scss" scoped>
  .wrapper
  {
    margin: 0;
    padding: 30px;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .search
  {
    display: flex;
    flex-direction: column;
    width: 300px;
  }
  label
  {
    font-family: Montserrat, sens-serif;
  }
  input{
    height: 30px;
    border: 0;
    border-bottom: 1px solid black;
  }
</style>
