<template>
	<div className="wrap">
		<h1>Погода</h1>
		<p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
		<input type="text" v-model="city" placeholder="Введите город">
		<button v-if="city != ''" @click="getWeather()">Получить погоду</button>
		<button disabled v-else>Получить погоду</button>
		<p className="error">{{ error }}</p>
		<div v-if="info != null">
			<p>{{ showTemp }}</p>
			<p>{{ showFeelsLike }}</p>
			<p>{{ showMinTemp }}</p>
			<p>{{ showMaxTemp }}</p>
			<p>{{ showWind }}</p>
		</div>
	</div>
</template>

<script>
import axios from 'axios';

	export default {
		data() {
			return {
				city: "",
				error: "",
				info: null
			}
		},
		computed: {
			cityName() {
				return "«" + this.city + "»"
			},
			showTemp() {
				return "Температура: " + this.info.main.temp + "℃"
			},
			showFeelsLike() {
				return "Ощущается как: " + this.info.main.feels_like + "℃"
			},
			showMinTemp() {
				return "Минимальная температура: " + this.info.main.temp_min + "℃"
			},
			showMaxTemp() {
				return "Максимальная температура: " + this.info.main.temp_max + "℃"
			},
			showWind() {
				return "Ветер: " + this.info.wind.speed + "м/с"
			},
		},
		methods: {
			getWeather() {
				if (this.city.trim().length < 2) {
					this.error = "Нужно название более одного символа"
					return false
				}
				this.error = "";

				axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=7b935acd7f486e8e6a1944effb30019a`)
					.then(res => (this.info = res.data))
			}
		}
}
</script>

<style scoped>
	.wrap {
		width: 900px;
		height: 500px;
		padding: 20px;
		border-radius: 50px;
		background: #1f0f24;
		text-align: center;
		color: #fff;
	}
	.wrap h1 {
		margin-top: 50px;
	}
	.wrap p {
		margin-top: 20px;
	}
	.wrap input {
		margin-top: 30px;
		background: transparent;
		border: 0;
		border-bottom: 2px solid #110813;
		color: #fcfcfc;
		font-size: 14px;
		padding: 5px 8px;
		outline: none;
	}
	.wrap input:focus {
		border-bottom-color: #6e2d7d;
	}
	.wrap button {
		background: #e3bc4b;
		color: #fff;
		border-radius: 10px;
		border: 2px solid #b99935;
		padding: 10px;
		margin-left: 20px;
		cursor: pointer;
		transition: transform 0.5s ease 0s;
	}
	.wrap button:hover {
		transform: scale(1.1) translateY(-5px);
	}
	.wrap button:active {
		transform: scale(1.1) translateY(0px);
	}
	.wrap button:disabled {
		background: #a3a3a3;
		color: #dfdede;
		border-radius: 10px;
		border: 2px solid #c0b8a1;
		padding: 10px;
		margin-left: 20px;
		cursor: pointer;
	}
	.error {
		color: #d03939;
	}
</style>
