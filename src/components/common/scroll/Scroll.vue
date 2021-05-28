<template>
	<div class="wrapper" ref="wrapper">
		<div class="content">
			<slot></slot>
		</div>
	</div>
</template>

<script>
	import BScroll from "better-scroll";
	export default {
		name: "Scroll",
		props: {
			probeType: {
				type: Number,
				default: 3
			},
			pullUpLoad: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return {
				scroll: null
			}
		},
		mounted() {
			this.scroll = new BScroll(this.$refs.wrapper, {
				probeType: this.probeType, //3 监听手指滑动和惯性滑动
				pullUpLoad: true, // 监听上拉加载更多,
				mouseWheel: true,  // 能使用鼠标滑动
				observeDOM: true, // 可能会出现不能滑动的情况
				click: true,
			})

			this.scroll.on('scroll', (position) => {
				this.$emit("scroll", position)
			})

			this.scroll.on('pullingUp', () => {
				this.$emit('pullingUp')
			})

		},
		methods: {
			scrollTo(x, y, time=300) {
				this.scroll.scrollTo(x, y, time)
			},
			finishPullUp() {
				this.scroll.finishPullUp()
			}
		}
	}
</script>

<style scoped>

</style>
