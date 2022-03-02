<template>
	<view>
		<view class="addrerss-choose-box" v-if="JSON.stringify(address) === '{}'">
			<button type="primary" size="mini" class="btnChooseAddress" @click="chooseAddress">请选择收获地址 +</button>
		</view>
		<view class="address-info-box" v-else @click="chooseAddress">
			<view class="row1">
				<view class="row1-left">
					<view class="username">收货人： <text>{{address.userName}}</text></view>
				</view>
				<view class="row1-right">
					<view class="phone">电话： <text>{{address.telNumber}}</text></view>
					<uni-icons type="arrowright" size="16"></uni-icons>
				</view>
			</view>
			<view class="row2">
				<view class="row2-left">收货地址：</view>
				<view class="row2-right">{{addressStr}}</view>
			</view>
		</view>
		<image src="/static/cart_border@2x.png" class="address-border-bottom"></image>
	</view>
</template>

<script>
	import {mapState, mapMutations, mapGetters } from 'vuex'
	export default {
		name:"my-address",
		data() {
			return {
			};
		},
		methods: {
			...mapMutations('m_user',['updateAddress']),
			async chooseAddress() {
				const [err, succ] = await uni.chooseAddress().catch(err =>err)
					if(err === null && succ.errMsg === 'chooseAddress:ok') {
						this.updateAddress(succ)
					}
			}
		},
		computed: {
			...mapState('m_user', ['address']),
			...mapGetters('m_user', ['addressStr'])
		}
	}
</script>

<style lang="scss">
	.addrerss-choose-box {
		position: relative;
		height: 80px;
		button {
			position: absolute;
			top: 50%;
			left: 50%;
			transform: translate(-50%, -50%);
		}
	}
	.address-info-box {
		font-size: 12px;
		height: 90px;
		display: flex;
		flex-direction: column;
		justify-content: center;
		padding: 0 5px;
		.row1 {
			display: flex;
			justify-content: space-between;
			.row1-right {
				display: flex;
				justify-content: space-between;
				.phone {
					
				}
			}
		}
		.row2 {
			display: flex;
			align-items: center;
			margin-top: 10px;
			.row2-left {
				white-space: nowrap;
				// 文本不换行
			}
		}
	}
	.address-border-bottom {
		margin: 0;
		height: 5px;
		width: 100%;
	}
</style>
