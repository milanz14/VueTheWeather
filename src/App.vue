<template>
    <div id="App">
        <main>
            <h1 class="title">Vue The Weather</h1>
            <div class="search-box">
                <label class="label">Enter location</label>
                <input
                    type="text"
                    name="search-bar"
                    id="search-bar"
                    class="search-bar"
                    placeholder="Search... press enter"
                    v-model="search"
                    @keypress="fetchWeather"
                />
            </div>

            <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
                <div class="location-box">
                    <div class="location">
                        {{ weather.name }},{{ weather.sys.country }}
                    </div>
                    <div class="date">{{ dateBuilder() }}</div>
                </div>
                <div class="weather-box">
                    <div class="temperature">
                        {{ Math.round(weather.main.temp) }}&#8451;
                    </div>
                    <div class="weather-status">
                        {{ weather.weather[0].main }}
                    </div>
                </div>
            </div>
        </main>
    </div>
</template>

<script>
export default {
    name: "App",
    data() {
        return {
            api_key: "20a288da336624707ee975473953c012",
            url_base: "https://api.openweathermap.org/data/2.5/",
            search: "",
            weather: {},
        };
    },
    methods: {
        fetchWeather(e) {
            if (e.key === "Enter") {
                fetch(
                    `${this.url_base}weather?q=${this.search}&units=metric&APPID=${this.api_key}`
                )
                    .then((res) => {
                        const json = res.json();
                        console.log(json);
                        return json;
                    })
                    .then(this.setResults)
                    .then(this.clearInput);
            }
        },
        setResults(results) {
            this.weather = results;
        },
        clearInput() {
            this.search = "";
        },
        dateBuilder() {
            let d = new Date();
            let months = [
                "jan",
                "feb",
                "mar",
                "apr",
                "may",
                "jun",
                "jul",
                "aug",
                "sep",
                "oct",
                "nov",
                "dec",
            ];
            let days = ["sun", "mon", "tue", "wed", "thu", "fri", "sat"];

            let day = days[d.getDay()];
            let date = d.getDate();
            let month = months[d.getMonth()];
            let year = d.getFullYear();

            return `${day} ${date} ${month} ${year}`;
        },
    },
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: "montserrat", sans-serif;
}

#app {
    background-image: url("https://images.unsplash.com/photo-1592210454359-9043f067919b?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1740&q=80");
    background-size: cover;
    background-position: bottom;
}

main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(
        to bottom,
        rgba(0, 0, 0, 0.25),
        rgba(0, 0, 0, 0.75)
    );
}

.search-box {
    width: 100%;
    margin-bottom: 30px;
}

.label {
    color: white;
    font-size: 20px;
    padding: 10px;
    margin-bottom: 12px;
}

.search-box .search-bar {
    display: block;
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
    background: none;
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 12px 0px 12px;
    transition: 0.4s;
}

.search-box .search-bar:focus {
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
    border-radius: 12px 0px 12px 0px;
}

.location-box .location {
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.weather-box {
    text-align: center;
}

.weather-box .temperature {
    display: inline-block;
    padding: 10px 25px;
    color: #fff;
    font-size: 102px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px 0px;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather-status {
    color: #fff;
    font-size: 48px;
    font-weight: bold;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.title {
    text-align: center;
    color: white;
    margin-bottom: 15px;
}
</style>
