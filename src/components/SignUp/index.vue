<template>
	<form class="registration">
		<h1 class="registration--title">Регистрация</h1>
		<p class="registration--subtitle">
			Уже есть аккаунт?
			<router-link to="/sign-in">Войти</router-link>
		</p>
		<div class="registration--name">
			<label for="registration--name">Имя</label>
			<input
				id="registration--name"
				type="text"
				placeholder="Введите Ваше имя"
				v-model="name"
				@input="validate('name')"
			/>
			<span class="registration--input-invalid"
				>Может содержать только буквы, пробелы, дефис</span
			>
		</div>
		<div class="registration--email">
			<label for="registration--email">Email</label>
			<input
				id="registration--email"
				type="mail"
				placeholder="Введите ваш email"
				v-model="email"
				@input="validate('email')"
			/>
			<span class="registration--input-invalid">Введите корректный email</span>
		</div>
		<div class="registration--tel">
			<label for="registration--tel">Номер телефона</label>
			<input
				id="registration--tel"
				type="text"
				placeholder="Введите номер телефона"
				v-model="tel"
				@input="validate('tel')"
			/>
			<span class="registration--input-invalid"
				>Одиннадцать цифр. Возможен плюс, дефис, скобки</span
			>
		</div>
		<label for="registration--lang">Язык</label>
		<select-custom
			class="registration--select"
			id="registration--lang"
			:options="langs"
			@select="val => (lang = val)"
		></select-custom>

		<div class="registration--confirm">
			<input
				type="checkbox"
				id="registration--confirm"
				@change="isConfirmed = !isConfirmed"
			/>
			<p>
				Принимаю
				<router-link to="/use-terms">условия</router-link> использования
			</p>
		</div>
		<input
			class="registration--submit"
			type="submit"
			value="Зарегистрироваться"
			:disabled="
				!(
					Object.values(this.isValid).reduce((sum, el) => (sum += el)) === 3 &&
					lang &&
					isConfirmed
				)
			"
			@click.prevent
		/>
	</form>
</template>

<script>
import Select from '@/components/Select'
export default {
	components: {
		'select-custom': Select
	},
	data() {
		return {
			langs: ['Русский', 'Английский', 'Китайский', 'Испанский'],
			name: '',
			email: '',
			tel: '',
			lang: '',
			isConfirmed: false,
			isValid: {
				name: 0,
				email: 0,
				tel: 0
			}
		}
	},
	methods: {
		// I ❤ Vuelidate...
		validate(type) {
			const elem = document.querySelector(`.registration--${type}`)
			const input = elem.querySelector('input')
			const invalidMessage = elem.querySelector('.registration--input-invalid')
			const types = {
				name: /^[A-Za-zА-Яа-я -]+$/,
				email: /^([A-Za-zА-Яа-я0-9_\-\.])+\@([A-Za-zА-Яа-я0-9_\-\.])+\.([A-Za-zА-Яа-я]{2,6})$/,
				tel: /^\+?\d{1}[-\(]?\d{3}[-\)]?\d{3}\-?\d{2}\-?\d{2}$/
			}

			invalidMessage.style.display = 'none'
			this.isValid[type] = 0
			if (input.value.length) {
				if (!types[type].test(this[type])) {
					invalidMessage.style.display = 'block'
				} else {
					this.isValid[type] = 1
				}
			}
		}
	}
}
</script>

<style lang="scss">
$color-light: #ebf4f8;
$color-primary: #0880ae;
$color-primary-light: #7c9cbf;
$color-primary-lighter: #dbe2ea;
$color-secondary: #756f86;
$color-secondary-light: #b1b5bf;
$color-danger: #ff7171;
$color-black: #2c2738;

.registration {
	font-family: 'IBM Plex Sans', sans-serif;
	font-size: 16px;
	line-height: 1.375;
	background-color: white;
	padding: 40px 30px;
	border-radius: 24px;
	box-shadow: 0px 12px 24px rgba($color-black, 0.02),
		0px 32px 64px rgba($color-black, 0.04);
	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	& > div {
		margin-bottom: 34px;
	}

	&--title {
		font-size: 34px;
		color: $color-black;
		margin-bottom: 8px;
	}

	&--subtitle {
		margin-bottom: 56px;
	}

	a {
		text-decoration: none;
		color: $color-primary;
	}

	label {
		display: block;
		color: $color-secondary;
		font-weight: 500;
		margin-bottom: 6px;
	}

	input {
		appearance: none;
		font-family: inherit;
		width: 100%;
		height: 50px;
		border: 1px solid $color-primary-lighter;
		border-radius: 6px;
		box-shadow: 0px 4px 8px rgba($color-black, 0.04);
		padding: 16px 0 16px 16px;
		&:focus {
			border: 1px solid $color-primary;
			outline: none;
		}
		&::placeholder {
			color: $color-primary-light;
		}
		&[type='checkbox'] {
			display: inline-block;
			position: relative;
			min-width: 28px;
			max-width: 28px;
			height: 28px;
			padding: 0;
			margin: 0;
			&:focus {
				border: 1px solid $color-primary-lighter;
			}
			&:checked {
				border: 1px solid $color-primary;
				border-radius: 6px;
				&::after {
					content: '';
					position: absolute;
					width: 28px;
					height: 28px;
					background: url('i-checked.svg') no-repeat center;
					top: -1px;
					left: -1px;
				}
			}
		}
		&[type='submit'] {
			background-color: $color-primary;
			color: $color-light;
			box-shadow: 0px 4px 8px rgba($color-black, 0.08);
			margin-bottom: 0;
			padding: 0;
			&:active {
				border: 2px solid rgba($color-black, 0.86);
			}
			&:disabled {
				background-color: $color-primary-lighter;
				color: $color-secondary-light;
			}
		}
		&:-webkit-autofill,
		&:-webkit-autofill:hover,
		&:-webkit-autofill:focus,
		&:-webkit-autofill:active,
		&:-webkit-autofill::first-line {
			box-shadow: inset 0 0 0 50px #fff;
			font-family: 'IBM Plex Sans', sans-serif;
		}
	}

	&--input-invalid {
		display: none;
		color: $color-danger;
		font-size: 14px;
	}

	&--select {
		@extend input;
		&.open {
			border: 2px solid $color-primary;
			&::after {
				top: 10px;
				right: 10px;
				transform: rotate(180deg);
			}
			.placeholder {
				color: $color-black;
			}
			.options {
				display: block;
			}
		}
		.placeholder {
			color: $color-primary-light;
		}
		.options {
			display: none;
			position: absolute;
			left: -2px;
			right: -2px;
			top: 50px;
			background-color: white;
			z-index: 1;
			border: 1px solid $color-primary-lighter;
			border-radius: 6px;
			box-shadow: 0px 4px 8px rgba($color-black, 0.04),
				0px 20px 20px rgba($color-black, 0.04);
			list-style: none;
			.option {
				height: 44px;
				display: flex;
				align-items: center;
				color: $color-secondary;
				cursor: pointer;
				padding-left: 16px;
				&:hover {
					background-color: $color-light;
				}
			}
		}
	}

	&--confirm {
		display: flex;
		align-items: center;
		margin-bottom: 38px;
		p {
			margin-bottom: 0;
			margin-left: 8px;
			white-space: nowrap;
		}
	}
}
</style>
