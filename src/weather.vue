<template>
    <div class="m-0" :class="imageSelection">
        <div class="row m-0">
            <div class="col-lg-4 offset-lg-4 col-md-6 offset-md-3 col-sm-8 offset-sm-2 col-10 offset-1">
                <input type="text" class="search form-control" placeholder="search..." v-model="query" @keyup.enter="fetchClimate"/>
            </div>            
        </div>
        
        <div v-if="country">
            <div class="country text-center text-dark mt-5">{{ sName }}, {{ country }}</div>
            <div class="time text-center text-secondary mt-4">{{ localTime | dateFilter }}</div>
            <div class="col-lg-2 col-md-3 col-sm-4 col-4 card shadow text-center mx-auto mt-5">
                <div class="card-body">
                    {{ temp }}°c
                </div>
            </div>
            <div class="weathertype text-center mt-4"><img :src="weatherIcon"> {{ weather }}</div>  
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data(){
        return{
            query: "",
            temp: null,
            weather: "",
            weatherIcon: "",
            sName: "",
            country: "",
            localTime: ""
        } 
    },
    methods: {
        fetchClimate(){
            axios.get("http://api.weatherapi.com/v1/current.json?key=9edb0764268f49a1b8264554202210&q=" + this.query)
            .then(res => {
                console.log(res);
                this.temp = Math.round(res.data.current.temp_c);
                this.weather = res.data.current.condition.text;
                this.weatherIcon = res.data.current.condition.icon;
                this.sName = res.data.location.name;
                this.country = res.data.location.country;
                this.localTime = res.data.location.localtime;
            })
            .catch(error => console.log(error))
        }
    },
    computed: {
        imageSelection(){
            if(this.temp == null){
                return "light";
            }
            else if(this.temp < 20){
                return "winter";
            }
            else if(this.temp >= 20){
                return "summer";
            }
        }
    },
    filters: {
        dateFilter(val){
            const localDate = new Date(val);
            const day = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
            const month = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
            const d = day[localDate.getDay()];
            const date = localDate.getDate();
            const m = month[localDate.getMonth()];
            const y = localDate.getFullYear(); 
            return d + ", " + date + " " + m + ", " + y;
        }
    }
}
</script>

<style scoped>
.country{
    font-weight: bold;
    font-size: x-large;
    margin-top: 30px;
}
.time{
    margin-top: 20px;
    font-weight: bold;
    font-size: large;
}
.card{
    width: auto;
    background-color: transparent;
    font-weight: bold;
    font-size: 45px;
}
.weathertype{
    font-size: 28px;
    font-weight: bold;
    font-style: italic;
}
.search{
    font-weight: 500;
    margin-top: 50px;
    width: 100%;
    border: transparent;
    padding: 20px;
    outline: none;
    background: none;
    background-color: rgba(255, 255, 255, 0.75);
    transition: 0.5s;
    border-radius: 10px 0px 10px 0px;
}
.search:focus{
    font-weight: 500;
    margin-top: 50px;
    width: 100%;
    border: transparent;
    padding: 20px;
    outline: none;
    background: none;
    background-color: rgba(255, 255, 255, 1);
    transition: 0.5s;
    border-radius: 0px 10px 0px 10px;
}
.light{
    background-image: url("./assets/light.jpg");
    height: 100vh;
    width: 100%;
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    transition: 0.5s;
}
.summer{
    background-image: url("./assets/summer.jpg");
    height: 100vh;
    width: 100%;
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    transition: 0.5s;
    color: white;
}
.winter{
    background-image: url("./assets/winter.jpg");
    height: 100vh;
    width: 100%;
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    transition: 0.5s;
    color: white;
}

@media (max-width: 550px){
    .card-body{
        font-size: 20px;
        text-align: center;
    }
}
</style>