<template>
	<div style="width: 100%;height: 100%;">
		<div v-if="!show">
			<svg class="loading_ico" width="1em" height="1em" fill="currentColor" aria-hidden="true" viewBox="0 0 1024 1024" focusable="false"><path d="M988 548c-19.9 0-36-16.1-36-36 0-59.4-11.6-117-34.6-171.3a440.45 440.45 0 00-94.3-139.9 437.71 437.71 0 00-139.9-94.3C629 83.6 571.4 72 512 72c-19.9 0-36-16.1-36-36s16.1-36 36-36c69.1 0 136.2 13.5 199.3 40.3C772.3 66 827 103 874 150c47 47 83.9 101.8 109.7 162.7 26.7 63.1 40.2 130.2 40.2 199.3.1 19.9-16 36-35.9 36z"></path></svg>
		</div>
		<div class="main screen-full" :style="{backgroundColor: isFullscreen?'black':'rgba(0, 0, 0, 0.5)'}" @click.stop="close" @mousewheel.prevent="rollImg" v-show="show">
			<img @load="img_load" @mousedown="move" @dblclick="handleFullScreen" class="image" @click.stop="img_click" :src="src"
				ondragstart="return false;"
				:style="{transform: 'rotate('+img_style.rotate+'deg) translate(-50%, -50%)',width: img_style.width==-1?'auto':img_style.width+'px', height:img_style.height==-1?'auto':img_style.height+'px', left: img_style.left+'px', top: img_style.top+'px'}">
			<ul @click.stop="img_click">
				<li @click="handleFullScreen">
					<svg v-if="!isFullscreen" title="打开全屏" class="ico_class" data-icon="expand" width="1em" height="1em" fill="currentColor" aria-hidden="true" viewBox="64 64 896 896" focusable="false"><path d="M342 88H120c-17.7 0-32 14.3-32 32v224c0 8.8 7.2 16 16 16h48c8.8 0 16-7.2 16-16V168h174c8.8 0 16-7.2 16-16v-48c0-8.8-7.2-16-16-16zm578 576h-48c-8.8 0-16 7.2-16 16v176H682c-8.8 0-16 7.2-16 16v48c0 8.8 7.2 16 16 16h222c17.7 0 32-14.3 32-32V680c0-8.8-7.2-16-16-16zM342 856H168V680c0-8.8-7.2-16-16-16h-48c-8.8 0-16 7.2-16 16v224c0 17.7 14.3 32 32 32h222c8.8 0 16-7.2 16-16v-48c0-8.8-7.2-16-16-16zM904 88H682c-8.8 0-16 7.2-16 16v48c0 8.8 7.2 16 16 16h174v176c0 8.8 7.2 16 16 16h48c8.8 0 16-7.2 16-16V120c0-17.7-14.3-32-32-32z"></path></svg>
					<svg v-else title="退出全屏" class="ico_class" data-icon="compress" width="1em" height="1em" fill="currentColor" aria-hidden="true" viewBox="64 64 896 896" focusable="false"><path d="M326 664H104c-8.8 0-16 7.2-16 16v48c0 8.8 7.2 16 16 16h174v176c0 8.8 7.2 16 16 16h48c8.8 0 16-7.2 16-16V696c0-17.7-14.3-32-32-32zm16-576h-48c-8.8 0-16 7.2-16 16v176H104c-8.8 0-16 7.2-16 16v48c0 8.8 7.2 16 16 16h222c17.7 0 32-14.3 32-32V104c0-8.8-7.2-16-16-16zm578 576H698c-17.7 0-32 14.3-32 32v224c0 8.8 7.2 16 16 16h48c8.8 0 16-7.2 16-16V744h174c8.8 0 16-7.2 16-16v-48c0-8.8-7.2-16-16-16zm0-384H746V104c0-8.8-7.2-16-16-16h-48c-8.8 0-16 7.2-16 16v224c0 17.7 14.3 32 32 32h222c8.8 0 16-7.2 16-16v-48c0-8.8-7.2-16-16-16z"></path></svg>
				</li>
				<li @click="img_chushi" title="复位图片">
					<svg class="ico_class" data-icon="sync" width="1em" height="1em" fill="currentColor" aria-hidden="true" viewBox="64 64 896 896" focusable="false"><path d="M168 504.2c1-43.7 10-86.1 26.9-126 17.3-41 42.1-77.7 73.7-109.4S337 212.3 378 195c42.4-17.9 87.4-27 133.9-27s91.5 9.1 133.8 27A341.5 341.5 0 01755 268.8c9.9 9.9 19.2 20.4 27.8 31.4l-60.2 47a8 8 0 003 14.1l175.7 43c5 1.2 9.9-2.6 9.9-7.7l.8-180.9c0-6.7-7.7-10.5-12.9-6.3l-56.4 44.1C765.8 155.1 646.2 92 511.8 92 282.7 92 96.3 275.6 92 503.8a8 8 0 008 8.2h60c4.4 0 7.9-3.5 8-7.8zm756 7.8h-60c-4.4 0-7.9 3.5-8 7.8-1 43.7-10 86.1-26.9 126-17.3 41-42.1 77.8-73.7 109.4A342.45 342.45 0 01512.1 856a342.24 342.24 0 01-243.2-100.8c-9.9-9.9-19.2-20.4-27.8-31.4l60.2-47a8 8 0 00-3-14.1l-175.7-43c-5-1.2-9.9 2.6-9.9 7.7l-.7 181c0 6.7 7.7 10.5 12.9 6.3l56.4-44.1C258.2 868.9 377.8 932 512.2 932c229.2 0 415.5-183.7 419.8-411.8a8 8 0 00-8-8.2z"></path></svg>
				</li>
				<li @click="whirl(false)" title="向左旋转">
					<svg class="ico_class" data-icon="rotate-left" width="1em" height="1em" fill="currentColor" aria-hidden="true" viewBox="64 64 896 896" focusable="false"><path d="M672 418H144c-17.7 0-32 14.3-32 32v414c0 17.7 14.3 32 32 32h528c17.7 0 32-14.3 32-32V450c0-17.7-14.3-32-32-32zm-44 402H188V494h440v326z"></path><path d="M819.3 328.5c-78.8-100.7-196-153.6-314.6-154.2l-.2-64c0-6.5-7.6-10.1-12.6-6.1l-128 101c-4 3.1-3.9 9.1 0 12.3L492 318.6c5.1 4 12.7.4 12.6-6.1v-63.9c12.9.1 25.9.9 38.8 2.5 42.1 5.2 82.1 18.2 119 38.7 38.1 21.2 71.2 49.7 98.4 84.3 27.1 34.7 46.7 73.7 58.1 115.8a325.95 325.95 0 016.5 140.9h74.9c14.8-103.6-11.3-213-81-302.3z"></path></svg>
				</li>
				<li @click="whirl(true)" title="向右旋转">
					<svg class="ico_class" data-icon="rotate-right" width="1em" height="1em" fill="currentColor" aria-hidden="true" viewBox="64 64 896 896" focusable="false"><path d="M480.5 251.2c13-1.6 25.9-2.4 38.8-2.5v63.9c0 6.5 7.5 10.1 12.6 6.1L660 217.6c4-3.2 4-9.2 0-12.3l-128-101c-5.1-4-12.6-.4-12.6 6.1l-.2 64c-118.6.5-235.8 53.4-314.6 154.2A399.75 399.75 0 00123.5 631h74.9c-.9-5.3-1.7-10.7-2.4-16.1-5.1-42.1-2.1-84.1 8.9-124.8 11.4-42.2 31-81.1 58.1-115.8 27.2-34.7 60.3-63.2 98.4-84.3 37-20.6 76.9-33.6 119.1-38.8z"></path><path d="M880 418H352c-17.7 0-32 14.3-32 32v414c0 17.7 14.3 32 32 32h528c17.7 0 32-14.3 32-32V450c0-17.7-14.3-32-32-32zm-44 402H396V494h440v326z"></path></svg>
				</li>
				<li @click.stop="close" title="退出预览">
					<svg class="ico_class" data-icon="export" width="1em" height="1em" fill="currentColor" aria-hidden="true" viewBox="64 64 896 896" focusable="false"><path d="M888.3 757.4h-53.8c-4.2 0-7.7 3.5-7.7 7.7v61.8H197.1V197.1h629.8v61.8c0 4.2 3.5 7.7 7.7 7.7h53.8c4.2 0 7.7-3.4 7.7-7.7V158.7c0-17-13.7-30.7-30.7-30.7H158.7c-17 0-30.7 13.7-30.7 30.7v706.6c0 17 13.7 30.7 30.7 30.7h706.6c17 0 30.7-13.7 30.7-30.7V765.1c0-4.3-3.5-7.7-7.7-7.7zm18.6-251.7L765 393.7c-5.3-4.2-13-.4-13 6.3v76H438c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h314v76c0 6.7 7.8 10.5 13 6.3l141.9-112a8 8 0 000-12.6z"></path></svg>
				</li>
			</ul>
		</div>
	</div>
</template>

<script>
	import screenfull from "screenfull"; //引入依赖
	import {
		ref,
		h
	} from 'vue';
	export default ({
		props: ["src"],
		data: function() {
			return {
				img_style: {
					width: 150,
					height: -1,
					left: 0,
					top: 0,
					rotate:0
				},
				window_inner: {
					width: 0,
					height: 0
				},
				isFullscreen: false, //是否全屏
				show: false
			}
		},
		methods: {
			whirl:function(clockwise){
				if(clockwise){
					this.img_style.rotate+=90
				}else{
					this.img_style.rotate-=90
				}
				if(this.img_style.rotate==360||this.img_style.rotate==-360){
					var img = document.getElementsByClassName('image')[0]
					img.style.transition='none 0s ease 0s'
					this.img_style.rotate=0
					img.style.transition='transform .1s linear'
				}
			},
			handleFullScreen() {
				setTimeout(() => {
					this.img_chushi()
				}, 50)
				if (!screenfull.isEnabled) {
					alert("您的浏览器版本过低，不支持全屏浏览");
					return false;
				}
				screenfull.toggle();
			},
			change() {
				this.isFullscreen = screenfull.isFullscreen;
			},
			init() {
				if (screenfull.isEnabled) {
					screenfull.on("change", this.change);
				}
			},
			destroy() {
				if (screenfull.isEnabled) {
					screenfull.off("change", this.change);
				}
			},
			close: function() {
				this.show = false
				if (this.isFullscreen)
					this.handleFullScreen()
				setTimeout(() => {
					this.$emit('close')
				}, 10)

			},
			img_click: function() {

			},
			move: function(e) {
				var x = e.pageX - this.img_style.left
				var y = e.pageY - this.img_style.top
				var that = this

				function move(e) {
					that.img_style.left = e.pageX - x
					that.img_style.top = e.pageY - y
					// console.log(e.pageX - x,e.pageY - y);
				}
				var box = document.getElementsByClassName('main')[0]
				box.addEventListener('mousemove', move)
				box.addEventListener('mouseup', function() {
					box.removeEventListener('mousemove', move)
				})
			},
			rollImg: function(event) {
				 var e = event || window.event;
				var img = document.getElementsByClassName('image')[0]
				var ppp = img.width / 1000
				var w = img.width + (e.wheelDelta * ppp)
				if (w <= 150 || w >= 5000) {
					return
				}
				this.img_style.width = w
				this.img_style.height = -1
				// console.log(this.img_style.width);
				return false;
			},
			img_chushi: function() {
				var img = document.getElementsByClassName('image')[0]
				var img_b = img.width - img.height
				if (img_b < 0) {
					if (this.window_inner.height > this.window_inner.width) {
						this.img_style.height = -1
						this.img_style.width = this.window_inner.width - (this.window_inner.width * 0.1)
					} else {
						this.img_style.height = this.window_inner.height - (this.window_inner.height * 0.1)
						this.img_style.width = -1
					}

				} else {
					if (this.window_inner.width > this.window_inner.height) {
						this.img_style.height = this.window_inner.height - (this.window_inner.height * 0.1)
						this.img_style.width = -1
					} else {
						this.img_style.height = -1
						this.img_style.width = this.window_inner.width - (this.window_inner.width * 0.1)
					}

				}
				this.img_style.left = this.window_inner.width / 2
				this.img_style.top = (this.window_inner.height - 40) / 2
				this.img_style.rotate=0
			},
			img_load: function(){
				this.show=true
			}
		},
		beforeDestroy() {
			this.destroy();
		},
		mounted() {
			this.window_inner.height = window.innerHeight
			this.window_inner.width = window.innerWidth
			window.addEventListener('resize', () => {
				this.window_inner.height = window.innerHeight
				this.window_inner.width = window.innerWidth
			})
			this.img_chushi()
			this.init();
			window.addEventListener('keyup', (e) => {
				//此处填写你的业务逻辑即可
				if (e.keyCode == 27) {
					this.close()
				}
			})
		},
		components:{
		}
	})
</script>

<style scoped>
	@import url("./animation.css");
	.main {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		
		overflow: none;
		user-select: none;
		-webkit-user-drag: none;
		transition: all .2s linear;
	}

	.image {
		cursor: grab;
		position: absolute;
		/* transform: translate(-50%, -50%); */
		transition: transform .1s linear;
		transform-origin: left top;
	}

	.image:active {
		cursor: grabbing;
	}

	ul {
		position: fixed;
		bottom: 0;
		left: 50%;
		transform: translateX(-50%);
		list-style-type: none;
		background-color: rgba(255, 255, 255, 0.5);
		padding: 0;
		/* width: 100%; */
		transition: all .2s linear;
		border-radius: 5px;
		box-shadow: 0 0 13px 0px #0000004f;
	}

	li {
		float: left;
		padding: 5px 10px;
		margin: 5px;
		box-shadow: 0 0 13px 0px #0000004f;
		background-color: #ffffffa6;
		cursor: pointer;
		border-radius: 5px;
		transition: all .15s linear;
	}

	li:nth-child(1) {
		margin: 5px 5px 5px 10px;
	}

	li:nth-last-child(1) {
		margin: 5px 10px 5px 5px;
	}

	li:hover {
		background-color: #0079FE;
		color: white;
	}
	.ico_class{
		width: 1.1rem;
		height: 1.1rem;
		transform: translateY(3px);
	}
	li:active .ico_class{
		color: wheat;
	}
	
	.loading_ico{
		color: white;
		width: 4rem;
		height: 4rem;
		transform: rotate(0deg);
		animation: waiting .8s linear infinite;
	}
</style>
