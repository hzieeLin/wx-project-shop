<template>
	<view class="my-settle-container">
		<label class="radio" @click="changeAllState">
			<radio color="#C00000" :checked="isFullCheck"/><text>全选</text>
		</label>
		
		<view class="amount-box">
			合计<text class="amount">￥{{chechedGoodsAmount}}</text>
		</view>
		
		<view class="btn-settle" @click="settlement">结算({{checkedCount}})</view>
	</view>
</template>

<script>
	import {mapGetters, mapMutations, mapState} from 'vuex'
	export default {
		name:"my-settle",
		data() {
			return {
				
			};
		},
		computed: {
			...mapGetters('m_cart', ['checkedCount','total','chechedGoodsAmount']),
			...mapGetters('m_user', ['addressStr']),
			...mapState('m_user', ['token']),
			isFullCheck() {
				//console.log("total"+this.total);
				//console.log(this.checkedCount);
				return this.total === this.checkedCount
			}
		},
		methods: {
			...mapMutations('m_cart', ['updateAllGoodsState']),
			changeAllState() {
				this.updateAllGoodsState(!this.isFullCheck)
			},
			settlement() {
				// 先判断是否勾选了需要的商品
				if(!this.checkedCount) return uni.$showMsg('请选择结算的商品！')
				if(!this.addressStr) return uni.$showMsg('请选择结算的商品！')
				if(!this.token) return uni.$showMsg('请先登录！')
			}
		}
	}
</script>

<style lang="scss">
	.my-settle-container {
		position: fixed;
		left: 0;
		bottom: 0;
		height: 50px;
		width: 100%;
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 14px;
		padding-left: 5px;
		border-top: 1px solid #757575;
		.radio {
			display: flex;
			align-items: center;
		}
		.amount-box {
			.amount {
				color: #C00000;
				font-weight: bold;
			}
		}
		.btn-settle {
			background-color: #c00000;
			height: 50px;
			color: #FFFFFF;
			line-height: 50px;
			padding: 0 10px;
			min-width: 100px;
			text-align: center;
		}
	}
</style>
