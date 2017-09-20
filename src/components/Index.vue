<template>
	<div class="main-wrap">
		<div class="search">
			<input type="text" placeholder="Find your hero" v-model="heroName">
		</div>
		<div class="add-new-hero-btn" v-on:click="openMenu()">
			<i class="fa fa-plus-circle" aria-hidden="true"></i>
		</div>
	  	<ul class="heroes-list">
			<hero-item v-for="(hero, index) in heroes" v-bind:item="hero" v-bind:idx="index" v-bind:key="index"></hero-item>
	  	</ul>
  	</div>
</template>

<script>

export default {
  	name: 'index',
	props: ['item'],
  	data () {
    	return {
    		heroes: [],
			heroName: ''
    	}
  	},
	methods: {
  		openMenu () {
			this.$parent.$options.methods.openMenu();
		},
		getData: function () {
			this.heroes = localStorage.heroes ? JSON.parse(localStorage.heroes) : [];
		}
	},
	created () {
  		this.getData();

  		setTimeout(() => {
			document.body.setAttribute('style', 'opacity: 1');
		}, 5000)
	},
	watch : {
		heroName: function () {
			if (this.heroName.length == 0) {
				this.getData();
				return ;
			}
			var heroes = localStorage.heroes ? JSON.parse(localStorage.heroes) : [];
			this.heroes = [];

			for (var i = 0; i < heroes.length; i++) {
				if (heroes[i].name.indexOf(this.heroName) != -1)
					this.heroes.unshift(heroes[i]);
			}
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	@import '../assets/css/style.css';
</style>
