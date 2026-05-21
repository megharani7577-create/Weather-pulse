# Weather-pulse
Weather Pulse is a modern weather forecasting web app that provides real-time weather updates using live API data. Users can search cities or use live location to check temperature, humidity, wind speed, and weather conditions with a responsive and attractive UI design.
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Live Weather App</title>

<style>

*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial,sans-serif;
}

body{
    height:100vh;
    display:flex;
    justify-content:center;
    align-items:center;
    background:linear-gradient(135deg,#0f172a,#1e293b,#334155);
}

.container{
    width:350px;
    padding:25px;
    border-radius:25px;
    background:rgba(255,255,255,0.1);
    backdrop-filter:blur(15px);
    box-shadow:0 8px 32px rgba(0,0,0,0.3);
    color:white;
    text-align:center;
}

h1{
    margin-bottom:20px;
    font-size:32px;
}

.search-box{
    display:flex;
    gap:10px;
    margin-bottom:15px;
}

.search-box input{
    flex:1;
    padding:12px;
    border:none;
    border-radius:12px;
    outline:none;
    font-size:16px;
}

.search-box button{
    padding:12px 15px;
    border:none;
    border-radius:12px;
    background:#38bdf8;
    color:white;
    font-size:16px;
    cursor:pointer;
}

.search-box button:hover{
    background:#0ea5e9;
}

.location-btn{
    width:100%;
    padding:12px;
    border:none;
    border-radius:12px;
    background:#22c55e;
    color:white;
    font-size:16px;
    cursor:pointer;
    margin-bottom:20px;
}

.location-btn:hover{
    background:#16a34a;
}

.weather-icon{
    width:120px;
    margin:10px auto;
}

.temp{
    font-size:55px;
    font-weight:bold;
}

.city{
    font-size:30px;
    margin-top:5px;
}

.description{
    font-size:18px;
    margin-top:8px;
    text-transform:capitalize;
}

.details{
    display:flex;
    justify-content:space-between;
    margin-top:25px;
}

.card{
    width:45%;
    padding:15px;
    border-radius:15px;
    background:rgba(255,255,255,0.15);
}

.card h3{
    font-size:16px;
    margin-bottom:10px;
}

.card p{
    font-size:20px;
    font-weight:bold;
}

.error{
    margin-top:15px;
    color:#ffb4b4;
}

</style>
</head>

<body>

<div class="container">

    <h1>Weather App</h1>

    <div class="search-box">
        <input type="text" id="cityInput" placeholder="Enter city name">
        <button onclick="getWeatherByCity()">Search</button>
    </div>

    <button class="location-btn" onclick="getLiveLocationWeather()">
        Use Live Location
    </button>

    <img src="https://cdn-icons-png.flaticon.com/512/1163/1163661.png"
    class="weather-icon"
    id="weatherIcon">

    <div class="temp" id="temp">--°C</div>

    <div class="city" id="city">City Name</div>

    <div class="description" id="description">
        Weather Description
    </div>

    <div class="details">

        <div class="card">
            <h3>Humidity</h3>
            <p id="humidity">--%</p>
        </div>

        <div class="card">
            <h3>Wind</h3>
            <p id="wind">-- km/h</p>
        </div>

    </div>

    <div class="error" id="error"></div>

</div>

<script>

const apiKey = "0f97f6fb0cbf91fdd1852115fd21adeb";

async function showWeather(data){

document.getElementById("temp").innerHTML =
Math.round(data.main.temp) + "°C";

document.getElementById("city").innerHTML =
data.name;

document.getElementById("description").innerHTML =
data.weather[0].description;

document.getElementById("humidity").innerHTML =
data.main.humidity + "%";

document.getElementById("wind").innerHTML =
data.wind.speed + " km/h";

const weatherMain = data.weather[0].main;

const icon = document.getElementById("weatherIcon");

if(weatherMain == "Clouds"){
icon.src =
"https://cdn-icons-png.flaticon.com/512/414/414825.png";
}

else if(weatherMain == "Clear"){
icon.src =
"https://cdn-icons-png.flaticon.com/512/869/869869.png";
}

else if(weatherMain == "Rain"){
icon.src =
"https://cdn-icons-png.flaticon.com/512/3351/3351979.png";
}

else if(weatherMain == "Snow"){
icon.src =
"https://cdn-icons-png.flaticon.com/512/642/642102.png";
}

else{
icon.src =
"https://cdn-icons-png.flaticon.com/512/1163/1163661.png";
}

}

async function getWeatherByCity(){

const city =
document.getElementById("cityInput").value;

if(city === ""){
document.getElementById("error").innerHTML =
"Please enter city name";
return;
}

try{

const apiUrl =
`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;

const response = await fetch(apiUrl);

const data = await response.json();

if(data.cod == "404"){

document.getElementById("error").innerHTML =
"City not found";

return;
}

document.getElementById("error").innerHTML = "";

showWeather(data);

}

catch(error){

document.getElementById("error").innerHTML =
"Something went wrong";

}

}

async function getLiveLocationWeather(){

if(navigator.geolocation){

navigator.geolocation.getCurrentPosition(async(position)=>{

try{

const lat = position.coords.latitude;
const lon = position.coords.longitude;

const apiUrl =
`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${apiKey}&units=metric`;

const response = await fetch(apiUrl);

const data = await response.json();

document.getElementById("error").innerHTML = "";

showWeather(data);

}

catch(error){

document.getElementById("error").innerHTML =
"Location weather failed";

}

});

}

else{

document.getElementById("error").innerHTML =
"Geolocation not supported";

}

}

</script>

</body>
</html>
