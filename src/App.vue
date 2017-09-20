<template>
  	<div id="app">
		<div class="menu" v-on:click="openMenu">
			<form name="newHeroAdd" v-on:submit.prevent="addNewHero" class="add-new-hero-form clearfix">
				<div class="error-nha">{{error}}</div>
				<input type="text" name="heroName" v-model="name" placeholder="hero name" required>
				<span>Hero image (png) : </span>
				<input type="file" name="heroImg" required>
				<span>Hero film poster : </span>
				<input type="file" name="heroFilm" required>
				<button>save</button>
			</form>
		</div>
    	<router-view></router-view>
  	</div>
</template>

<script>

var async = require('async');

export default {
  	name: 'app',
	data ( ) {
  		return ({
			name: '',
			error: '',
			hero: {}
		})
	},
	methods : {
  		openMenu: function (force) {
  			var m = document.getElementsByClassName('menu')[0];
			if (force != true && event.target.classList.value != 'menu' && m.hasAttribute('style')) return ;
  			m[m.hasAttribute('style')?'removeAttribute':'setAttribute']('style', 'left:0');
		},
		addNewHero: function () {
			var img 	= document.querySelectorAll('input[name=heroImg]')[0];
			var poster 	= document.querySelectorAll('input[name=heroFilm]')[0];
			var hero 	= {
				name: this.name,
				img: '',
				poster: '',
			};

			async.parallel([
				(callback) => {
					var file = img.files[0];
					this.addFiles(file, (err, res) => {
						callback(err?err:null, {img:res, action: 'img'})
					});
				},
				(callback) => {
					var file = poster.files[0];
					this.addFiles(file, (err, res) => {
						callback(err?err:null, {img:res, action: 'poster'})
					});
				}
			], (err, res) => {
				if (res[0] == null || res[1] == null) {
					this.error = 'Incorrect type of file!';
				} else {
					this.error = '';
					res.forEach((data) => {
						hero[data.action] = data.img;
					});
					this.setData(hero, (err) => {
						if (err) return ;
						this.$children[0]._data.heroes.unshift(hero);
						this.openMenu(true);
					});
				}
			})
		},
		addFiles: function (file, callback) {
			var reader          = new FileReader();

			if (!file.type.match(/.(jpg|jpeg|png|gif|bmp)$/i)) {
				callback(null);
			} else if (file) {
				reader.readAsDataURL(file); //reads the data as a URL
				reader.onloadend = () => {
					callback(null, reader.result);
				}
			} else {console.log('error');}
		},
		setData: function (hero, callback) {
  			var heroes = localStorage.heroes ? JSON.parse(localStorage.heroes) : [];
			heroes.push(hero);
			try {
				localStorage.heroes = JSON.stringify(heroes);
			} catch(e) {
				this.error = 'File too big!';
				return callback('err')
			}
  			callback();
		}
	}
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
