<template>
	<div>
	</div>
</template>

<script>
export default {
	loading: false,
	async asyncData({$axios, isDev, route, store, env, params, query, req, res, redirect, error}) {
		return {
			t: query.t
		}
	},
	mounted() {
		if (this.$store.state.login.isLogin && localStorage['teamNum']) {
			let n = localStorage['teamNum'];
			let params = `TemplateNumber=${this.t}&TeamNum=${n}` ;
			this.$axios.post('/CopyTemplate', params)
			.then(({data}) => {
				let str = '/designer/' +  window.btoa(`DocumentNumber=${data}`)
				console.log(str)
				this.$router.replace(str)
				// window.location.replace(str)
			})
		} else {
			this.$store.commit('login/toggleShow', true)
		}
	}
}
</script>

<style>

</style>
