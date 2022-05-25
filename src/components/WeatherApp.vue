<template>
    <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 20 ? 'hotday' : '' ">
        <div class="search-box">
            <input 
            type="text" 
            class="search-bar"
            placeholder="Search...."
            v-model="query"
            @keypress="fetchWeather"
            >
        </div>

        <div class="weather-warp" v-if=" typeof weather.main != 'undefined'" >
            <div class="location-box">
                <div class="location">
                    {{weather.name}}, {{weather.sys.country}}
                </div>
                <div class="date">
                    {{getDate()}}
                </div>
            </div>

            <div class="weather-box">
                <div class="temp">{{Math.floor(weather.main.temp)}}&#176;C</div>
                <div class="status">{{weather.weather[0].main}}</div>
            </div>

        </div>

    </div>

</template>

<script>
    export default {
        name : 'weatherApp',
        data(){
            return{
                query : '',
                urlBase : 'http://api.openweathermap.org/data/2.5/',
                apiKey : 'f0bc3bd7b0b25b085386e36ad01bbc79' ,
                weather : {}
            }
        },
        methods: {
            fetchWeather(e){
                if(e.key == "Enter"){
                    fetch(`${this.urlBase}weather?q=${this.query}&units=metric&APPID=${this.apiKey} `)
                        .then(res =>{
                            return res.json();
                        }).then(this.setResult);
                }
            },
            setResult(results){
                this.weather = results;
                console.log(this.weather);
            },
            getDate(){
                let d = new Date();
                let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
                let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
                
                let day = days[d.getDay()];
                let date = d.getDate();
                let month = months[d.getMonth()]
                let year = d.getFullYear();

                return `${day} ${date} ${month} ${year}`
            }
        }
    }
</script>

<style scoped>

#app{
    background-image: linear-gradient( to bottom ,  rgba(0,0,0,0.25) , rgba(0,0,0,0.5)) , url(../assets/cold-bg.jpg);
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
    height: 100vh;
}

#app.hotday{
    background-image: linear-gradient( to bottom ,  rgba(0,0,0,0.25) , rgba(0,0,0,0.5)) , url(../assets/warm-bg.jpg);
}

.search-box{
    padding: 2rem;
}

.search-box .search-bar{
    padding: 10px;
    width: 100%;
    border-radius: 0 10px 0 10px ;
    transition: 0.4s;

    border: none;
    outline: none;
    appearance: none;
    background: none;
    background-color: rgba(225, 225, 225,1);
    box-shadow: 3px 6px 8px  rgba(0,0,0,0.1);
}

.search-box .search-bar:focus{
    border: none;
    border-radius: 10px 0 10px 0;
    box-shadow: 3px 6px 16px  rgba(0,0,0,0.1);

}

.weather-warp{
    text-align: center;
}

.location-box{
    margin-bottom: 20px;
}

.location-box .location{
    color: white;
    font-size: 36px;
    text-shadow: 0 0 2px rgba(0,0,0,0.8);
    font-family:Arial, Helvetica, sans-serif;
    font-weight: 600;
}

.location-box .date{
    font-size: 20px;
    font-style: italic;
    color: white;
    font-weight: 300;
}

.weather-box .temp{
    padding: 10px 20px;
    font-size: 42px;
    width: 150px;
    border-radius: 12px;
    background-color: rgba(225,225,225,0.5);
    margin: 20px auto;
    color: rgb(255, 255, 255);
    font-weight: bold;
}

.weather-box .status{
    font-size: 30px;
    color: rgb(255, 255, 255);
    font-style: italic;
    font-weight: bold;
}

</style>