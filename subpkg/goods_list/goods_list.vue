<template>
	<view>
		<view class="goods-list">
			<view v-for="(item, index) in goodsList" :key="index" @click="gotoDetail(item)">
				<my-goods :item="item"></my-goods>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				queryObj: {
					query: '',
					cid: '',
					pagenum: 1,
					pagesize: 10
				},
				goodsList:[],
				total: 0,
				isloading: false
				
			};
		},
		onLoad(options) {
			this.queryObj.query = options.query || ''
			this.queryObj.cid = options.cid || ''
			this.getGoodsList()
		},
		methods: {
			async getGoodsList(cb) {
				this.isloading = true
				const {data: res} = await uni.$http.get('/api/public/v1/goods/search', this.queryObj)
				this.isloading = false
				cb && cb()
				if(res.meta.status !== 200) {
					return uni.$showMsg()
				}
				this.goodsList = [...this.goodsList, ... res.message.goods]
				this.total = res.message.total
			},
		gotoDetail(goods) {
			uni.navigateTo({
				url: '/subpkg/goods_detail/goods_detail?goods_id='+goods.goods_id
			})
		}
		},
		// 上拉加载
		onReachBottom() {
			// 判断是否真正请求，是，则不发起请求
			if(this.queryObj.pagenum * this.queryObj.pagesize >= this.total) return uni.$showMsg('数据加载完毕！')
			if(this.isloading) return
			// 页码值加1
			this.queryObj.pagenum++
			console.log(this.queryObj.pagenum);
			this.getGoodsList()
		},
		//下拉刷新
		onPullDownRefresh() {
			// 重置数据
			this.queryObj.pagenum = 1
			this.total = 0
			this.isloading = false
			this.goodsList = []
			this.getGoodsList(() => uni.stopPullDownRefresh())
		}
	}
</script>

<style lang="scss">
	
</style>
