<template>
	<div class="select" @click="selectHandler">
		<span class="placeholder" v-if="!selectedOption || isOpen">Язык</span>
		<span class="selectedValue" v-if="selectedOption && !isOpen">{{
			selectedOption
		}}</span>
		<ul class="options">
			<li
				class="option"
				v-for="item in items"
				:key="item.id"
				@click="
					selectedOption = item
					$emit('select', item)
				"
				:value="item"
				:selected="selectedOption === item"
			>
				{{ item }}
			</li>
		</ul>
	</div>
</template>

<script>
export default {
	props: ['options'],
	data() {
		return {
			isOpen: false,
			selectedOption: null
		}
	},
	computed: {
		items() {
			return this.options
		}
	},
	methods: {
		selectHandler() {
			const self = this
			const select = self.$el
			const options = select.querySelector('.options')
			const open = function () {
				select.classList.add('open')
				document.addEventListener('click', notSelectClickListener)
			}
			const close = function () {
				select.classList.remove('open')
				document.removeEventListener('click', notSelectClickListener)
			}
			const toggle = function () {
				select.classList.contains('open') ? close() : open()
				self.isOpen = !self.isOpen
			}
			const notSelectClickListener = function (e) {
				if (!select.contains(e.target)) {
					close()
				}
			}
			toggle()
		}
	}
}
</script>

<style lang="scss">
.select {
	width: 100%;
	line-height: 1;
	position: relative;
	&::after {
		content: '';
		position: absolute;
		width: 30px;
		height: 30px;
		background: url('i-arrow.svg') no-repeat center;
		top: 11px;
		right: 11px;
		transition: transform 0.35s ease;
	}
}
</style>
