<template>
	<div id="app">
		<sign-up></sign-up>
	</div>
</template>

<script>
import SignUp from '@/components/SignUp.vue'
export default {
	components: {
		'sign-up': SignUp
	}
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@400;500;700&display=swap');
#app {
	width: 70%;
	min-width: 360px;
	margin: 20px auto;
}
</style>
