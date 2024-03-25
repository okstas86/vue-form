<template>
	<div class="form__conainer">
		<form class="form" @submit.prevent="submitForm" v-if="!registrationSuccess">
			<h2 class="form__title">Регистрация</h2>
			<hr />
			<p class="form__subtitle">Заполните Ваши данные</p>
			<div class="form__inputBox">
				<div>
					<input
						type="text"
						id="username"
						v-model="formData.username"
						class="form__input"
						:class="{ error: errors.username }"
						placeholder="Имя"
						@input="clearError('username')"
					/>
					<p v-if="errors.username" class="error-message">
						Пожалуйста, введите ваше имя
					</p>
				</div>
				<div>
					<input
						type="email"
						id="email"
						v-model="formData.email"
						class="form__input"
						:class="{ error: errors.email }"
						placeholder="Email"
						@input="clearError('email')"
					/>

					<p v-if="errors.email" class="error-message">
						Пожалуйста, введите корректный email
					</p>
				</div>
			</div>

			<div class="form__inputBox middle">
				<div class="form__select_box">
					<TheSelect
						:options="options"
						@select="optionSelect"
						:error="errors.role"
					/>
					<p v-if="errors.role" class="error-message">
						Пожалуйста, выберите должность
					</p>
				</div>
			</div>

			<div class="form__inputBox bottom">
				<div class="wrap">
					<input
						:type="showPassword ? 'text' : 'password'"
						id="password"
						v-model="formData.password"
						class="form__input"
						:class="{ error: errors.password }"
						placeholder="Пароль"
						@input="clearError('password')"
					/>
					<span v-if="errors.password" class="error-message"
						>Пожалуйста, введите пароль</span
					>
					<button
						type="button"
						class="form__input_btn"
						@click="toggleShowPassword(true)"
					>
						<img v-if="showPassword" src="../assets/notvisible.svg" alt="" />
						<img v-else src="../assets/visible.svg" alt="" />
					</button>
				</div>
				<div class="wrap">
					<input
						:type="showRepeatPassword ? 'text' : 'password'"
						id="password_repeat"
						v-model="formData.password_repeat"
						class="form__input"
						:class="{ error: errors.password_repeat }"
						placeholder="Повторите пароль"
						@input="clearError('password_repeat')"
					/>
					<p v-if="errors.password_repeat" class="error-message">
						Пожалуйста, повторите пароль
					</p>
					<button
						type="button"
						class="form__input_btn"
						@click="toggleShowPassword(false)"
					>
						<img
							v-if="showRepeatPassword"
							src="../assets/notvisible.svg"
							alt=""
						/>
						<img v-else src="../assets/visible.svg" alt="" />
					</button>
				</div>
			</div>
			<hr />
			<div class="form__checkbox_container">
				<div class="form__checkbox_wrap">
					<label class="switch">
						<input type="checkbox" v-model="isChecked" @change="handleChange" />
						<span class="slider round"></span>
					</label>
					<label class="form__checkbox_label" for="agreement"
						>Хотите чтобы Ваш профиль видели другие участники платформы?</label
					>
				</div>
				<p class="form__checkbox_p parag">
					Включает профиль для просмотра другими пользователями по ссылке
				</p>
			</div>
			<div class="form__inputBox">
				<div class="form__checkbox_wrap">
					<input
						:class="{ error: errors.agreement }"
						type="checkbox"
						id="agreement"
						v-model="formData.agreement"
					/>
					<label class="form__checkbox_p" for="agreement"
						>Регистрируясь, Вы соглашаетесь с
						<span>политикой конфиденциальности</span> и обработкой
						<span>персональных данных</span></label
					>
				</div>
				<button class="form__btn" type="submit.prevent">
					Зарегистрироваться
				</button>
			</div>
		</form>
		<div v-if="registrationSuccess" class="success-message">
			Регистрация успешно завершена
		</div>
	</div>
</template>

<script>
import TheSelect from "./TheSelect.vue"

export default {
	components: { TheSelect },
	data() {
		return {
			formData: {
				username: "",
				email: "",
				role: null,
				agreement: false,
				password: "",
				password_repeat: "",
			},
			selectedRole: "",
			errors: {},
			registrationSuccess: false,
			isChecked: false,
			showPassword: false,
			showRepeatPassword: false,
			options: [
				{ name: "Frontend Developer", value: "Frontend Developer" },
				{ name: "Backend Developer", value: "Backend Developer" },
				{ name: "HR", value: "HR" },
				{ name: "UI Designer", value: "UI Designer" },
				{ name: "System Admin", value: "System Admin" },
			],
		}
	},
	methods: {
		changeColor(e) {
			const color = e.value
			e.style.color = color
		},
		validateForm() {
			this.errors = {}
			if (!this.formData.username) this.errors.username = true
			if (!this.formData.email || !this.validEmail(this.formData.email))
				this.errors.email = true
			if (this.formData.role === null) this.errors.role = true

			if (!this.formData.password) this.errors.password = true
			if (
				!this.formData.password_repeat ||
				this.formData.password_repeat !== this.formData.password
			)
				this.errors.password_repeat = true
			if (!this.formData.agreement) this.errors.agreement = true

			return Object.keys(this.errors).length === 0
		},

		validEmail(email) {
			return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
		},
		submitForm() {
			if (!this.validateForm()) return

			console.log("Отправка данных:", this.formData)

			this.registrationSuccess = true
		},
		handleChange() {
			console.log("Состояние переключателя:", this.isChecked)
		},
		clearError(field) {
			this.errors[field] = false
		},
		toggleShowPassword(isPasswordField) {
			if (isPasswordField) {
				this.showPassword = !this.showPassword
			} else {
				this.showRepeatPassword = !this.showRepeatPassword
			}
		},
		optionSelect(option) {
			this.errors.role = false
			this.formData.role = option
		},
	},
}
</script>

<style>
.form__conainer {
	width: 100%;
	height: 100vh;
	display: flex;
	align-items: center;
	justify-content: center;
}
.form {
	width: 96rem;
	height: 54.7rem;
	padding: 2rem 0;
	box-shadow: 0 2.4rem 4.8rem rgba(0, 0, 0, 0.1);
	border-radius: 1.5rem;
}
.form__title {
	font-size: 1.9rem;
	font-weight: 700;
	margin-bottom: 1rem;
	margin-left: 3rem;
}
hr {
	height: 1px;
	color: #d9d9d9;
	background-color: #d9d9d9;
	border: none;
}
.form__subtitle {
	font-size: 1.6rem;
	font-weight: 500;
	margin: 2rem 0 4rem 3rem;
}

.form__inputBox {
	display: flex;
	justify-content: space-between;
	margin: 3rem 0;
	padding: 0 2rem 0 3rem;
}
.middle {
	justify-content: flex-end;
}
.form__select_box {
	position: relative;
}
.form__input {
	width: 45rem;
	height: 3.9rem;
	border-radius: 1.1rem;
	border: 1px solid #e6e6eb;
	padding-left: 1rem;
	font-size: 1.4rem;
	font-weight: 400;
	font-family: "Montserrat", sans-serif;
}

.form__input::placeholder {
	color: #9292a0;
}

.form__input:focus {
	outline: none;
}

.wrap {
	position: relative;
}

.bottom {
	display: flex;
	gap: 1rem;
}
.form__input_btn {
	position: absolute;
	top: 1.3rem;
	right: 1rem;
	border: none;
	background-color: transparent;
}

.form__checkbox_container {
	padding: 1rem 2rem 0 3rem;
}
.form__checkbox_wrap {
	display: flex;
	align-items: center;
	gap: 1rem;
	margin: 1rem 0;
}

.form__checkbox_label {
	font-size: 1.6rem;
	font-weight: 500;
}
.form__checkbox_p {
	font-size: 1.4rem;
	font-weight: 400;
}
.parag {
	color: #696977;
}
.form__checkbox_p span {
	color: #497ada;
}

.form__btn {
	width: 30.2rem;
	height: 4rem;
	border-radius: 0.8rem;
	color: #497ada;
	background-color: rgba(73, 121, 218, 0.2);
	cursor: pointer;
	border: none;
}

.switch {
	position: relative;
	display: inline-block;
	width: 39px;
	height: 19px;
}

.switch input {
	opacity: 0;
	width: 0;
	height: 0;
}

.slider {
	position: absolute;
	cursor: pointer;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background-color: #ccc;
	-webkit-transition: 0.4s;
	transition: 0.4s;
}

.slider:before {
	position: absolute;
	content: "";
	height: 19px;
	width: 19px;
	left: 0px;
	bottom: 0px;
	background-color: white;
	-webkit-transition: 0.4s;
	transition: 0.4s;
}

input:checked + .slider {
	background-color: #3586ff;
}

input:focus + .slider {
	box-shadow: 0 0 1px #3586ff;
}

input:checked + .slider:before {
	-webkit-transform: translateX(19px);
	-ms-transform: translateX(19px);
	transform: translateX(19px);
}

.slider.round {
	border-radius: 34px;
}

.slider.round:before {
	border-radius: 50%;
}
</style>
