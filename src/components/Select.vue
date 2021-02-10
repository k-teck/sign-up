<template>
	<div class="select" @click="selectHandler">
		<span class="placeholder" v-if="!selectedOption || isOpen">Язык</span>
		<span class="selected-value" v-if="selectedOption && !isOpen">
			{{ selectedOption }}
		</span>
		<ul class="options" v-show="isOpen">
			<li
				v-for="item in items"
				class="option"
				:key="item.id"
				:value="item"
				:selected="selectedOption === item"
				@click="
					selectedOption = item
					$emit('select', item)
				"
			>
				{{ item }}
			</li>
		</ul>
	</div>
</template>

<script>
export default {
	props: {
		options: {
			type: Array,
			required: true
		}
	},
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
			const open = function() {
				self.isOpen = true
				select.classList.add('open')
				document.addEventListener('click', notSelectClickListener)
			}
			const close = function() {
				self.isOpen = false
				select.classList.remove('open')
				document.removeEventListener('click', notSelectClickListener)
			}
			const toggle = function() {
				self.isOpen ? close() : open()
			}
			const notSelectClickListener = function(e) {
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
	display: flex;
	align-items: center;
	&::after {
		content: '';
		position: absolute;
		width: 30px;
		height: 30px;
		background: url('../assets/img/i-arrow.svg') no-repeat center;
		top: 11px;
		right: 11px;
		transition: transform 0.35s ease;
	}
	.options {
		list-style: none;
		position: absolute;
		bottom: 0;
		transform: translateY(100%);
		z-index: 1;
	}
}
</style>
