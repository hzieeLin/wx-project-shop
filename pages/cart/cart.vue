<template>
	<view class="cart-container" v-if="cart.length !== 0">
		<my-address></my-address>
		<view class="cart-title">
			<uni-icons type="shop" size="18"></uni-icons>
			<text class="cart-title-text">购物车</text>
		</view>
		
			
		<uni-swipe-action>
			<view v-for="(item, index) in cart" :key="index">
			<uni-swipe-action-item :rightOptions="options" @click="deleteGoodsListItem(item)">
				<my-goods :item="item"
						  :show-radio="true" 
						  @radio-change="radioChangeHandle" 
						  :show-num="true"
						  @num-change="changeGoodsCount"></my-goods>
			</uni-swipe-action-item>
			</view>
		</uni-swipe-action>
		<my-settle></my-settle>
	</view>
	<view class="empty-cart" v-else>
		<!-- 空白购物车页面 -->
		<image src="/static/tab_icons/cart-active.png" class="empty-img"></image>
		<text class="text-tip">空空如也~~</text>
	</view>
</template>

<script>
	import {mapState, mapMutations} from 'vuex'
	import badgeMix from '@/mixins/tabbar-badge.js'
	export default {
		mixins: [badgeMix],
		computed: {
			...mapState('m_cart',['cart'])
		},
		data() {
			return {
				options:[{
					text: '删除',
					style: {
						backgroundColor: '#C00000'
					}
				}]
			};
		},
		methods: {
			...mapMutations('m_cart', ['updateGoodsState','updateGoodsCount','removeGoodsById']),
			radioChangeHandle(e) {
				this.updateGoodsState(e)
			},
			changeGoodsCount(e) {
				this.updateGoodsCount(e)
			},
			deleteGoodsListItem(goods) {
				this.removeGoodsById(goods.goods_id)
			},
			
		}
	}
</script>

<style lang="scss">
	.cart-container {
		padding-bottom: 50px;
	}
	.cart-title {
		height: 40px;
		display: flex;
		align-items: center;
		font-size: 14px;
		padding-left: 5px;
		border-bottom: 1px solid #efefef;
		.cart-title-text {
			margin-left: 10px;
		}
	}
	.empty-cart {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding-top: 150px;
		.empty-img {
			width: 90px;
			height: 90px;
		}
		.text-tip {
			font-size: 12px;
			color: gray;
			margin-top: 15px;
		}
	}
</style>
