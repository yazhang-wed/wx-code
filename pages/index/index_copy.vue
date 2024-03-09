<template>
	<view class="model">
		<view class="left-model">
			<view class="navTitle" v-for="(item,index) in navList" :key="index">
				<view :class="{'active':isActive === index}" @click="checked(index)">
					{{item.title}}
				</view>
			</view>
		</view>
		<view class="right-model">
			<view class="imgage-model" v-for="(item,index) in childList" :key="index" @click="checkedImage(item.url)">
				<view class="title-code">{{item.title}}</view>
				<image class="imgage-code" mode="aspectFit" :src="item.url"></image>
			</view>
		</view>
		<view class="hidden-model" v-show="showView">
			<view class="delete-button" @click="checkedHidden">
				X
			</view>
			<view class="imgage-div">
				<image class="imgage" mode="aspectFit" :src="selecedUrl" show-menu-by-longpress="true">
				</image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isActive: 0,
				navList: [{
						title: '广东',
						child: [{
							url: '/static/logo.png',
							title: '广东测试1'
						}, {
							url: '/static/logo.png',
							title: '广东测试2'
						}, {
							url: '/static/logo.png',
							title: '广东测试3'
						}, {
							url: '/static/logo.png',
							title: '广东测试4'
						}, {
							url: '/static/logo.png',
							title: '广东测试5'
						}, {
							url: '/static/logo.png',
							title: '广东测试6'
						}]
					},
					{
						title: "湖南",
						child: [{
							url: '/static/logo.png',
							title: '湖南测试1'
						}, {
							url: '/static/logo.png',
							title: '湖南测试2'
						}, {
							url: '/static/logo.png',
							title: '湖南测试3'
						}, {
							url: '/static/logo.png',
							title: '湖南测试4'
						}, {
							url: '/static/logo.png',
							title: '湖南测试5'
						}, {
							url: '/static/logo.png',
							title: '湖南测试6'
						}]
					},
					{
						title: "福建"
					}
				],
				selecedUrl: null,
				showView: false
			}
		},
		computed: {
			childList: function() {
				return this.navList[this.isActive].child || []
			},
		},
		methods: {
			checked(index) {
				this.isActive = index
			},
			checkedImage(url) {
				this.showView = true
				this.selecedUrl = url
			},
			checkedHidden() {
				this.showView = false
			},
			// 点击图片展示全屏
			previewImage: function(event) {
				var currentUrl = event.currentTarget.dataset.src; // 获取当前点击的图片路径
			},
			// 长按图片处理
			handleLongPress: function(event) {
				var currentUrl = event.currentTarget.dataset.src; // 获取当前长按的图片路径
				// 在这里可以执行长按图片的操作，比如保存图片或者识别二维码
				// 这里只是一个简单示例，你可以根据具体需求来实现
				wx.showActionSheet({
					itemList: ['保存图片', '识别二维码'],
					success: function(res) {
						if (res.tapIndex === 0) {
							// 保存图片
							wx.saveImageToPhotosAlbum({
								filePath: '/static/logo.png',
								success(res) {
									wx.showToast({
										title: '保存成功',
										icon: 'success',
										duration: 2000
									})
								},
								fail(res) {
									wx.showToast({
										title: '保存失败',
										icon: 'none',
										duration: 2000
									})
								}
							})
						} else if (res.tapIndex === 1) {
							// 识别二维码
							wx.scanCode({
								success(res) {
									console.log('二维码内容：', res.result)

									// // 获取到二维码中的信息
									// const groupJoinCode = res.result;

									// // 使用群的加入码跳转到微信的加群页面
									// wx.navigateToMiniProgram({
									// 	appId: 'wx3670d6a21a6bf940', // 微信官方提供的加群页面的小程序的 appid
									// 	path: 'pages/join/join?inviteCode=' +groupJoinCode,
									// 	// 通过 path 参数传递群的加入码
									// 	extraData: {
									// 		// 额外的数据，可选
									// 	},
									// 	envVersion: 'develop', // 发布的版本，可以选择 release 或 develop
									// 	success(res) {
									// 		// 成功跳转到加群页面的回调函数
									// 		console.log(res);
									// 	},
									// 	fail(err) {
									// 		// 跳转失败的回调函数
									// 		console.error(err);
									// 	}
									// });
								},
								fail(res) {
									wx.showToast({
										title: '识别失败',
										icon: 'none',
										duration: 2000
									})
								}
							})
						}
					}
				})
			}
		}
	}
</script>

<style>
	.model {
		width: 100%;
		height: 100vh;
		display: flex;
	}

	.model .left-model {
		width: 70px;
		height: 100%;
		overflow-y: auto;
		overflow-x: hidden;
	}

	.model .left-model .navTitle {
		height: 90rpx;
		line-height: 90rpx;
		width: 100%;
		text-align: center;
		font-size: 32rpx;
		font-family: Alibaba PuHuiTi;
		color: #333;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		padding: 0 10px;
		box-sizing: border-box;
	}

	.model .left-model .navTitle .active {
		position: relative;
		color: #333;
	}

	.model .left-model .navTitle .active::after {
		content: "";
		position: absolute;
		width: 100rpx;
		height: 8rpx;
		background-color: #FFC125;
		left: 0px;
		right: 0px;
		bottom: 0px;
		margin: auto;
	}

	.model .right-model {
		width: calc(100% - 70px);
		height: 100%;
		overflow-y: auto;
		background-color: #f0f0f0;
	}

	.model .right-model .imgage-model {
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 15px 0;
		margin-bottom: 20px;
		background-color: #fff;
	}

	.model .right-model .imgage-model:last-child {
		margin-bottom: 0;
	}

	.model .right-model .imgage-model .imgage-code {
		object-fit: contain;
		width: 100%;
		box-sizing: border-box;
		padding: 0 20px;
		height: 140px;
	}

	.model .right-model .imgage-model .title-code {
		height: 36px;
		line-height: 36px;
		width: 100%;
		text-align: center;
		font-size: 18px;
		font-family: Alibaba PuHuiTi;
		color: #333;
	}

	.hidden-model {
		/* display: none; */
		position: absolute;
		left: 0;
		top: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(2, 2, 2, 0.2);
	}

	.hidden-model .delete-button {
		position: absolute;
		right: 0;
		top: 0;
		width: 20px;
		height: 20px;
		border-radius: 50%;
		background-color: rgba(0, 0, 0, 0.7);
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 15px;
		color: #fff;
		cursor: pointer;
		box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);

	}

	.hidden-model .delete-button:hover {
		background-color: rgb(0, 0, 0);
	}

	.hidden-model .imgage-div {
		background-color: #fff;
		height: 60%;
		width: 100%;
		position: absolute;
		top: 0;
		bottom: 0;
		left: 0;
		right: 0;
		margin: auto;
		padding: 20px;
		box-sizing: border-box;
	}

	.hidden-model .imgage-div .imgage {
		width: 100%;
		height: 100%;
	}
</style>