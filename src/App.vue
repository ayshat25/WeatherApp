<template>
  <div class="weather" :class="wetherClass">
    <div class="container">
      <div class="card weather-form">
        <input type="text" class="weather-form_input" v-model="searchQuery" @keyup.enter="weatherSearch" placeholder="Enter city">
        <button class="weather-form_btn" @click="weatherSearch">Search</button>
      </div>

      <div class="card weather-load" v-if="loading">Loading...</div>

      <div class="weather-info" v-show="!error && location && temperature !== 0 && description">

        <div class="card" v-if="error">Error</div>

        <div class="weather-info_text">
          <p class="card">{{location}}</p>
          <p class="card">{{ temperature }}°C</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>
    </div>

    <div class="weather-bg">
      <div>
        <img src="./assets/img1.webp" alt="App Background" class="weather-bg_img bg">
        <img src="./assets/img2.jpg" alt="overcast" class="weather-bg_img overcast">
        <img src="./assets/img3.jpg" alt="partly-cloudy" class="weather-bg_img partly-cloudy">
        <img src="./assets/img4.webp" alt="sunny" class="weather-bg_img sunny">
      </div>
    </div>
  </div>
</template>

<script>

export default{
  data(){
    return{
      location: '',
      temperature: 0,
      description: '',
      loading: false,
      error: false,
      searchQuery: '',
    };
  },
  computed: {
    wetherClass(){
      if(this.description.includes('Sunny')){
        return 'sunny';
      }else if(this.description.includes('Overcast')){
        return 'overcast';
      }else if(this.description.includes('Partly cloudy')){
        return 'partly-cloudy';
      }else{
        return '';
      }
    }
  },
  methods: {
    weatherSearch(){
      this.loading = true;
      this.error = false;
      fetch(`http://api.weatherapi.com/v1/current.json?key=b21e338430534970bfb161554260301&q=${this.searchQuery}`)
      .then(response => response.json())
      .then(data => {
        this.loading = false;
        this.location = data.location.name;
        this.temperature = data.current.temp_c;
        this.description = data.current.condition.text;
        this.resetSearchQuery();
      })
      .catch(error =>{
        this.loading = false;
        this.error = true;
        console.error(error);
      });
    },
    resetSearchQuery(){
      this.searchQuery = '';
    }
  }
};

</script>