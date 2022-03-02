<template>
	<view>
		<my-search @click="gotoSearch"></my-search>
		<view class="scroll-view-container">
			<scroll-view class="left-scroll-view" scroll-y="true"  :style="{height: wh + 'px'}">
				<block v-for="(item, index) in cateList" :key="index">
					<view :class="['left-scroll-view-item', index === active ? 'active': '']" @click="activeChange(index)">{{item.cat_name}}</view>
				</block>
			</scroll-view>
			<scroll-view scroll-y="true" :style="{height: wh + 'px'}" class="right-scroll-view" :scroll-top="scrollTop">
				<view class="cate-level2" v-for="(item2, index2) in cateLevel2" :key="index2">
					<!-- 二级分类的标题 -->
					<view class="cate-level2-title">{{item2.cat_name}}</view>
					<!-- 三级分类列表 -->
					<view class="cate-level3-list">
						<view class="cate-level3-item" v-for="(item3, index3) in item2.children" :key="index3">
							<image :src="item3.cat_icon"></image>
							<text>{{item3.cat_name}}</text>
						</view>
					</view>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	import badgeMix from '@/mixins/tabbar-badge.js'
	export default {
		mixins: [badgeMix],
		data() {
			return {
				wh: 0,
				cateList:[],
				active: 0,
				// 二级分类列表
				cateLevel2:[],
				// 滚动条的高度
				scrollTop: 0
			};
		},
		onLoad() {
			const sysInfo = uni.getSystemInfoSync()
			this.wh = sysInfo.windowHeight - 50
			this.getCateList()
		},
		methods: {
			async getCateList() {
				const {data: res} = await uni.$http.get('/api/public/v1/categories');
				if(res.meta.status !== 200) return uni.$showMsg()
				this.cateList = res.message
				this.cateLevel2 = res.message[0].children
			},
			activeChange(index) {
				this.active = index
				this.cateLevel2 = this.cateList[index].children
				this.scrollTop = this.scrollTop === 0 ? 1 : 0
			},
			gotoSearch() {
				uni.navigateTo({
					url: '/subpkg/search/search'
				})
			}
		}
	}
</script>

<style lang="scss">
	.scroll-view-container {
		display: flex;
		.left-scroll-view {
			width: 120px;
		}
	}
	.left-scroll-view-item {
		background-color: #F7F7F7;
		line-height: 60px;
		text-align: center;
		font-size: 12px;
		&.active {
			background-color: #FFFFFF;
			position: relative;
			&::before {
				content: " ";
				display: block;
				width: 3px;
				height: 30px;
				background-color: #c00000;
				position: absolute;
				top: 50%;
				left: 0;
				transform: translateY(-50%);
			}
		}
	}
	.right-scroll-view {
		background-color: #FFFFFF;
	}
	.cate-level2-title {
		font-size: 12px;
		font-weight: bold;
		text-align: center;
		padding: 15px 0;
	}
	.cate-level3-list {
		display: flex;
		flex-wrap: wrap;
		.cate-level3-item {
			width: 33.3%;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			margin-bottom: 10px;
			image {
				width: 60px;
				height: 60px;
			}
			text {
				font-size: 12px;
			}
		}
	}
</style>
