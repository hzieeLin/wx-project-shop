<template>
	<view>
		<view class="search-box">
			<uni-search-bar @input="input" :radius="18" cancelButton="null"></uni-search-bar>
		</view>
		<view class="sugg-list">
			<view class="sugg-item" v-for="(item, index) in searchResults" :key="index" @click="gotoDetail(item.goods_id)">
				<view class="goods-name" >{{item.goods_name}}</view>
				<uni-icons type="arrowright" size="16"></uni-icons>
			</view>
		</view>
		<view class="history-box">
			<view class="history-title">
				<text>搜索历史</text>
				<uni-icons type="trash" size="17" @click="clean"></uni-icons>
			</view>
			<view class="history-list">
				<uni-tag :text="item" v-for="(item, index) in historyList" :key="index" @click="gotoGoodsList(item)"></uni-tag>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				timer: null,
				// kw是关键词
				kw: '',
				// searchResults是指搜索的结果列表
				searchResults:[],
				// historyList历史记录
				historyList:[]
			};
		},
		onLoad() {
			this.historyList = JSON.parse(uni.getStorageSync('kw') || '[]')
		},
		methods: {
			input(e) {
				clearTimeout(this.timer)
				this.timer = setTimeout(() => {
					this.kw = e
					this.getSearchList()
				}, 500)
			},
			async getSearchList() {
				if(this.kw.length === 0) {
					this.searchResults = []
					return
				}else {
					const {data: res } = await uni.$http.get('/api/public/v1/goods/qsearch', { query: this.kw })
					if(res.meta.status !== 200) return uni.$showMsg()
					this.searchResults = res.message
					this.saveSearchHistory()
				}
			},
			gotoDetail(goods_id) {
				uni.navigateTo({
					url: '/subpkg/goods_detail/goods_detail?goods_id'+goods_id
				})
			},
			saveSearchHistory() {
				const set = new Set(this.historyList)
				// 去重
				set.delete(this.kw)
				set.add(this.kw)
				// 赋值
				this.historyList = Array.from(set)
				// 对数据持久化存储
				uni.setStorageSync('kw', JSON.stringify(this.historyList))
			},
			clean() {
				this.historyList = []
				uni.setStorageSync('kw', '[]')
			},
			gotoGoodsList(kw) {
				console.log(kw)
				uni.navigateTo({
					url: '/subpkg/goods_list/goods_list?query='+kw
				})
			}
		}
	}
</script>

<style lang="scss">
  .search-box {
	  position: sticky;
	  top: 0;
	  z-index: 99;
  }
  .sugg-list {
	  padding: 0 5px;
	  .sugg-item {
		  font-size: 12px;
		  padding: 13px 0;
		  border-bottom: 1px solid #efefef;
		  display: flex;
		  align-items: center;
		  justify-content: space-around;
		  .goods-name {
			  white-space: nowrap;
			  // 隐藏溢出的文字
			  overflow: hidden;
			  // 把溢出的文字用...代替
			  text-overflow: ellipsis;
			  margin-right: 3px;
		  }
	  }
  }
  .history-box {
	  padding: 0 5px;
	  .history-title {
		  display: flex;
		  justify-content: space-between;
		  height: 40px;
		  align-items: center;
		  font-size: 13px;
		  border-bottom: 1px solid #efefef;
	  }
	  .history-list {
		  display: flex;
		  flex-wrap: wrap;
		  .uni-tag {
			  margin-top: 5px;
			  margin-right: 5px;
		  }
	  }
  }
</style>
