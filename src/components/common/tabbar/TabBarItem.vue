<template>
	<div class="tab-bar-item" @click="itemClick">
		<div v-if="!isActive"><slot name="item-icon"></slot></div>
		<div v-else><slot name="item-icon-active"></slot></div>

		<div :style="activeStyle"><slot name="item-text"></slot></div>

	</div>
</template>

<script>
	export default {
		name: "TabBarItem",
		props: {
			path: String,
			activeColor: {
				type: String,
				default: 'red'
			}
		},
		computed: {
			isActive() {
				// 判断当前活跃路由路径与该路径是否相同
				// 实质上应该是匹配字符串 e.g. /home home
				return this.$route.path.indexOf(this.path) !== -1
			},
			activeStyle() {
				return this.isActive? {color: this.activeColor} : {}
			}
		},
		methods: {
			itemClick() {
				this.$router.replace(this.path)
			}
		}
	}
</script>

<style scoped>
	.tab-bar-item {
		flex: 1;
		text-align: center;
		height: 49px;
		font-size: 14px;
	}

</style>
