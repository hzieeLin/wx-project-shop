<template>
	<view class="goods-item">
		<view class="goods-item-left">
			<radio :checked="item.goods_state" color="#C00000" v-if="showRadio" @click="radioChangeHandle"></radio>
			<image :src="item.goods_small_logo || defaltPic" class="goods-pic"></image>
		</view>
		<view class="goods-item-right">
			<view class="goods-name">{{item.goods_name}}</view>
			<view class="goods-info-box">
				<view class="goods-price">￥{{item.goods_price | tofixed}}</view>
				<uni-number-box :min="1" :value="item.goods_count" v-if="showNum" @change="changeGoodsCount"></uni-number-box>
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
			},
			showRadio: {
				type: Boolean,
				default: false
			},
			showNum: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return {
				// 默认图片，当图片不存在时候
				defaltPic: 'https://img2.baidu.com/it/u=59285992,513800291&fm=26&fmt=auto'
			};
		},
		methods: {
				radioChangeHandle(e) {
					this.$emit('radio-change', {
						goods_id: this.item.goods_id,
						goods_state: !this.item.goods_state
					})
			},
			changeGoodsCount(val) {
				this.$emit('num-change', {
					goods_id: this.item.goods_id,
					goods_count: val - 0
				})
			}
		},
		filters: {
			tofixed(num) {
				return Number(num).toFixed(2)
			}
		},
	}
</script>

<style lang="scss">
	.goods-item {
			width: 750rpx;
			box-sizing: border-box;
			display: flex;
			padding: 10px 5px;
			border-bottom: 1px solid #f0f0f0;
			.goods-item-left {
				margin-right: 5px;
				display: flex;
				justify-content: space-between;
				align-items: center;
				.goods-pic {
					width: 100px;
					height: 100px;
					display: block;
				}
			}
			.goods-item-right {
				display: flex;
				flex: 1;
				flex-direction: column;
				justify-content: space-between;
				.goods-name {
					font-size: 13px;
				}
				.goods-info-box {
					display: flex;
					justify-content: space-between;
					align-items: center;
					.goods-price {
						color: #C00000;
						font-size: 16px;
					}
				}
			}
		}
</style>
