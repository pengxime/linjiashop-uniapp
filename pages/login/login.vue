<template>
	<view class="wrap">
		<view class="top"></view>
		<view class="content">
			<view class="title">欢迎登录邻家小铺</view>
			<u-form-item>
				<u-input type="number" v-model="tel" placeholder="请输入手机号" />
			</u-form-item> 
			<!-- <view class="tips">未注册的手机号验证后自动创建邻家小铺账号</view> -->
		 
			<button @tap="submit" :style="[inputStyle]" class="getSmsCode">获取短信验证码</button>
			<view class="alternative">
				<view class="password" @click="passwordLogin">密码登录</view>
				<view class="issue">遇到问题</view>
			</view>
		</view>
		<view class="bottom">
			<view class="loginType">
				<!-- #ifdef APP-PLUS -->
				<view class="wechat item" @click="loginBy('wechat')">
					<view class="icon">
						<u-icon size="70" name="weixin-fill" color="rgb(83,194,64)"></u-icon>
					</view>
					微信
				</view>
				<view class="QQ item" @click="loginBy('QQ')">
					<view class="icon">
						<u-icon size="70" name="qq-fill" color="rgb(17,183,233)"></u-icon>
					</view>
					QQ
				</view>
				<!-- #endif -->
			</view>
			<view class="hint">
				登录代表同意
				<text class="link" @click="goPage('/pages/login/userProtocol')">邻家小铺用户协议</text>、
				<text class="link" @click="goPage('/pages/login/privateProtocol')">隐私政策，</text>
				并授权使用您的邻家小铺账号信息（如昵称、头像、收获地址）以便您统一管理
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tel: '',
			}
		},
		computed: {
			inputStyle() {
				let style = {};
				if (this.tel && this.tel.length == 11 && this.tel.startsWith('1')) {
					style.color = "#fff";
					style.backgroundColor = this.$u.color['warning'];
				}
				return style;
			}
		},
		onLoad(){
		},
		methods: {
			submit() {
				if (!(this.tel && this.tel.length == 11 && this.tel.startsWith('1'))) {
					this.$u.toast('请输入正确手机号');
					return;
				}
				this.$u.post('sendSmsCode?mobile=' + this.tel).then(response => {
					this.$u.route({
						url: '/pages/login/smsCode',
						params: {
							mobile: this.tel,
							result: response
						}
					})
				})
			},
			loginBy(type) {
				this.$u.toast('第三方账号登录开发中，敬请期待')
			},
			goPage(url) {
				this.$u.route({
					url: url
				})
			},
			passwordLogin() {
				this.$u.route({
					url: '/pages/login/passwordLogin'
				})
			},

		}
	};
</script>

<style lang="scss" scoped>
	.wrap {
		font-size: 28rpx;

		.content {
			width: 600rpx;
			margin: 0 auto;
			padding-top: 80rpx;

			.title {
				text-align: left;
				font-size: 60rpx;
				font-weight: 500;
				margin-bottom: 100rpx;
			}

			input {
				text-align: left;
				margin-bottom: 10rpx;
				padding-bottom: 6rpx;
			}

			.tips {
				color: $u-type-info;
				font-size: 20rpx;
				margin-bottom: 60rpx;
				margin-top: 30rpx;
			}

			.getSmsCode {
				background-color: rgb(253, 243, 208);
				color: $u-tips-color;
				border: none;
				font-size: 30rpx;
				padding: 12rpx 0;

				&::after {
					border: none;
				}
			}

			.alternative {
				color: $u-tips-color;
				display: flex;
				justify-content: space-between;
				margin-top: 30rpx;
			}
		}

		.bottom {
			.loginType {
				display: flex;
				padding: 260rpx 150rpx 150rpx 150rpx;
				justify-content: space-between;

				.item {
					display: flex;
					flex-direction: column;
					align-items: center;
					color: $u-content-color;
					font-size: 28rpx;
				}
			}

			.hint {
				padding: 20rpx 40rpx;
				font-size: 20rpx;
				color: $u-tips-color;

				.link {
					color: $u-type-warning;
				}
			}
		}
	}
</style>
