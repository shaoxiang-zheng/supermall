<template>
	<div id="home">
		<nav-bar class="home-nav">
			<template v-slot:center><div>购物街</div></template>
		</nav-bar>

		<scroll class="content"
						ref="scroll"
						:probe-type="3"
						@scroll="contentScroll"
						:pull-up-load="true"
						@pullingUp="loadMore">
			<home-swiper :banners="banners"></home-swiper>
			<recommend-view :recommends="recommends"></recommend-view>
			<feature-view></feature-view>
			<tab-control class="tab-control"
									 :titles="['流行', '新款', '精选']" @tabClick="tabClick"></tab-control>
			<goods-list :goods="showGoods"></goods-list>
		</scroll>

		<back-top @click="backClick" v-show="isShowBackTop"></back-top>
	</div>

</template>

<script>
	import HomeSwiper from "./childComps/HomeSwiper";
	import RecommendView from "./childComps/RecommendView";
	import FeatureView from "./childComps/FeatureView";

	import NavBar from "components/common/navbar/NavBar";

	import TabControl from "components/content/tabControl/TabControl";
	import GoodsList from "../../components/content/goods/GoodsList";

	import {getHomeMultiData, getHomeGoods} from "network/home";

	import Scroll from "components/common/scroll/Scroll";
	import BackTop from "components/content/backTop/BackTop";

	export default {
		name: "Home",
		components: {
			NavBar,
			HomeSwiper,
			RecommendView,
			FeatureView,
			TabControl,
			GoodsList,
			Scroll,
			BackTop
		},
		data() {
			return {
				banners: [],
				recommends: [],
				goods: {
					pop: {page: 0, list: []},
					new: {page: 0, list: []},
					sell: {page: 0, list: []},
				},
				currentType: 'pop',
				isShowBackTop: false,
			}
		},
		computed: {
			showGoods() {
				return this.goods[this.currentType].list
			}
		},
		created() {
			this.getHomeMultiData()  //

			this.getHomeGoods('pop')
			this.getHomeGoods('new')
			this.getHomeGoods('sell')
		},
		methods: {
			backClick() {
				this.$refs.scroll.scrollTo(0, 0, 500)
			},
			loadMore() {
				this.getHomeGoods(this.currentType)

				this.$refs.scroll.scroll.refresh()  // 图片加载卡顿
			},
			contentScroll(position) {
				this.isShowBackTop = (-position.y) > 1000
			},
			tabClick(index) {
				switch (index) {
					case 0:
						this.currentType = 'pop'
						break
					case 1:
						this.currentType = 'new'
						break
					case 2:
						this.currentType = 'sell'
						break
				}
			},
			getHomeMultiData() {
				getHomeMultiData().then(res => { // 异步操作
					this.banners = res.data.banner.list;
					this.recommends = res.data.recommend.list
				})
			},
			getHomeGoods(type) {
				const page = this.goods[type].page + 1;
				getHomeGoods(type, page).then(res => {
					this.goods[type].page = page;
					this.goods[type].list.push(...res.data.list);

					this.$refs.scroll.finishPullUp()
				})
			},
		}
	}
</script>

<style scoped>
	#home {
		padding-top: 44px;
		height: 100vh;
		position: relative;
	}

	.home-nav {
		background-color: var(--color-tint);
		color: #f6f6f6;

		position: fixed;
		left: 0;
		right: 0;
		top: 0;
		z-index: 9;
	}

	.tab-control {
		/*position: sticky;*/
		top: 44px;
		z-index: 9;
		/*background-color: #f6f6f6;*/
	}

	.content {
		overflow: hidden;
		position: absolute;
		top: 44px;
		bottom: 49px;
	}
</style>
