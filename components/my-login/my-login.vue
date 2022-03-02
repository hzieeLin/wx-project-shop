<template>
	<view>
		<view class="login-container">
			<uni-icons type="contact-filled" size="100" color="#AFAFAF"></uni-icons>
			<button type="primary" class="btn-login"  @click="getUserProfile">一键登录</button>
			<view class="tips text">登录后尽更多权益</view>
		</view>
	</view>
</template>
<script>
	import { mapMutations } from 'vuex'
	export default {
		name:"my-login",
		data() {
			return {
				
			};
		},
		methods: {
			...mapMutations('m_user', ['updateUserInfo','updateToken']),
			getUserProfile() {
			uni.getUserProfile({
				desc: '你的授权信息',
				success: (res) => {
						console.log(res);
						this.updateUserInfo(res.userInfo)
						this.getToken(res)
				
				},
				fail:(res) => {
					return uni.$showMsg('您取消了登录授权！')
				}
			})
			},
			async getToken(info) {
				const [err, res] = await uni.login().catch(err => err)
				console.log(res.errMsg);
				if(err || res.errMsg !== 'login:ok') return uni.$showMsg('登录失败')
				console.log("wx"+res.code);
				const query = {
					encryptedData: info.encryptedData,
					rawData: info.rawData,
					iv: info.iv,
					signature: info.signature,
					code: res.code,
				}
				//console.log(query);
				const { data: loginResult } = await uni.$http.post('/api/public/v1/users/wxlogin', query)
				console.log(loginResult);
				// if(loginResult.meta.status !== 200) return uni.$showMsg('登录失败！')
				// uni.$showMsg('登录成功')
				// this.updateToken(loginResult.message.token)
			}
		}
	}
</script>

<style lang="scss">
	.login-container {
		// 登录盒子的样式
		height: 750rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		background-color: #F8f8f8;
		position: relative;
		overflow: hidden;
		&::after {
			content: ' ';
			display: block;
			position: absolute;
			width: 100%;
			height: 40px;
			left: 0;
			bottom: 0;
			background-color: #F8f8f8;
			border-radius: 100%;
			transform: translateY(50%);
		}
		.btn-login {
			width: 90%;
			border-radius: 100px;
			margin: 15px 0;
			background-color: #C00000;
		}
		.tips-text {
			font-size: 12px;
			color: gray;
		}
	
	}
</style>
