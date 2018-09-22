<template>
	<div id="search">
		<input type="text" v-model="search" placeholder="search">
	</div>
</template>
<script>
	import axios from 'axios'
	export default {
		data() {
			return {
				search: '',
				options: {
					params: {
						client_id: '9031921175042d5a0c24e5fa71974ff708aa898ad5850cd5cbb9b900714fec26',
						query: null,
						page: 1,
						per_page: 24 
					}
				},
				timer: null,
				url: 'https://api.unsplash.com/search/photos'
			}
		},
		watch: {
			search: {
				handler: 'searchData'
			}
		},
		methods: {
			searchData(val) {
				clearTimeout(this.timer)
				this.timer = setTimeout(() => {
					if (val.length >= 3) {
						this.options.params.query = val
						axios.get(this.url , this.options).then(response => {
							this.$emit('searchResult', response, this.options.params.query)
						})
					} else {
						this.$emit('searchResult', false)
					}
				}, 800)	
			}
		}
	}	
</script>
	
<style lang="scss">
	#search {
		height: 5vh;
		display: flex;
		justify-content: center;
		position: relative;
		background-color: rgba(0,0,0, 0);
		input {
			width: 60%;
			font-family: 'Roboto';
			font-size: .8em;
			font-weight: ligther !important;
			border: 0px; 
		}
		input:focus {
 		   outline:none;
		}
		input[placeholder] {
			opacity: .5;
			font-weight: lighter;
		}
	}
</style>