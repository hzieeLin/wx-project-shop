<template>
	<view class="goods-item">
		<view class="goods-item-left">
			<image :src="item.goods_small_logo || defaltPic" class="goods-pic"></image>
		</view>
		<view class="goods-item-right">
			<view class="goods-name">{{item.goods_name}}</view>
			<view class="goods-info-box">
				<view class="goods-price">￥{{item.goods_price | tofixed}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			item: {
				type:Object,
				default: {}
			}	
		},
		data() {
			return {
				// 默认图片，当图片不存在时候
				defaltPic: 'https://img2.baidu.com/it/u=59285992,513800291&fm=26&fmt=auto'
			};
		},
		filters: {
			tofixed(num) {
				return Number(num).toFixed(2)
			}
		}
	}
</script>

<style lang="scss">
	.goods-item {
			display: flex;
			padding: 10px 5px;
			border-bottom: 1px solid #f0f0f0;
			.goods-item-left {
				.goods-pic {
					width: 100px;
					height: 100px;
					display: block;
					margin-right: 5px;
				}
			}
			.goods-item-right {
				display: flex;
				flex-direction: column;
				justify-content: space-between;
				.goods-name {
					font-size: 13px;
				}
				.goods-info-box {
					.goods-price {
						color: #C00000;
						font-size: 16px;
					}
				}
			}
		}
</style>
