<template>
	<div class="div">
		<div v-if="dataPage" @click.self="hidePage" id="picture-page">
			<div @mouseleave.self="mouseLeave" class="page-contain">
				<div class="page-title">
					<p>{{dataPage.user.name}}</p>
					<img @mouseover="showUserInf" sytle="margin: 3px;" :src="dataPage.user.profile_image.small" alt="">
				</div>	
				<img id="photo" :src="dataPage.urls.regular" alt="">
			</div>
		</div>	
	</div>
</template>

<script>
	export default {
		props: ['dataPage'],
		data() {
			return {
				showName: false
			}
		},
		methods: {
			hidePage() {
				this.$emit('hidePage')
			},
			mouseLeave(e) {
				e.target.classList.add('mouse-leave')
			},
			showUserInf() {
				this.showName = true
			}
		}
	}	
</script>

<style lang="scss">
	@keyframes show-inf {
		from {
			opacity: 0;
			right:  -100px;
		}
		to {
			opacity: 1;
		}
	}
	#picture-page {
		font-family: 'Roboto';
		font-weight: lighter;
		position: absolute;
		color: rgba(#000, .7);
		width: 100%;
		top: 0;
		z-index: 9999;
		height: 100vh;
		display: flex;
		justify-content: center;
		align-items: center;
		overflow-y: scroll;
		background-color: rgba(#000, .88);
		.page-contain {
			background-color: #fff;
			width: 80%;
			height: 100vh;
			.page-title {
				display: flex;
				align-items: center;
				justify-content: flex-end;
				p {
					font-size: .75em;
					z-index: 0;
					position: relative;
					color: rgba(#000, .7);
					right: 10px;
					animation: 1s show-inf;
					display: block;
				}
				img {
					margin: .9px;
					cursor: pointer;
				}
			}
			#photo {
				width: 100%;
			}
		}
	}	
	.mouse-leave {
		opacity: .4;
		transition: all 1s;
		&:hover {
			opacity: 1;
		}
	}
</style>