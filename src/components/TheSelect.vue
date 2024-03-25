<template>
	<div class="v-select">
		<p
			v-if="!this.selectedRole"
			type="button"
			@click="handleChooseOption"
			class="title"
		>
			Должность
		</p>
		<img class="arrow" src="../assets/carret.svg" alt="arrow" />
		<p class="title" @click="handleChooseOption">{{ selectedRole }}</p>
		<div v-if="isVisible" class="options">
			<p
				@click="selectOptions(option.value)"
				v-for="option in options"
				:key="option.value"
			>
				{{ option.name }}
			</p>
		</div>
	</div>
</template>

<script>
export default {
	props: {
		options: {
			type: Array,
			required: true,
		},
	},
	data() {
		return {
			isVisible: false,
			selectedRole: "",
		}
	},
	methods: {
		handleChooseOption() {
			this.isVisible = !this.isVisible
		},
		selectOptions(option) {
			this.selectedRole = option
			this.$emit("select", this.selectedRole)
			this.isVisible = !this.isVisible
		},
	},
}
</script>

<style scoped>
.v-select {
	position: relative;
	width: 45rem;
	height: 3.9rem;
	border-radius: 1.1rem;
	border: 1px solid #e6e6eb;
	padding-left: 1rem;
	cursor: pointer;
}
.title {
	margin-top: 1rem;
	font-size: 14px;
	font-weight: 400;
	color: #9292a0;
}
.options {
	display: flex;
	flex-direction: column;
	gap: 0.5rem;
	border: 1px solid #e6e6eb;
	border-top: none;
	position: absolute;
	top: 3.5rem;
	right: 0;
	width: 100%;
	z-index: 1;
	font-size: 14px;
	font-weight: 400;
	background-color: #fff;
	color: #9292a0;
	padding-left: 1rem;
}
.options p:hover {
	background-color: rgba(73, 121, 218, 0.2);
}

.arrow {
	position: absolute;
	top: 1.6rem;
	right: 1.5rem;
}
</style>
