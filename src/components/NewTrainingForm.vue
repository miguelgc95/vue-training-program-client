<template>
	<base-card>
		<h2>Registrar nuevo entrenamiento</h2>
		<form @submit.prevent="submitForm">
			<div class="form-control" :class="{ invalid: !date.isValid }">
				<label for="date">Fecha</label>
				<input type="date" id="date" v-model.trim="date.val" @blur="clearValidity('date')" />
				<p v-if="!date.isValid">date must not be empty.</p>
			</div>
			<div class="form-control" :class="{ invalid: !description.isValid }">
				<label for="description">Descripción - Estado de ánimo/energía - Consideraciones generales</label>
				<textarea id="description" rows="5" v-model.trim="description.val" @blur="clearValidity('description')"></textarea>
				<p v-if="!description.isValid">Description must not be empty.</p>
			</div>

			<div class="list-choice">
				<div class="list-choice-title">Añadir bloque de entrenamiento</div>
				<div class="list-choice-objects">
					<label> <input type="radio" name="month" /> <span>Tren superior</span> </label>
					<label> <input type="radio" name="month" /> <span>Tren inferior</span> </label>
					<label> <input type="radio" name="month" /> <span>Handstand</span> </label>
					<label> <input type="radio" name="month" /> <span>Movilidad isquisural</span> </label>
					<label> <input type="radio" name="month" /> <span>Movilidad de cadera</span> </label>
					<label> <input type="radio" name="month" /> <span>Movilidad torácica</span> </label>
				</div>
			</div>

			<div class="form-control">
				<label for="select">block</label>
				<select id="select" v-model="selected">
					<option value="Tren superior">Tren superior</option>
					<option value="Tren inferior">Tren inferior</option>
					<option value="Handstand">Handstand</option>
					<option value="Movilidad isquisural">Movilidad isquisural</option>
					<option value="Movilidad de cadera">Movilidad de cadera</option>
					<option value="Movilidad torácica">Movilidad torácica</option>
				</select>
			</div>
			<p v-if="!formIsValid">Please fix the above errors and submit again.</p>
			<base-button mode="outline" @click="addBlock">Añadir bloque de entrenamiento</base-button>
			<base-button @click="submitForm">Register</base-button>
		</form>
	</base-card>
</template>

<script>
export default {
	emits: ['save-data'],
	data() {
		return {
			selected: 'Tren superior',
			date: {
				val: new Date().toISOString().substr(0, 10),
				isValid: true,
			},
			description: {
				val: '',
				isValid: true,
			},
			training: {
				val: [],
				isValid: true,
			},
			formIsValid: true,
		}
	},
	methods: {
		clearValidity(input) {
			this[input].isValid = true
		},
		validateForm() {
			this.formIsValid = true
			if (this.date.val === '') {
				this.date.isValid = false
				this.formIsValid = false
			}
			if (this.lastName.val === '') {
				this.lastName.isValid = false
				this.formIsValid = false
			}
			if (this.description.val === '') {
				this.description.isValid = false
				this.formIsValid = false
			}
		},
		addBlock() {
			console.log('addblock')
		},
		submitForm() {
			this.validateForm()

			if (!this.formIsValid) {
				return
			}

			const formData = {
				first: this.firstName.val,
				last: this.lastName.val,
				desc: this.description.val,
				rate: this.rate.val,
				areas: this.areas.val,
			}

			this.$emit('save-data', formData)
		},
	},
}
</script>

<style scoped>
.form-control {
	margin: 0.5rem 0;
}

label {
	font-weight: bold;
	display: block;
	margin-bottom: 0.5rem;
}

input[type='checkbox'] + label {
	font-weight: normal;
	display: inline;
	margin: 0 0 0 0.5rem;
}

input,
textarea {
	display: block;
	width: 100%;
	border: 1px solid #ccc;
	font: inherit;
}

input:focus,
textarea:focus {
	background-color: #f0e6fd;
	outline: none;
	border-color: #3d008d;
}

h3 {
	margin: 0.5rem 0;
	font-size: 1rem;
}

.invalid label {
	color: red;
}

.invalid input,
.invalid textarea {
	border: 1px solid red;
}

/* styles for the select input, atked from https://www.sliderrevolution.com/resources/css-select-styles/ */
@import url(https://fonts.googleapis.com/css?family=Raleway);
body {
	font-family: 'Raleway', sans-serif;
	background: #3e4651;
}
.list-choice {
	width: 300px;
	margin: 1em auto;
	position: relative;
	cursor: pointer;
}
.list-choice input[type='radio'] {
	position: absolute;
	left: -9999px;
}

.list-choice-title {
	width: 100%;
	display: block;
	background: #d8d8d8;
	text-align: center;
	padding: 0.55em 1em;
	box-sizing: border-box;
	color: #fff;
	text-shadow: 0 1px 0 #cacaca;
	border-radius: 0.2em;
}
.list-choice:hover .list-choice-title {
	border-radius: 0.2em 0.2em 0 0;
}
.list-choice-objects label:nth-last-of-type(1) span {
	border-radius: 0 0 0.2em 0.2em;
}
.list-choice input[type='radio'] + span {
	color: #fff;
	background: #d8d8d8;
	padding: 0.55em 1em;
	display: block;
	text-align: center;
	box-sizing: border-box;
	cursor: pointer;
	width: 100%;
}
.list-choice-objects {
	position: absolute;
	top: 0;
	padding-top: 2.1em;
	box-sizing: border-box;
	width: 100%;
	overflow: hidden;
	max-height: 0;
	transition: all 250ms ease;
}
.list-choice:hover .list-choice-objects input[type='radio'] + span {
	position: relative;
	top: 0;
	transition: all 250ms ease-in-out;
}
.list-choice:hover input[type='radio'] + span:hover {
	background: #bbb;
}
.list-choice:hover input[type='radio']:checked + span:hover {
	background: #74d68e;
}
.list-choice input[type='radio']:checked + span {
	background: #74d68e;
	position: absolute;
	top: 0em;
	border-radius: 0.2em;
}
.list-choice:hover input[type='radio']:checked + span {
	border-radius: 0;
}
.list-choice:hover .list-choice-objects label:nth-last-of-type(1) input[type='radio']:checked + span {
	border-radius: 0 0 0.2em 0.2em;
}

.list-choice:hover .list-choice-objects {
	max-height: 540px;
}
</style>
