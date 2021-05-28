<template>
	<tab-bar>
		<tab-bar-item v-for="item in tabBarItems" :path="item.path">
			<template v-slot:item-icon><img :src="item.src" alt=""></template>
			<template v-slot:item-icon-active><img :src="item.activeSrc" alt=""></template>
			<template v-slot:item-text><div >{{item.text}}</div></template>
		</tab-bar-item>
	</tab-bar>
</template>

<script>
	import TabBar from "components/common/tabbar/TabBar";
	import TabBarItem from "components/common/tabbar/TabBarItem";
	export default {
		name: "MainTabBar",
		components: {
			TabBar,
			TabBarItem
		},
		data() {
			return {"tabBarItems":
					[{path: '/home', src: require("assets/img/tabbar/home.svg"),
						activeSrc: require("assets/img/tabbar/home_active.svg"),  text: '首页'},
						{path: '/category', src: require("assets/img/tabbar/category.svg"),
							activeSrc: require("assets/img/tabbar/category_active.svg"), text: '分类'},
						{path: '/cart', src: require("assets/img/tabbar/shopcart.svg"),
							activeSrc: require("assets/img/tabbar/shopcart_active.svg"), text: '购物车'},
						{path: '/profile', src: require("assets/img/tabbar/profile.svg"),
							activeSrc: require("assets/img/tabbar/profile_active.svg"), text: '我的'}]}
		},
	}
</script>

<style scoped>

</style>
