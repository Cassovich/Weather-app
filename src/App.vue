<template>
<div id="app">
    <main>
        <div class="intro">
            <h1>What's the weather like?</h1>
            <p>Type in your desired city to see the how the weather is right now</p>
        </div>
        <div class="search-box">
            <input 
                type="text" 
                class="search-bar" 
                placeholder="Search..."
                v-model="query"
                v-on:keypress="fetchWeather" 
            />
        </div>

        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'"> 
            <div class="location-box">
                <div class="location">{{ weather.name }}</div>
                <!-- <div class="date">Monday 20 January 2023</div> -->
            </div>

            <div class="weather-box">
                <div class="temp"> {{ Math.round(weather.main.temp) }}°c</div>
                <div class="weather"> {{ weather.weather[0].main }} </div>
            </div>
        </div>
    </main>
</div>
</template>

<script>
export default {
    name: 'app',
    data () {
        return{
            api_key: '31696ef48c350dac29ed9ba8c530f115',
            url_base: 'http://api.openweathermap.org/data/2.5/',
            query: '',
            weather: {}
        }
    },
    // ${this} referer til mine variabler der står ovenfor
    // this.query er knyttet til the search bar
    // & tegnet betyder at man begynder på en ny parameter
    // units=metric betyder vi gerne vil have celcius
    methods: {
        fetchWeather (e) {
            if (e.key == "Enter") {
                fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
                    .then(response => {
                        return response.json();
                    }).then(this.setResults);
            }
        },
        setResults (results) {
            this.weather = results;
        }
    }
}
</script>

