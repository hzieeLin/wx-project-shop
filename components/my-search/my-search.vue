<template>
	<view class="my-search-container" :style="{'background-color': bgcolor}" @click="searchBoxHandle">
		<view class="my-search-box" :style="{'border-radius': radius + 'px'}">
			<uni-icons type="search" size="17"></uni-icons>
			<text>搜索</text>
		</view>
	</view>
</template>

<script>
	export default {
		name:"my-search",
		data() {
			return {
				
			};
		},
		props: {
			// 背景颜色
			bgcolor: {
				type: String,
				default: '#C00000'
			},
			// 圆角尺寸
			radius: {
				type:Number,
				default: 18
			}
		},
		methods: {
			searchBoxHandle() {
				this.$emit('click')
			}
		}
	}
</script>

<style lang="scss">
  .my-search-container {
	  height: 50px;
	  display: flex;
	  padding: 0 10px;
	  align-items: center;
	  .my-search-box {
		  background-color: #FFFFFF;
		  height: 36px;
		  width: 100%;
		  display: flex;
		  justify-content: center;
		  align-items: center;
		  text {
			  font-size: 15px;
			  margin-left: 5px;
		  }
	  }
  }
</style>
