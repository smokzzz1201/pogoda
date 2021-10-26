<template>
   <div >
      <div style="margin-left:5%;">
       <h1>Погода</h1>

       <h3>Добавить город</h3>
             <input type="text" style = "width:300px;font-size:25px;" v-model="gorodAdd"> <button v-on:click="addWCity" >Добавить город </button>
                    <pre> </pre>
                     <h3>Выбрать город</h3>
               <select v-model="gorod" style = "width:300px;font-size:25px;">
                        <option  v-for="city in cities" v-bind:key="city" v-bind:value="city">{{ city}}</option>
                    </select>
                    <button v-on:click="getWeather">Посмотреть погоду</button>
            
      </div>
  <pre> </pre>
  
       <div v-if="weather.cod >0" style="margin-left:40%;margin-top:-13%; font-size:25px;">
           <tr ><h3>Информация о погоде: </h3></tr>
               <tr>Страна: {{weather.sys.country}}</tr>   
              <tr>Город: {{gorod}}</tr>
             <tr>Долгота: {{weather.coord.lon}} °</tr>   
              <tr>Широта : {{weather.coord.lat}} °</tr> 
              <tr>  Облачность: {{weather.weather[0].main}}</tr>    
               <tr>  Описание: {{weather.weather[0].description}}</tr>    
               <tr> Температура: {{weather.main.temp-273.15 | roundValue}} C</tr>
               <tr>  Ощущается как: {{weather.main.feels_like-273.15 | roundValue}} C</tr>
               <tr>  Минимальная Температура: {{weather.main.temp_min-273.15 | roundValue}} C</tr>
               <tr>   Максимальная Температура: {{weather.main.temp_max-273.15 | roundValue}} C</tr>
               <tr>Давление: {{weather.main.pressure}} P</tr>
                 <tr>Облачность: {{weather.clouds.all}}</tr> 
               <tr>Скорость ветра: {{weather.wind.speed}} А</tr>
                   <tr>Направление ветра: {{weather.wind.deg}} °</tr>
            
       </div>
   </div>
</template>

<script>
    import Vue from 'vue'
    import axios from 'axios'
    import VueAxios from 'vue-axios'
 
   export default {
        data: function() {
           return {
           cities:[],
           weather:[],
           gorodAdd:"",
           gorod:"",
        }},
        mounted: function(){

        if(localStorage.getItem("cities") !== '') this.cities = JSON.parse(localStorage.getItem("cities")|| "[]");
                        
        },
        methods:{
         
            getWeather: function() {
                 axios.get("https://api.openweathermap.org/data/2.5/weather?q="+this.gorod+"&appid=7914d5a440960cfd5df3bd0388a7ad0f", {
                          units: "metric",
                })
                .then((response)=>{
                    console.log(response.data);
                    this.weather = response.data;
                    
                })
            },
            addWCity: function(){
                this.cities.push(this.gorodAdd)
                localStorage.setItem("cities",JSON.stringify(this.cities))
            },
        
        },
        filters:{
            roundValue: function(value){
                return parseFloat(value.toFixed(2));
            },
        }
    }
</script>
