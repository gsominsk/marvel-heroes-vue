<template>
	<li class="hl-item">
		<div class="row">
			<div class="hidden-xs-down col-sm-1 col-md-1 hero-row-num">
				<span>{{idx + 1}}</span>
			</div>
			<div class="col-xs-8 col-sm-8 col-md-8 hero-img" v-bind:style="{ 'background-image': 'url(' + item.img + ')' }">
				<div class="hero-name"><span>{{item.name}}</span></div>
			</div>
			<div class="col-xs-8 col-sm-3 col-md-3 hero-film" v-bind:style="{ 'background-image': 'url(' + item.poster + ')' }">>

			</div>
		</div>
	</li>
</template>

<script>
	export default {
		props: ['item', 'idx']

	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	@import '../assets/css/style.css';
</style>
